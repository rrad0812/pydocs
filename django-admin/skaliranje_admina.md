# Kako skalirati Django admin

## Sadržaj

[Nazad](sadrzaj.md)

- [Evidentiranje](#evidentiranje)
- [N + 1 problem](#n--1-problem)
- [Nadjačavanje get_queryset](#nadjačavanje-get_queryset)
- [Poboljšani search sa DjangoQL](#poboljšani-search-sa-djangoql)
- [Elminisanje COUNT upita](#elminisanje-count-upita)
- [show_full_result_count](#show_full_result_count)
- [deffer](#deffer)
- [Paginator](#paginator)
- [readonly_fields](#readonly_fields)
- [Skaliranje admin date_hierarchy](#skaliranje-admin-date_hierarchy)
  - [Paket django-admin-lightweight-date-hierarchy](#paket-django-admin-lightweight-date-hierarchy)
  - [date_hierarchy](#date_hierarchy)

Django pruža snažan I fleksibilan admin interfejs. Medjutim kako tabele bivaju punije sa podacima, počinje usporavanje odgovora pri pretrazi. Sa nekom jednostavnim prilagodjenjima moguće je nastaviti sa korišćenjemDjango admina čak i sa velikim dataset-ovima.

[Sadržaj](#sadržaj)

### Evidentiranje

Većina Djangovog rada obavlja SQL upite, tako da će naš glavni fokus biti na smanjenju količine upita. Da biste pratili izvršavanje upita, možete da koristite jedno od sledećeg:

- django-debug-toolbar
- možete prijaviti SQL upite na konzolu dodavanjem sledećeg u settings.py.

`settings.py`

```py
LOGGING = {
  ...
  'loggers': {
    'django.db.backends':
    { 'level': 'DEBUG',},
  },
...
}
```

[Sadržaj](#sadržaj)

### N + 1 problem

N + 1 problem je dobro poznat problem u ORM-ima. Da bismo ilustrovali problem, recimo da imamo ovu šemu:

```py
class Category(models.Model):
    name = models.CharField(max_length=50)

    def str(self):
        return self.name

class Product(models.Model):
    name = models.CharField(max_length=50)
    category = models.ForeignKey(Category)

Implementacijom `__str__` kažemo Django-u da želimo da se naziv kategorije koristi kao opis objekta.

Kad god ispisujemo objekt kategorije, Django će dohvatiti ime kategorije. Kod za
admin stranu:

class Category(models.Model):
    name = models.CharField(max_length=50)

    def str (self):
        return self.name

class Product(models.Model):
    name = models.CharField(max_length=50)
    category = models.ForeignKey(Category)
```

Ovo se čini nedužnim, ali SQL dnevnik otkriva užas:

```sql
SELECT COUNT(*) AS count 
  FROM app_product

SELECT app_product.id, app_product.name, app_product.category_id
  FROM app_product
  ORDER BY app_product.id
  DESC LIMIT 100;

SELECT ... FROM app_category where app_category.id = 1;
SELECT ... FROM app_category where app_category.id = 2;
SELECT ... FROM app_category where app_category.id = 1;
SELECT ... FROM app_category where app_category.id = 4;
SELECT ... FROM app_category where app_category.id = 3;
...
SELECT ... FROM app_category where app_category.id = 2;
SELECT ... FROM app_category where app_category.id = 99;
SELECT ... FROM app_category where app_category.id = 104;
```

Django prvo broji objekte (o tome više kasnije), zatim preuzima stvarne objekte (ograničavajući se na podrazumevanu veličinu stranice od 100), a zatim prosleđuje podatke u obrazac za prikazivanje.

Ime kategorije smo koristili kao opis objekta "category", tako da za svaki proizvod Django mora da preuzme "name" kategorije. Rezultat je dodatnih 100 upita.

Da bismo rekli Djangu da želimo da izvršimo pridruživanje umesto da preuzmemo imena kategorija jednu po jednu, možemo koristiti `list_select_related`:

```py
@admin.register(models.Product)
class ProductAdmin(admin.ModelAdmin):
    list_display = ( 'id', 'name', 'category', )
    list_select_related = ('category',)
```

Sada SQL dnevnik izgleda mnogo lepše. Umesto 101 upita imamo samo 1:

```sql
SELECT app_product.id, app_product.name, app_product.category_id,
    app_category.id, app_category.name
  FROM app_product
  INNER JOIN app_category ON (app_product.category_id = app_category.id)
  ORDER BY app_product.id
  DESC LIMIT 100;
```

Da biste razumeli stvarni uticaj ove postavke, uzmite u obzir sledeće.

Podrazumevana veličina stranice za Django je 100 objekata. Ako imate jedno povezano polje, imate ~ 101 upit. Ako su u prikazu liste prikazana dva povezana objekta, imate ~ 201 upit i tako dalje. Preuzimanje povezanih polja u relaciji može raditi samo za veze `ForeignKey`. Ako želite prikazati relacije `ManyToMany`, malo je složenije (i najčešće pogrešno), ali nastavite čitati.

[Sadržaj](#sadržaj)

### Nadjačavanje get_queryset

Za strane ključeve i izračunata polja nema potrebe za pokretanjem dodatnih upita po vrsti. Izvršavanje višestrukih upita za svaki objekat može dovesti do značajnog usporavanja. Možemo prepisati `get_queryset` metod u `ModelAdmin` klasi, i koristiti `select_releated` metod na poljima stranih ključeva:

```py
class MyModelAdmin(ModelAdmin):

    def get_queryset(self, request):
        queryset = super().get_queryset(request)
        return queryset.select_related('foreign_key_1', 'foreign_key_2')
```

ili koristiti annotacije da smanjimo broj upita:

```py
class MyModelAdmin(ModelAdmin):
    
    def get_queryset(self, request):
        queryset = super().get_queryset(request)
        return queryset.annotate(field_count=Count('field'))
```

[Sadržaj](#sadržaj)

### Poboljšani search sa DjangoQL

Standardni search radi dobro ali posle ulaska u sferu miliona zapisa, search postaje problem. Kada je više od jednog polja u `search_fields`, SQL upit ima višestruke `JOIN` komande ulančane jedna na drugu. Rešenje može biti uvodjenje `DjangoQL` paketa.

`DjangoQLSearchMixin` menja standardni Django `search` sa `DjangoQL search`-om. DjangoQL pomaže nam da radimo `search` kroz specifikovana polja, uključujući povezana. Sve što je potrebno je dodavanje `DjangoQLSearchMixin` na `ModelAdmin`:

```py
from djangoql.admin import DjangoQLSearchMixin

class MyModelAdmin(DjangoQLSearchMixin, ModelAdmin):
    pass
```

DjangoQL autokompletira polja modela, podržava logičke operatore, zagrade i povezivanje tabela. Možete dobiti višestruke JOIN komande sa samo nekoliko linija koda i fokus na ono što pretražujete bez ograničenja kao u tvrdo kodovanom `search_fields`.

[Sadržaj](#sadržaj)

### Elminisanje COUNT upita

Posle dostizanja odredjene veličine, cena `COUNT(*)` upita postaje značajna na `change_list` i izaziva timeout greške. MySQL je naš go-to izbor i mi koristimo `Django-MySQL` paket da proširimo Djangovu ugradjenu podršku za MySQL. Paket dolazi sa korisnom `count_tries_approx` metodom koja vraća aproksimativni broj umesto tačnog broja zapisa. Na taj način eliminišemo puni sken na InnoDB i postižemo značjno brži rezultat.

```py
class MyModelAdmin(ModelAdmin):

    def get_queryset(self, request):
        qs = super().get_queryset(request)
        return qs.count_tries_approx()
```

Pre:

```sql
SELECT COUNT(*) AS ` count` FROM `mytable`
```

Posle: `count_tries_approx`:

```sql
SELECT TABLE_ROWS
  FROM INFORMATION_SCHEMA.TABLES
  WHERE TABLE_SCHEMA = DATABASE() AND TABLE_NAME = 'mytable'
```

[Sadržaj](#sadržaj)

### show_full_result_count

Sprečite Django da prikazuje ukupan broj redova u prikazu liste. Postavljanjem `show_full_result_count = False` uklanjate upit `count(*)` na queryset-u pri svakom učitavanju stranice.

### deffer

Prilikom izvršavanja upita, čitav skup rezultata se stavlja u memoriju radi obrade. Ako u svom modelu imate velike kolone kao što su JSON ili tekstualna polja, možda bi bilo dobro odložiti ih dok zaista ne budete trebali da ih koristite. Da biste odložili polja, zamenite metodu `get_queryset`.

[Sadržaj](#sadržaj)

### Paginator

`Paginator` svakog admin pogleda zahteva ukupan broj zapisa, što dovodi do prebrojavanja zapisa preko cele tabele. Za tabelu sa milionima zapisa, prebrojavanje može potrajati. Za admin pogled za velike tabele, možemo pokušati da sprečimo prebrojavanje. Možemo nadjačati `paginator` atribut admin objekta
sa sledećim blokom koda.

```py
from django.contrib import admin
from django.core.paginator import Paginator
class BigTablePaginator(Paginator):

    @property
    def count(self):
        return 9999999

class BigTableAdmin(admin.ModelAdmin):
    paginator = BigTablePaginator
    show_full_result_count = False
```

Za svaku veliku tabelu, možemo naslediti `BigTableAdmin`. Na taj način možemo izbeći prebrojavanje zapisa u velikim tabelama.

[Sadržaj](#sadržaj)

### readonly_fields

Na stranici sa detaljima, Django kreira element za uređivanje za svako polje. Tekstualna i numerička polja prikazivaće se kao redovno polje za unos. Polja izbora i polja stranog ključa prikazivaće se kao element `<select>`.

Da bi prikazao polje za potvrdu, Django mora učiniti sledeće:

- Preuzeti ceo povezani model i njegove opise.
- Prikazati celu listu opcija, jedna opcija za svaku povezanu instancu.

Uobičajeni scenario koji se često previdi je strani ključ za model `User`. Kada imate 100 korisnika, možda nećete primetiti opterećenje, ali šta se dešava kada imate 100.000 korisnika? Stranica sa detaljima preuzima celu tabelu korisnika, a lista opcija će rezultirajući HTML učiniti ogromnim. Plaćamo dva puta, prvo za
skeniranje kompletne tabele, a zatim za preuzimanje html datoteke. A da se i ne spominje memorija potrebna za generisanje html datoteke.

Imati element za odabir sa opcijama od 100.000 nije baš upotrebljivo. Najlakši način da sprečite Django da prikazuje polje kao element `<select>` je da ga označite kao `readonly_fields`:

```py
@admin.register(SomeModel)
def SomeModelAdmin(admin.ModelAdmin):
    readonly_fields = ( 'user', )
```

Ovo će prikazati opis povezanog modela, bez mogućnosti da ga promenite u adminu. Druga opcija koja sprečava Django da prikazuje okvir za odabir je označavanje polja kao `raw_id_fields`.

```py
@admin.register(SomeModel)
def SomeModelAdmin(admin.ModelAdmin):
    raw_id_fields = ('user', )
```

Koristeći `raw_id_fields`, Django će prikazati poseban vidžet koji prikazuje ID vrednosti i opciju za otvaranje liste svih vrednosti u iskačućem prozoru. Ova opcija je veoma korisna kada želite da izmenite vrednost stranog ključa.

[Sadržaj](#sadržaj)

### Skaliranje admin date_hierarchy

Ako niste familijarni sa Django Admin `date_hierarchy` opcijom - trebalo bi, sjajna je.Postavlja `date_hierarchy` atribut `ModelAdmin` na neko `DateField` polje:

```py
from django.contrib import admin
from .models import Sale

@admin.register(Sale)
class SaleAdmin(admin.ModelAdmin):
    date_hierarchy = 'created'
```

Na ovaj način dobijete lep propadajuči meni na vrhu list view strane. Kada izaberete godinu, Django filtrira podatke i prikaže listu meseci za datu godinu.

[Sadržaj](#sadržaj)

#### Paket django-admin-lightweight-date-hierarchy

U nekoliko projekata smo koristili ovu mogćnost. Odlučili smo da objavimo kao paket.

Instalacija:

```py
pip install django-admin-lightweight-date-hierarchy
```

Dodati u `INSTALLED_APPS`:

```py
INSTALLED_APPS = (
    'django_admin_lightweight_date_hierarchy',
)
```

Postavi `date_hierarchy_drilldown` na `False` na `ModelAdmin`-u da bi se sprečilo podrazumevano propadajuće ponašanje:

```py
@admin.register(MyModel)
class MyModelAdmin(admin.ModelAdmin):
    date_hierarchy = 'created'
    date_hierarchy_drilldown = False
```

[Sadržaj](#sadržaj)

#### date_hierarchy

Otkrili smo da se ovaj indeks može koristiti za poboljšanje generisanja upita sa predikatom hijerarhije datuma u PostgresSQL 9.4:

```sql
CREATE INDEX yourmodel_date_hierarchy_ix ON yourmodel_table (
  extract('day' from created at time zone 'America/New_York'),
  extract('month' from created at time zone 'America/New_York'),
  extract('year' from created at time zone 'America/New_York')
)
```

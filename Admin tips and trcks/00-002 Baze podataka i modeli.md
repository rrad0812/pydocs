
# Baze podataka i nodeli u Django-u

Objektno-relaciono mapiranje (ORM) je sloj između aplikacije i baze podataka. Ono konvertuje upite napisane u kodu u SQL i konvertuje tabelarne rezultate u objekte.

Ovo pojednostavljuje kreiranje aplikacije na dva načina:

1. Omogućava programerima da se fokusiraju na objekte, a ne na osnovnu bazu podataka.
2. ORM se može ažurirati da koristi drugu bazu podataka bez potrebe za prepisivanjem gomile koda.

Podrazumevana razvojna baza podataka za Django projekte je `SQLite`. Međutim, možete koristiti i `MySQL` i `PostgreSQL`. Tip baze podataka je definisan u `settings.py` datoteci u podešavanjima `DATABASES`.

```py
DATABASES = {
  'default': {
    'ENGINE': 'django.db.backends.sqlite3',
    'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
  }
}
```

Modeli koriste atribute klase `Model` za definisanje polja. Svaka klasa modela predstavlja tabelu u bazi podataka. Svaki objekat ima metode za interakciju sa bazom podataka ( `Save` za umetanje i ažuriranje, `Delete` za uklanjanje zapisa i `Queries` za učitavanje zapisa), koje su već uključene u Django.

U donjem primeru, prvi model se zove `Item` , a polja u modelu sadrže informacije o nazivu, opisu i svojstvima svakog polja u modelu. Sintaksa za određivanje svojstava polja je:

```py
property_name = models.Type(parameters)
```

```py
from django.db import models

class Item(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField()
    amount = models.IntegerField()
```

Modeli su obično organizovani na sledeći način:

```py
choices for select fields
fields od models
attributes of custom manager

Meta klasa
def __str__()
def save()
def get_absolute_url()
```

## Tipovi i svojstva polja

Polja u modelu predstavljaju polja baze podataka tabele koju model definiše. Polja se u Django-u određuju pomoću atributa klase modela.
Django ima mnogo ugrađenih tipova polja.

Primeri uobičajenih tipova polja prikazani su u tabeli ispod.

 Polje          | Primer vrednosti
----------------|-------------------------------------
 `DecimalField` | 0,5
 `ImageField`   | avatar.jpg
 `DateTimeField`| datetime(1960, 1, 1, 8, 0, d 0)

Različiti tipovi polja mogu imati različite atribute. Ispod su neki primeri, a kompletna lista se nalazi u Django dokumentaciji u odeljku
`ModelField Reference`:

Atribut `max_length` se koristi za definisanje maksimalne dužine `CharField`-a ili jednog od njegovih podređenih polja kao što je `URLField`.

Atribut `null` podešen na `True` kaže da se polje može sačuvati kao null, što znači da nema podataka za to polje u datom zapisu.

Atribut `blank` se može podesiti na vrednost `True` u tekstualnom polju kako bi se omogućio prazan string.

Atribut `default` se može koristiti za postavljanje podrazumevane vrednosti za polje.

Atribut `choices` se koristi za ograničavanje vrednosti koje se mogu sačuvati u tom polju na skup datih izbora.

Atribut `help_text` se koristi da pomogne korisniku da oko ulazne vrednosti.

## Nazivi polja

Konvencija u Django-u za nazive polja je da se koriste mala slova i donje crte ( _ ) umesto velikih i malih slova (CamelCase).

Django primenjuje neka podrazumevana pravila prikaza za imenovanje polja koja treba imati na umu:

- Donje crte se zamenjuju razmacima kada se prikazuju, first_name
  prikazuje "first name"

- Django automatski piše veliko prvo slovo imena polja, blog se prikazuje
  kao Blog

- Django dodaje „s“ imenu polja da bi označio stavke u množini Ovo možete
  prilagoditi po potrebi (videti dole).
  - blog se prikazuje kao Blogs
  - entry se prikazuje kao Entrys

- Django vam omogućava da prilagodite labelu polja sledećim parametrima:
  - `verbose_name` - omogućava vam da unesete prilagođeno ime koje se
    razlikuju od imena polja. Primer:

    ```py
    name = models.CharField('album', max_length=50)
    ```

  - `verbose_name_plural` - omogućava vam da zamenite Django-ovo
    podrazumevano podešavanje za imena u množini kada dodavanje `s` nije gramatički ispravno. Primer:

    ```py
    verbose_name_plural = 'entries'
    ```

## Primarni ključevi

Django automatski kreira `id` ključ na svakoj tabeli kao primarni ključ. Međutim, možete ovo prilagoditi ako više volite da odredite
primarni ključ umesto da koristite podrazumevani. Ispod su pravila:

1. Ako je polje string, dodajte mu `primary_key=True` u odeljak `parameters` deklaracije `CharField`.
2. Ako je polje ceo broj, dodajte mu `primary_key=True` u odeljak `parameters` deklaracije `IntField`.

> [!Note]
>
> Ako vam je potrebno da dozvolite `null` vrednost u `BooleanField`,
> koristite `NullBooleanField`.

## Relacije

### Polje ForeignKey (više prema jedan)

Da biste označili relaciju dete-roditelj, možete koristiti Django-ovu `models.ForeignKey`.

```py
class Artist(models.Model):
    name = models.CharField(max_length=50)
    year_formed = models.PositiveIntegerField()

class Album(models.Model):
    name = models.CharField(max_length=50)
    artist = models.ForeignKey(Artist, on_delete=models.CASCADE)
```

Počevši od Django 2.0, `on_delete` argument je obavezan za polja `ForeignKey`. On govori Django-u kako da rukuje objektima na koje se poziva `ForeignKey` kada se objekat obriše.

Opcije su:

- `model.CASCADE` - Briše objekat koji sadrži spoljni ključ.
- `model.PROTECT` - Sprečava brisanje referenciranog objekta izazivanjem greške.
- `model.SET_NULL` - Postavlja spoljni ključ na null; `null=True` argument mora biti prisutan da bi ovo funkcionisalo.
- `model.SET_DEFAULT` - Postavlja spoljni ključ na podrazumevanu vrednost; `default=` argument mora biti podešen da bi ovo ispravno funkcionisalo.
- `model.DO_NOTHING` - Ne preduzima nikakvu akciju. (Ovo može prouzrokovati grešku integriteta ako vaš bekend baze podataka primenjuje referencijalni integritet.)

### Ostale polja za relacije

Polje `ManyToManyField` ima relaciju „više-prema-više“ i funkcioniše kao polje `ForeignKey`, ali ima nekoliko dodatnih argumenata o kojima treba voditi računa. Više detalja možete pronaći u Django dokumentaciji.

Polje `OneToOneField` ima relaciju `jedan-prema-jedan` i konceptualno je slično stranom ključu ( `ForeignKey` ) sa `unique=True`, ali će `obrnuta` strana relacije direktno vratiti jedan objekat.

## Queryset

`QuerySet` je kolekcija objekata iz vaše baze podataka koja može imati nekoliko filtera za ograničavanje rezultata. Obično se podešava u Django `menadžeru modela`.

`QuerySet` se ne izvršava dok se ne prisili ili ne pozive, što ga čini veoma efikasnim načinom za preuzimanje podataka.

### Uobičajene metode upita

- `all()` - skup upita sadrži sve instance modela
- `create()` - prihvata argumente polja korišćenih u modelu i kreira novu instancu
- `get()` - koristi se za dobijanje specifične instance modela određene datim argumentom ( npr. id=1 ); izdaće grešku ako se ne pronađu instance koje odgovaraju argumentu ili ih ima više
- `filter()` - koristi se za filtriranje objekata koji odgovaraju datim kriterijumima; vratiće prazan skup upita bez greške ako se ne pronađu odgovarajući objekti
- `get_or_create()` - traži određenu instancu i ako ona ne postoji, kreira je
- `delete()` - briše sve instance unutar preuzetog skupa upita

### Menadžeri modela

`Menadžer modela` je interfejs preko koga se operacije upita baze podataka pružaju Django modelima.

**Podrazumevani menadžer modela**:

Za sve Django modele podrazumevani maenadžer modela naziva se `objects`. Više informacija potražite u `Menagers`.

**Prilagodjeni menadžer modela**:

Međutim, možete kreirati prilagođene menadžere modela proširivanjem osnovne klase Manager. Razlozi zbog kojih biste to možda želeli da uradite uključuju:

- Potrebno je dodati dodatne metode menadžeru
- Potrebno je izmeniti početni QuerySet menadžera

### Korišćenje više menadžera na istom modelu

```py
class AuthorManager(models.Manager):
  def get_queryset(self):
    return super(AuthorManager, self).get_queryset().filter(role='A')

class EditorManager(models.Manager):
  def get_queryset(self):
    return super(EditorManager, self).get_queryset().filter(role='E')

class Person(models.Model):
  first_name = models.CharField(max_length=50)
  last_name = models.CharField(max_length=50)
  role = models.CharField(max_length=1, choices=(('A', _('Author')), ('E', _('Editor'))))
  people = models.Manager()
  authors = AuthorManager()
  editors = EditorManager()
```

### Pozivanje prilagođenih QuerySet metoda iz menadžera

```py
def editors(self):
  return self.get_queryset().editors()

class Person(models.Model):
  first_name = models.CharField(max_length=50)
  last_name = models.CharField(max_length=50)
  role = models.CharField(max_length=1, choices=(('A', _('Author')), ('E', _('Editor'))))
  people = PersonManager()
```

## Ugrađeni modeli

Django dolazi sa nekim ugrađenim modelima spremnim za upotrebu. Često korišćeni ugrađeni model je model `User` sa ugrađenom autentifikacijom.

### Django User model

#### Ugrađena polja

Primarna polja za Django `User` model su:

- `username` - Obavezno polje koje ima 150 znakova ili manje i može da sadrži alfanumeričke znakove, kao i _ , @ , + , . , i ~
- `password` - Obavezno polje koje sadrži lozinku korisnika.
- `email` - opciono
- `first_name` - opciono
- `last_name` - opciono
- `is_staff` - Određuje da li ovaj korisnik može pristupiti administratorskoj stranici.
- `is_superuser` - Implicitno određuje da li korisnik ima sve dozvole bez njihovog dodeljivanja.

#### Atributi

- `is_authenticated` - atribut samo za čitanje koji je uvek `True` za prijavljenog korisnika

#### Prilagodjenje korisničkog modela

Iako ugrađeni Django User model obavlja osnovne funkcije umesto nas, on možda ne sadrži sve informacije koje želimo o korisniku.

Najčešće preporučeni način za rešavanje ovog problema je proširenje Django `User` modela.

U nastavku su dva veoma dobra članka koja vas vode kroz proširivanje DJango `User` modela.

## Korišćenje ModelForm

Django dolazi sa dve osnovne klase za izradu formi. `Form` vam omogućava da izgradite standardne forme, dok `ModelForm` vam omogućava da izgradite forme za kreiranje ili ažuriranje instanci modela.

1. Napravite HTML datoteku koja će sadržati formu.

   Dodajte novi HTML obrazac za čuvanje forme. Primer ispod je datoteka sa nazivom `create.html` koja opisuje metod i raspored forme.

   ```html
   <html>
   <body>
   
   </body>
   </html>
   ```

2. Izmenite datoteku `models.py` da biste povezali prikaz sa formom.
   Dodajte sledeću liniju za uvoz da biste koristili klasu `ModelForm`:

   ```py
   from Django.forms import ModelForm 
   ```

   Dodajte klasu koja detaljno opisuje sadržaj forme. Ispod je primer datoteke `model.py` koja kreira formular za obradu podataka za dodavanje muzičkih izvođača:

   `models.py`

   ```py
   from django.db import models
   from django.forms import ModelForm
   
   # Create your models here.
   class Artist(models.Model):
   
   """ Define Artist field """
     name = models.CharField('artist', max_length=50)
     year_formed = models.PositiveIntegerField()
   
   class ArtistForm(ModelForm):
      
     class Meta:
       model = Artist
       fields = ['name', 'year_formed']
   ```

   Podrazumevano, forma će biti kreiran kao tabela. Međutim, možete ga lako izmeniti korišćenjem komandi u nastavku:

   - `{{ form.as_p }}` koristiće `<p>` elemente
   - `{{ form.as_ul }}` koristiće `<li>` elemente

3. Izmenite datoteku `views.py` da biste dodali funkciju za kreiranje
   forme. Dodajte sledeću liniju za uvoz:

   ```py
   from django.http import HttpResponseRedirect
   ```

   Dodajte funkciju za obradu podataka koji se obrađuju. Ispod je primer funkcije koja će obraditi zahtev i popuniti obrazac podacima o muzičkom izvođaču ili će sačuvati informacije ako ih je korisnik dodao:

   ```py
   def artistcreate(request):
     if request.method == "GET":
       form = ArtistForm()
       return render(request, 'app/create.html', ['form':form });
     elif request.method == "POST":
        form = ArtistForm(request.POST)
        form.save()
        return HttpResponseRedirect('/artists')
   ```

4. Izmenite odeljak `urlpatterns` u datoteci `urls.py`

   Dodajte obrazac URL-a u `urlpatterns` odeljak da biste prešli na ispravan prikaz:

   ```py
   url('artists/create', app.views.artistcreate, name='artistcreate')
   ```

### Uklanjanje polja

Promena polja (promena atributa polja)

#### Migracije

Koraci uključeni u migraciju:

1. Izvršite migraciju ( `python manage.py makemigrations` )
2. Primeni migraciju ( `python manage.py migrate` )

Proces migracije može izgledati ovako:

1. Izvršite migraciju pokretanjem sledeće komande:

   ```shell
   python manage.py makemigrations --name migration_name app_name
   ```

   > [!Note]
   >
   > Svaka migracija će imati drugačije ime. Ako ne dodelite ime, Django  
     će joj automatski dati generičko ime.

2. Pregledajte migracije koje su izvršene.

   Da biste videli broj i naziv svake migracije koja je izvršena, koristite sledeću komandu:

   ```shell
   python manage.py showmigrations <app_name>
   ```

   Ili da biste videli sve završene migracije, koristite ovu komandu:

   ```shell
   python manage.py showmigrations --list
   ```

   Rezultati prikazuju sve migracije koje još nisu izvršene. Obratite pažnju na [ ] simbole. Da je migracija završena, videli biste [X].

3. Opciono: SQL izraze koji će se koristiti možete zapravo videti tako  
   što ćete otkucati sledeću komandu gde je „app“ naziv aplikacije,
   „0001_initial“ naziv migracije koju želite da vidite:

   ```shell
   python manage.py sqlmigrate app 0001_initial
   ```

4. Primenite migraciju.

   Primenite sve migracije kucanjem komande:

   ```shell
   python manage.py migrate
   ```

   Ne morate da primenite sve migracije odjednom. Možete ih primeniti i po aplikaciji ili po aplikaciji i nazivu migracije:

   ```shell
   python manage.py migrate <app_name> <migration_name>
   ```

   Za više uslužnih programa komandne linije koji rade sa migracijama, pogledajte odeljak Migracije na stranici Administracija Django-a u
   ovoj knjizi.

## Najbolje prakse

### Pišite debele modele

`Pišite debele modele i mršave poglede.` To znači da treba da učinite svoje modele robusnim tako što ćete logiku razbiti na male metode unutar vaših modela. Ovo vam omogućava da ih koristite više puta iz više izvora (administrativni interfejs, korisnički interfejs na frontendu, krajnje tačke API-ja, višestruki prikazi) u nekoliko redova koda umesto kopiranja i lepljenja tona koda.

Dakle, sledeći put kada šaljete korisniku imejl, proširite model funkcijom imejla umesto da pišete ovu logiku u svom kontroleru. Ovo takođe olakšava jedinično testiranje vašeg koda jer možete testirati logiku imejla na jednom mestu, umesto više puta u svakom kontroleru gde se to dešava.

### Stil modela

Redosled unutrašnjih klasa modela i standardnih metoda treba da bude sledeći (uz napomenu da nisu svi obavezni):

1. Sva polja baze podataka
2. Atributi prilagođenog menadžera
3. class Meta
     verbose_name_plural = 'people'

### Choices

Ako choices je definisano za dato polje modela, definišite svaki izbor kao listu torki, sa imenom napisanim velikim slovima kao atributom klase na modelu. Primer:

```py
class MyModel(models.Model):
  DIRECTION_UP = 'U'
  DIRECTION_DOWN = 'D'

  DIRECTION_CHOICES = [
    (DIRECTION_UP, 'Up'),
    (DIRECTION_DOWN, 'Down'),
  ]
```

### Koristite eksplicitno imenovanje

Koristite eksplicitno imenovanje u svojim modelima od samog početka kako biste izbegli probleme kako vaši modeli postaju sve složeniji. Zbog implicitnih pravila imenovanja u DŽangu, sledeće tri linije koda su identične onima u DŽangu:

```py
# All 3 are equivalent!
full_name = models.CharField(max_length=100)
full_name = models.CharField('full name', max_length=100)
full_name = models.CharField(verbose_name='full name', max_length=100)
```

Kada dodajete relaciona polja, bolje je da eksplicitno definišete imena ovih polja nego da dozvolite Django-u da primenjuje svoja pravila
imenovanja.

### Čuvanje korisničkih datoteka

Kada koristite tipove polja `FileField()` ili `ImageField()`, definišete putanju gde će se datoteke otpremiti.

Čuvanje više datoteka u jednoj fascikli znači da će sistem datoteka sporije tražiti potrebnu datoteku. Da biste izbegli takve probleme,
možete učiniti sledeće:

- Uvek koristite `unique` i `unique_together` za logički jedinstvene podatke.

  - `unique=True` se koristi u parametrima polja modela da bi se
    naznačilo da vrednost u ovom polju mora biti jedinstvena u celoj tabeli.

  - `unique_together=['first_field', 'second_field']` se koristi u meta
    opcijama da bi se naznačila lista polja koja moraju biti jedinstvena kada se posmatraju zajedno.

- Nikada ne koristite `required=False` u svakoj oblasti.

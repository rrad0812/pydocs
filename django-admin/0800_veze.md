
# Veze

## Sadržaj

[Nazad](sadrzaj.md)

- [Povezana polja na admin listview strani](#povezana-polja-na-admin-listview-strani)
- [Navigacija po povezanim poljima](#navigacija-po-povezanim-poljima)
  - [to_parent_link](#to_parent_link)
  - [to_child_link](#to_child_link)
- [Pružanje veza do drugih stranica sa listama objekata modela](#pružanje-veza-do-drugih-stranica-sa-listama-objekata-modela)
- [Reverzne veze](#reverzne-veze)
  - [related_name](#related_name)
  - [related_query_name](#related_query_name)
- [Autokompletiranje za povezana polja](#autokompletiranje-za-povezana-polja)
- [Hiperlinkovi](#hiperlinkovi)
- [Kako dobiti admin urls za specifične objekte?](#kako-dobiti-admin-urls-za-specifične-objekte)

### Povezana polja na admin listview strani

Django admin ima `ModelAdmin` klasu koja pruža opcije i funkcionalnosti za modele u adminu. To su `list_display`, `list_filter`, `search_fields` za specificiranje polja za odgovarajuću akciju.

`search_fields`, `list_filter` i druge opcije takodje pružaju uključivanje `ForeignKey` ili `ManyToMany` polja sa lookup API notacijom. Na primer, da bi pretražili po polju "name" "BestSellerAdmin" modela, možemo specificirati "book.name" u `search_fields`.

```py
from django.contrib import admin 
from book.models import BestSeller

class BestSellerAdmin(RelatedFieldAdmin):
    search_fields = ('book name', )
    list_display = ('id', 'year', 'rank', 'book')
    
admin.site.register(Bestseller, BestsellerAdmin)
```

Primetite da je "BestSellerAdmin" klasa nasledjena iz `RelatedFieldAdmin` klase.

Django ne dozvoljava istu notaciju u `list_display` opciji. Da bi uključili `ForeignKey` polje ili `ManyToMany` polje u `list_display`, moramo pisati prilagodjenu metodu i dodati je u `list_display`.

```py
from django.contrib import admin
from book.models import BestSeller

class BestSellerAdmin(RelatedFieldAdmin):
    list_display = ('id', 'rank', 'year', 'book', 'author')
    search_fields = ('book name', )

    def author(self, obj):
        return obj.book.author
    
    author.description = 'Author'
```

Dodavanje ForeignKey na taj način u `list_display` postaje zamorno, pogotovu ako je više polja sa `ForeignKey` na povezane modele. Možemo napisati prilagodjenu klasu za dinamičko postavljanje atributa kao metode tako da možemo postaviti `ForeignKey` polja u `list_display`.

```py
def get_related_field(name, admin_order_field=None,short_description=None):
    related_names = name.split(' ')

def dynamic_attribute(obj):
    for related_name in related_names:
    obj = getattr(obj, related_name)
    return obj

dynamic_attribute.admin_order_field = admin_order_field or name
dynamic_attribute.short_description = short_description or related_names[-1].   
    title ().replace('_', ' ')

return dynamic_attribute

class RelatedFieldAdmin(admin.ModelAdmin):

    def getattr (self, attr):
        if ' ' in attr:
            return get_related_field(attr)
        return self.getattribute (attr)

class BestSellerAdmin(RelatedFieldAdmin):
    list_display = ('id', 'rank', 'year', 'book','book author')
```

Nasledjivanjem klase `RelatedFieldAdmin`, možemo direktno koristiti `ForeignKey` polja u `list_display`. Medjutim to će dovesti do N+1 problema.
<https://github.com/theatlantic/django-nested-admin>.

[Sadržaj](#sadržaj)

### Navigacija po povezanim poljima

Ponekad može biti korisno brzo kretanje između objekata. Nakon pokušaja da naučimo osoblje za podršku da filtrira pomoću parametara URL-a, konačno smo odustali i stvorili dva jednostavna dekoratora.

[Sadržaj](#sadržaj)

#### to_parent_link

Napravite vezu do stranice povezanog modela:

```py
def admin_change_url(obj):
    app_label = obj._meta.app_label
    model_name = obj._meta.model. name .lower()
    return reverse('admin:{}_{}_change'.format( app_label, model_name ), args=(obj.pk,))

def to_parent_link(attr, short_description, empty_description="-"):
    """
    Dekorator za render linka na povezani admin detalj model
    attr (str) : Ime povezanog polja.
    short_description (str): Opis polja.
    empty_description (str): Vrednost za prikazivanje ako je povezanoi polje None.
    Treba da vrati link tekst.
    Korišćenje:
    @to_parent_link('credit_card', ('Credit Card'))
    def credit_card_link(self, credit_card):

    return credit_card.name
    """
    def wrap(func):

        def field_func(self, obj):
            related_obj = getattr(obj, attr)
            if related_obj is None:
                return empty_description
            url = admin_change_url(related_obj)
            return format_html('<a href="{}">{}</a>', url, func(self, related_obj)

        field_func.short_description = short_description
        field_func.allow_tags = True
        return field_func

    return wrap

Dekorator će prikazati vezu ( <a href="..."> ... </a> ) do povezanog modela. Ako na primer želimo da dodamo vezu sa svakog proizvoda na stranicu sa detaljima o kategoriji, koristimo dekorater ovako:

@admin.register(models.Product)
class ProductAdmin(admin.ModelAdmin):
    list_display = ( 'id', 'name', 'category_link', )
    admin_select_related = ('category',)
    
    @to_parent_link('category', _('Category'))
    def category_link(self, category):
        return category
```

[Sadržaj](#sadržaj)

#### to_child_link

Komplikovanije veze poput „svi proizvodi kategorije“ zahtevaju drugačiji pristup. Napravili smo dekorator koji prihvata niz upita i povezuje se na prikaz liste povezanog modela:

```py
def admin_changelist_url(model):
    app_label = model._meta.app_label
    model_name = model. name .lower()
    return reverse('admin:{}_{}_changelist'.format(app_label, model_name))

def to_child_link(attr, short_description, empty_description='-',   
    query_string=None):
    """
    Decorator used for rendering a link to the list display of a related model.
    attr (str): Name of the related field.
    short_description (str): Field display name.
    empty_description (str): Value to display if the related field is None.
    query_string (function): Optional callback for adding a query string to the link.
    Receives the object and should return a query string.
    The wrapped method receives the related object and should return the
    linktext.
    Usage:
    @admin_changelist_link('credit_card', _('Credit Card'))
    def credit_card_link(self, credit_card):

    return credit_card.name
    """
    def wrap(func):
        def field_func(self, obj):
            related_obj = getattr(obj, attr)
            if related_obj is None:
                return empty_description
            url = admin_changelist_url(related_obj.model)
            if query_string:
                url += '?' + query_string(obj)
                return format_html('<a href="{}">{}</a>', url, func(self, related_obj))

        field_func.short_description = short_description
        field_func.allow_tags = True
        return field_func
    return wrap
```

Da bismo dodali vezu iz kategorije u svoje proizvode, u CategoryAdmin uradimo sledeće:

```py
@admin.register(models.Category)
class CategoryAdmin(admin.ModelAdmin):
    list_display = ('id', 'name', 'products_link', )
    @to_child_link('products', _('Products'), query_string=lambda c:
    'category_id={}'.format(c.pk))
    def products_link(self, products):
        return _('Products')
```

Budite oprezni sa argumentom proizvoda. Veoma je primamljivo učiniti nešto poput ovog:

```py
@to_child_link('products',('Products'),
    query_string=lambda c:'category_id={}'.format(c.pk))

def products_link(self, products):
    “ Dont do that! “
    return 'see {} products'.format(products.count())
```

Gornji primer rezultiraće dodatnim upitima.

[Sadržaj](#sadržaj)

### Pružanje veza do drugih stranica sa listama objekata modela

Uobičajeno je da se objekti pozivaju na druge objekte pomoću stranih ključeva. list_display možete usmeriti na metodu koja vraća HTML vezu. Unutar core/admin.py izmenite klasu CourseAdmin na sledeći način:

```py
from django.urls import reverse
from django.utils.http import urlencode
@admin.register(Course)
class CourseAdmin(admin.ModelAdmin):

list_display = ("name", "year", "view_students_link")
def view_students_link(self, obj):

count = obj.person_set.count()
url = (reverse( "admin:core_person_changelist") + "?"

+urlencode({"courses id":
f"{obj.id}"})

)
return format_html('<a href="{}">{} Students</a>', url, count)
```

Ovaj kod dovodi do toga da lista objekata modela Course ima tri kolone:

- Naziv kursa
- Godina u kojoj je kurs ponuđen
- Veza koja prikazuje broj studenata na kursu

Kada kliknete na 2 učenika, preusmerava vas na stranicu sa listom objekata modela Person sa primenjenim filterom. Filtrirana stranica prikazuje samo studente na odgovarajućem kursu: Psich 101, Buffy i Villow. Xander se nije prijavio na ovaj kurs.

Primer koda koristi reverse()metodu za dobijanje je URLadrese u Django adminu. Možete potražiti bilo koju stranicu admina koristeći sledeću konvenciju:

```py
"admin:%(app)s_%(model)s_%(page)"
```

Ova struktura deli se na sledeći način:

- `admin`: je prostor imena.
- `app`: je naziv aplikacije.
- `model`: je objekt modela.
- `page`: je tip stranice u Django adminu.

Za gornji primer view_students_link() koristite admin: core_person_changelist da biste dobili referencu na stranicu liste objekta modela Personu core aplikacije.Evo dostupnih naziva URL-ova:

 Strana     | URL                            | Ime
------------|--------------------------------|---------
Listview    | %(app)s\_%(model)s\_changelist | listview
Addview     | %(app)s\_%(model)s\_add        | add
Historyview | %(app)s\_%(model)s\_history    | history  
Deleteview  | %(app)s\_%(model)s\_delete     | delete
Changeview  | %(app)s\_%(model)s\_change     | change

Stranicu liste objekata možete da filtrirate dodavanjem stringa na URL-u. Ovaj string upita modifikuje QuerySet koji se koristi za popunjavanje stranice. U gornjem primeru, string upita "?courses id = {obj.id}" filtrira listu osoba samo prema onim objektima koji imaju odgovarajuću vrednost u polju "Person.course".

Ovi filteri podržavaju pretragu polja `QuerySet` pomoću dvostrukih donjih crta (`__`). Možete pristupiti atributima povezanih objekata, kao i koristiti modifikatore filtera kao što su `exact` i `startswith`.

[Sadržaj](#sadržaj)

### Reverzne veze

#### related_name

`related_name` atribut u `ForeignKey` poljima je jako koristan. Daje nam mogućnost da definišemo značajno ime za reverznu vezu.

> [!Note]
>
> **Pravilo**: Ako niste sigurni šta bi trebalo da bude `related_name`, koristite množinu imena modela.

```py
class Company:
    name = models.CharField(max_length=30)

class Employee:
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
    company = models.ForeignKey(Company,
        on_delete=models.CASCADE, related_name='employees')
```

To znači da "Company" model će imati poseban atribut "employees", koji će vraćati `QuerySet` sa svim zaposlenim instancama povezanih sa kompanijom.

```py
google = Company.objects.get(name='Google')
google.employees.all()
```

Možete koristiti reverznu vezu za modifikovanje company polja na Employee instancama:

```py
vitor = Employee.objects.get(first_name='Vitor')
google = Company.objects.get(name='Google')
google.employees.add(vitor)
```

[Sadržaj](#sadržaj)

#### related_query_name

Ova se vrsta veze primenjuje i na filterske upite. Na primer, ako želim da izlistam sve kompanije koji zapošljavaju ljude sa imenom "Vitor", uradiću sledeće:

```py
companies = Company.objects.filter(employees__first_name='Vitor')
```

Ako želite sa promenite ime veze, evo kako da to uradite:

```py
class Employee:
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
    company = models.ForeignKey(Company, on_delete=models.CASCADE,
        related_name='employees', related_query_name='person'
)
```

Tada bi korišćenje bilo:

```py
companies = Company.objects.filter(person__first_name='Vitor')
```

Da bi bilo konzistentno `related_name` ide u množini a `related_query_name` ide u jednini.

[Sadržaj](#sadržaj)

### Autokompletiranje za povezana polja

Hajdemo u "BookAdmin" i pokušajmo da dodamo novu knjigu. Da bi popravili podrazumevano ponašanje, možemo pružiti autokompletirajuću opciju za polje "author" tako da korisnicimogu pretraživati i izabrati potrebnog autora.

```py
from book.models import Book

class AuthorAdmin(admin.ModelAdmin):
    search_fields = ('name',)

class BookAdmin(admin.ModelAdmin):
    list_display = ('id', 'name', 'author')
    autocomplete_fields = ('author',)

admin.site.register(Book, BookAdmin)
```

`ModelAdmin` pruža `autocomplete_fields` opciju za `select2` autokompletirajući ulaz. Takodje treba da definišemo `search_fields` na povezanom polju modela tako da admin može da pretražuje po njemu.

[Sadržaj](#sadržaj)

### Hiperlinkovi

Hajde da pregledamo "BookAdmin# i izaberimo neku od knjiga.

```py
from django.contrib import admin
from .models import Book

class BookAdmin(admin.ModelAdmin):
    list_display = ('id', 'name', 'author')

admin.site.register(Book, BookAdmin)
```

Ovde je `name` polje povezano sa `changeview` stranom objekta "book". Ali "author" polje je prikazano kao običan tekst. Ako bi smo želeli da predjemo u odgovarajući objekat autora radi promene njegovog imena, morali bi da se vratimo ma kućnu stranicu, izaberemo list stranu modela autora, nadjemo odgovarajućeg i tada preko linka odela autora, nadjemo odgovarajućeg i tada preko linka predjemo na stranicu za promene i promenimo ime autora.

Ovo izgleda kao prilično dosadan niz akcija, pogotovu ako ih treba ponoviti više puta. Ali, ako bi imali `hiperlink` na polju "autor" koji bi vodio direktno na stranu za promene autora, to bi bilo znatno lakše za rad.

Django pruža opciju za pristup admin pogledima njegovim URL reverzirajućim sistemom. Naprimer, možemo dobiti stranu za promene autorovog objekta u admin "Book" modelu korišćenjem sintakse:

```py
reverse(“admin:model_povezani_model_change”, args=id)
```

Primer:

```py
from django.contrib import admin
from django.utils.safestring import mark_safe

class BookAdmin(admin.ModelAdmin):
    list_display = ('name', 'author_link', )

    def author_link(self, book):
        url = reverse("admin:book_author_change", args=[book.author.id])
        link = '<a href="%s">%s</a>' % (url, book.author.name)
        return mark_safe(link)

    author_link.short_description = 'Author'
```

Sada u "BookAdmin" pogledu, "author" polje će biti hiperlinkovano na changeview stranu "AuthorAdmin" modela, možemo je posetiti jednostavnim klikom. Zavisno od zahteva, možemo linkovati bilo koje polje u django sa drugim poljem ili dodati prilagodjeno polje.

[Sadržaj](#sadržaj)

### Kako dobiti admin urls za specifične objekte?

Imate children kolonu koja prikazuje imena dece svakog od Hero objekata. Od vas se traži da linkujete svako od dece na njegovu changeview stranu.

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    def children_display(self, obj):
        display_text = ", ".join([ 
            "<a href={}>{}</a>".format( reverse('admin:{}_{}_change'.format(obj._meta.app_label,obj._meta.model_name), args=(child.pk,)), child.name)
            for child in obj.children.all()
        ])
        if display_text:
            return mark_safe(display_text)
        return "-"
```

Ovde su opcije:

Action  | reverse link
--------|----------------------------------------------------------------------
Delete: | reverse('admin:{}_{}_delete'.format(obj._meta.app_label,obj._meta. model_name), args=(child.pk,))
History:| reverse('admin:{}_{}_history' .format(obj._meta.app_label, obj._meta. model_name), args=(child.pk,))
Change: | reverse('admin:{}_{}_change' .format(obj._meta.app_label, obj._meta. model_name), args=(child.pk,))

[Sadržaj](#sadržaj)

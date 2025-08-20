
# Filtriranje

## Sadržaj

[Nazad](sadrzaj.md)

- [Kako dodati osnovno filtriranje u admin?](#kako-dodati-osnovno-filtriranje-u-admin)
- [Kako dodati prilagođeni filter u admin?](#kako-dodati-prilagođeni-filter-u-admin)
- [Kako ulančati filtere u adminu?](#kako-ulančati-filtere-u-django-adminu)
- [Kako dodati podrazumevani filter u admin?](#kako-dodati-podrazumevani-filter-u-admin)
- [Podrazumevani filteri - II način](#podrazumevani-filteri---ii-način)
- [Prilagodjeni tekst filteri](#prilagodjeni-tekst-filteri)
- [Prilagodjena lista filtera nad datumskim poljem](#prilagodjena-lista-filtera-nad-datumskim-poljem)
- [Napredni filteri](#napredni-filteri)

U ovom vodiču naučićemo osnovno, prilagođeno i napredno filtriranje poput lančanih filtera pomoću Django admina. Za ovaj vodič koristiće se modeli Shop i Product.

```py
class Shop(models.Model):
    user = models.ForeignKey(User, null=True, blank=True, on_delete=models.CASCADE)
    name = models.TextField()
    email = models.CharField(max_length=200)
    address = models.CharField(max_length=1024,null=True,
    blank=True)createdAt = models.DateTimeField(auto_now_add=True)
    updatedAt = models.DateTimeField(auto_now=True)
    
    def str (self):
        return u'%s' % (self.name)

class Product(models.Model):
    user = models.ForeignKey(User, null=True, blank=True,on_delete=models.CASCADE)
    refShop = models.ForeignKey(Shop, db_index=True,on_delete=models.CASCADE)
    title = models.TextField(max_length=65)
    price = models.FloatField(default=0)
    withEndDate = models.BooleanField(default=False)
    endDate = models.DateField(blank=True,null=True)
    description = models.CharField(max_length=65, default='', null=True, blank=True)
    createdAt = models.DateTimeField(auto_now_add=True)
    updatedAt = models.DateTimeField(auto_now=True)
    available = models.BooleanField(default=True)
```

Zaista jednostavno, proizvod pripada radnji.

[Sadržaj](#sadržaj)

### Kako dodati osnovno filtriranje u admin?

Recimo da bismo želeli da filtriramo našu prodavnicu u admin po korisniku. Da biste to postigli samo dodajte `list_filter` za "ShopAdmin" klasu (u admin.py ) i odredite oblast na kojima može da filtrira.

```py
list_filter = ('user', )
```

Pun kod:

```py
class ShopAdmin(admin.ModelAdmin):
    fieldsets = [
        (None, {'fields': ['user','name','email','address']}),
    ]
    list_display = ('user','name', 'email', )
    list_filter = ('user',)
    ordering = ('user',)
```

A sada će se u gornjem desnom uglu ekrana pojaviti opcija filtriranja. Još bolje, recimo da bismo želeli da filtriramo samo za korisnike koji imaju prodavnicu. Ovaj put ćemo napisati:

```py
list_filter = (('user', admin.RelatedOnlyFieldListFilter),)
```

Sada opcija filtriranja sadrži samo jednog korisnika, jer drugi korisnici nemaju prodavnice.

[Sadržaj](#sadržaj)

### Kako dodati prilagođeni filter u admin?

Sada zamislimo da želimo da dodamo filter zasnovan na poslovnim pravilima,a ne na određenom polju. Da bismo to uradili, treba da:

- Nasledimo klasu `SimpleListFilter`,
- Zadamo ime filtera preko osobine "name".
- Zadamo ime parametra filtera preko osobine `parametar_name`,
- Nadjačamo metodu `lookups`,
- Nadjačamo metodu `queryset`

`lookups` metod treba da vrati listu slogova.

- Prvi element sloga je kodirana vrednost koja će se pojaviti u url-u upita,
- Drugi element sloga je ime filtera koji će biti prikazan na admin interfejsu.

`queryset` metod vraća filtrirani `queryset`. Pogledajmo:

```py
class ProductPriceFilter(admin.SimpleListFilter):
    title = 'price'
    parameter_name = 'price'

    def lookups(self, request, model_admin):
        """
        Vraća listu slogova, prvi element svakog sloga je kodirana vrednost opcije koja će se pojaviti u query URL-u, drugi element je ime opcije koja će se pojaviti na filteru.
        """
        
        return (
            ("low_price", "Filter by low price"), 
            ("high_price","Filter by high price")
        )

    def queryset(self, request, queryset):
        """
        Vraća filtrirani queryset baziran na vrednosti query stringa, vraćenog sa `self.value()`.
        """
        if self.value()=="low_price":
            return queryset.filter(price lte=5)
        elif self.value() == "high_price":
            return queryset.filter(price gt=5)
        else:
            return queryset.all()
```

U ovom primeru ćemo dodati mogućnost filtriranja proizvoda po niskoj ili visokoj ceni. Smatramo da je niska cena cena ispod ili jednaka 5, dok će visoka cena biti iznad 5.

Da biste koristili filter treba samo da ukažete na ime filterske klase u `list_filter` opciji:

```py
list_filter = (ProductPriceFilter, )
```

Kao što vidimo, u interfejsu imamo novi filter "By price". Ako kliknemo na jednu od vrednosti, lista prikazanih rezultata će se promeniti i ako obratimo pažnju na url, možemo videti da izabrana vrednost filtera u url-u sadrži naš parametar dodan u metodu pretraživanja:

<http://127.0.0.1:8000/admin/backoffice/product/?price=low_price>

[Sadržaj](#sadržaj)

### Kako ulančati filtere u Django adminu?

Ok, idemo sada malo dalje. Recimo da bismo želeli da primenimo povezane filtere (tj. ulančane filtere). Na Product možemo dodati filtriranje korisnika

```py
list_filter=('user', ProductPriceFilter, 'refShop', )
```

Sada, ako filtriramo prema korisniku, i dalje ćemo videti sve vrednosti Shop (u Shop filteru). Moglo bi biti bolje da prikažemo samo prodavnice koje pripadaju korisniku kojeg smo upravo filtrirali. To je ono što predstavlja ulančani filter. Spisak vrednosti prikazanih u filtru zavisiće od vrednosti izabrane u drugom filtru.

Da bismo to uradili, ispitaćemo URL upita i utvrditi da li je korisnik korišćen za filtriranje rezultata, ako je tako, URL će sadržati

```html
? user_id__exact = "value"
```

Na osnovu toga možemo stvoriti prilagođeni filter koji će odabrati vrednosti samo zaizabranu vrednost korisničkog ID-a.

```py
class UserShopFilter(admin.SimpleListFilter):
    title = 'shop'
    parameter_name = 'refShop'
    
    def lookups(self, request, model_admin):
        if 'user_id__exact' in request.GET:
            id = request.GET['user_id__exact']
            shops = set([c.refShop
                for c in model_admin.model.objects.all().filter(user=id)])
        else:
            shops = set([c.refShop
                for c in model_admin.model.objects.all()])
        return [(s.id, s.name) for s in shops]

    def queryset(self, request, queryset):
        if self.value():
            return queryset.filter(refShop_id__exact = self.value())
```

Dakle, u metodu lookups, proveravamo da li imamo izabranog korisnika i da li ćemo odabrati samo prodavnice koje pripadaju tom korisniku:

```py
shops = set([c.refShop for c in model_admin.model.objects.all().filter(user=id)])
```

U suprotnom možemo odabrati sve prodavnice:

```py
shops = set([c.refShop for c in model_admin.model.objects.all()])
```

Tada možemo vratiti rezultate koji će biti prikazani u adminu:

```py
return [(s.id, s.name) for s in shops]
```

Sada ako na adminu izaberemo korisnika B, moći ćemo da vidimo samo prodavnice koje pripadaju našem korisniku B. To je zato što smo primenili filter koji zavisi od drugog filtera.

[Sadržaj](#sadržaj)

### Kako dodati podrazumevani filter u admin?

Recimo da za admin model naše pasmine želimo da korisnik može istovremeno da vidi sve pasmine iste vrste, dakle pse u jednom trenutku, mačke ili bilo koju drugu, ali ne sve njih zajedno.

Da bismo to uradili, moramo da zamenimo dodatni metod klase SimpleListFilter, value metodu. Ovaj metod je odgovoran da filtru kaže da li postoji izabrana stavka za filtriranje ili ne.

Da bismo to postigli, dodali smo value() metodu u naš filter:

```py
from django.contrib import admin
from adminfilters.models import Species, Breed

class SpeciesListFilter(admin.SimpleListFilter):
    """ 
    Ovaj filter uvek vraća subset instanci modela, bilo filtriranjem 
    po korisničkom izboru ili po default vrednosti.
    """
    title = 'species' parameter_name = 'species'default_value = None

    def lookups(self, request, model_admin):
        """
        Vraća listu slogova, prvi element svakog sloga je kodirana vrednostopcije koja
        će se pojaviti u query URL-u, drugi element je je ime opcije koja će se
        pojaviti na filteru.
        """
        list_of_species = []
        queryset = Species.objects.all(objects.all()for species in queryset:
        list_of_species.append( (str(species.id), species.name) )
        return sorted(list_of_species, key=lambda tp: tp[1])

    def queryset(self, request, queryset):
        """
        Vraća filtrirani queryset baziran na vrednost query stringa,vraćenog sa
        `self.value()`.
        """
        # Uporedjuje zahtevanu vrednost da odluči kako da filtrira queryset.

        if self.value():
            return queryset.filter(species_id = self.value())
        return queryset

    def value(self):
        """
        Prepisivanje ovog metoda imaćemo default vrednost uvek.
        """
        value = super(SpeciesListFilter, self).value()
        
        if value is None:
            if self.default_value is None:
                # Ako nema Species, vrati prvi po name. Inače None.
                first_species = Species.objects.order_by('name').first()
                value = None if first_species is None else first_species.id
                self.default_value = value
            else:
                value = self.default_value
        return str(value)

@admin.register(Breed)
class BreedAdmin(admin.ModelAdmin): list_display = ('name', 'species', )
    list_filter = (SpeciesListFilter, )
```

Samo zamenjujući metodu `value`, postižemo da ako nije izabran nijedan filter, dajemo filteru zadanu vrednost. Ostatak filtera već će vratiti filtrirani upit postavljen na `ModelAdmin`.

> [!Note]
>
> Čak i pritiskom na filter "All" vratiće se lista filtrirana prema podrazumevanoj vrednosti.

[Sadržaj](#sadržaj)

### Podrazumevani filteri - II način

Postoji mnogo pristupa za primenu podrazumevanih filtera. Neki pristupi uključuju prilagođene filtere ili

ubrizgavanje posebnih parametara upita u zahtev. Želeli smo dato izbegnemo. Otkrili smo da je sledeći pristup jednostavan i jasan:

from urllib.parse import urlencode from django.shortcuts import redirect

```py
class DefaultFilterMixin:

    def get_default_filters(self, request):
        """
        Postavlja default filtere na stranu.
        Vraća (dict): Default filter.
        """
        raise NotImplementedError()

    def changelist_view(self, request, extra_context=None):
        ref = request.META.get('HTTP_REFERER', '')
        path = request.META.get('PATH_INFO', '')
        # Ako već ima query parametre ili strana je ref. samu sebe
        # (propadanjem ili redirektom) ne primenjuj default filter.
        if request.GET or ref.endswith(path):
            return super().changelist_view(request, extra_context=extra_context)
        query = urlencode(self.get_default_filters(request))
        return redirect('{}?{}'.format(path, query))
```

Ako je prikazu liste pristupljeno iz drugog prikaza, a nisu navedeni parametri upita, generišemo podrazumevani upit i preusmeravamo.

Primenimo podrazumevani filter na našoj stranici proizvoda da bismo prikazali samo proizvode stvorene u poslednjih mesec dana:

```py
from django.utils import timezone

@admin.register(models.Product)
class ProductAdmin(admin.ModelAdmin, DefaultFilterMixin):
    date_hierarchy = 'created'

    def get_default_filters(self, request):
        now = timezone.now()
        return {'created year': now.year, 'created month': now.month,}
```

Ako idemo na stranicu detalja ili ako dođemo do stranice sa parametrima upita,podrazumevani filter se neće primeniti.

[Sadržaj](#sadržaj)

### Prilagodjeni tekst filteri

Ovde je izbor ograničen. Ali u nekim slučajevima, gde je jako puno filterskih mogućnosti, bolje je prikazati tekst ulaz umesto liste izbora. Hajde da napišemo prilagodjeni filter za filtriranje knjiga po godini publikovanja.

Napišimo ulazni filter.

```py
class PublishedYearFilter(admin.SimpleListFilter):
    title = 'published year'
    parameter_name = 'published_date'
    template = 'admin_input_filter.html'

    def lookups(self, request, model_admin):
        return ((None, None),)

    def choices(self, changelist):
        query_params = changelist.get_filters_params()
        query_params.pop(self.parameter_name, None)
        all_choice = next(super().choices(changelist))
        all_choice['query_params'] = query_params
        yield all_choice

    def queryset(self, request, queryset):
        value = self.value()
        if value:
            return queryset.filter(published_date year=value)
```

`#admin_input_filter.html`

```html
{% load i18n %}
<h3>{% blocktrans with filter_title=title %}By {{filter_title }}
{% endblocktrans %}</h3>
<ul>

<li>
{% with choices.0 as all_choice %}
<form method="GET">

<input type="text" name="{{ spec.parameter_name }}"
value="{{spec.qvalue|default_if_none:"" }}"/>

<input class="btn btn-info" type="submit"value="{% trans 'Apply' %}">
{% if not all_choice.selected %}

<button type="button" class= "btn btn-info">
<a href="{{all_choice.query_string }}">Clear</a></button>

{% endif %}
</form>

{% endwith %}
</li>
</ul>
```

[Sadržaj](#sadržaj)

### Prilagodjena lista filtera nad datumskim poljem

Možemo pisati prilagodjene filtere tako da možemo postaviti izračunata polja i dodati filtere nad njima.

```py
class CenturyFilter(admin.SimpleListFilter):
    title = 'century'
    parameter_name = 'published_date'

    def lookups(self, request, model_admin):
        return ((21, '21st century'), (20, '20th century'),)

    def queryset(self, request, queryset):
        value = self.value()
    
    if not value:
        return queryset

    start = (int(value) -1)*100
    end = start + 99
    return queryset.filter(published_date year gte=start,
```

```py
published_date_year__lte=end)
```

[Sadržaj](#sadržaj)

### Napredni filteri

Sve gornje metode biće korisne samo kao odredjeno proširenje. Preko toga, postoje paketi kao `django-advancedfilters` koji imaju napredne mogućnosti filtriranja.

Za postavku paketa

- Instaliraj paket sa: `pip install django-advanced-filters`
- Dodaj `advanced_filters` u `INSTALLED_APPS`
- Dodaj u projektni `urlconf.py`: `url(r’^advanced_filters/’`,

include(‘advanced_filters.urls’))
-Pokreni python manage.py migrate.

Kada je setup završen možemo pisati:

```py
from advanced_filters.admin import AdminAdvancedFiltersMixin

class BookAdAdminFilter(AdminAdvancedFiltersMixin, admin.ModelAdmin):
    list_display = ('id', 'name', 'author', 'published_date', 'is_available')
    advanced_filter_fields = ('name', 'published_date', 'author', 'is_available')
```

Jednostavno se može kreirati filter za sve knjige publikovane izmedju 1980 i 1990, koje imaju ocenu višu od 3.75 i broj strana veći od 100. Ovakav filter se može imenovati i sačuvati za kasnije korišćenje.

Za više detalja pogledati na: <https://github.com/modlinltd/django-advanced-filters>

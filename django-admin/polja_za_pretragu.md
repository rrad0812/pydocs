
# Polja za pretragu

## Sadržaj

[Nazad](sadrzaj.md)

- [Polja za pretragu na listview strani](#polja-za-pretragu-na-listview-strani)
- [DjangoQL](#djangoql)
  - [Instalacija](#instalacija)
  - [Korišćenje DjangoQL sa standardnim Django admin search-om](#korišćenje-djangoql-sa-standardnim-django-admin-search-om)
  - [Jezičke reference](#jezičke-reference)
  - [DjangoQL šema](#djangoql-šema)
  - [Prilagodjena polja za pretragu](#prilagodjena-polja-za-pretragu)
    - [Search po queryset annotacijama](#search-po-queryset-annotacijama)
    - [Prilagođene suggest_options](#prilagođene-suggest_options)
    - [Prilagodjeni search lookup](#prilagodjeni-search-lookup)
    - [Puni prilagodjeni search lookup](#puni-prilagodjeni-search-lookup)
    - [Mogu li koristiti Django QL van Django admina](#mogu-li-koristiti-django-ql-van-django-admina)
  
### Polja za pretragu na listview strani

Django admin pruža `search_fields` opciju na ModelAdmin. Postavkom ove opcije će biti omogućen boks za pretragu na `listview strani`, za filtriranje objekata na modelu. Može uraditi pretragu po svim poljima modela i povezanim poljima modela takodje.

```py
class BookAdmin(admin.ModelAdmin):
    search_fields = ('name', 'author__name')
```

Sa više polja pretrage `search_fields`, upiti postaju spori jer rade case-insensitive pretragu po svim terminima pretrage na svim poljima iz `search_fields`. Jedan primer pretraga "python for data analysis" se pretvara u sledeću SQL klauzulu:

```sql
WHERE
    (name ILIKE '%python%' OR author.name ILIKE '%python %') AND
    (name ILIKE '%for%' OR author.name ILIKE '%for% ') AND
    (name ILIKE '%data%' OR author.name ILIKE '%data%') AND
    (name ILIKE '%analysis%' OR author.name ILIKE '%analysis%')
```

[Sadržaj](#sadržaj)

### DjangoQL

Napredni jezik Django pretrage sa autokompletiranjem. Podržava logičke operatore zagrade, veze, i radi sa Django modelom.

#### Instalacija

```shell
pip install djangoql
```

Dodaj `djangoql` u `INSTALLED_APPS` listu u `settings.py`:

```py
INSTALLED_APPS = [
    ...
    'djangoql',
    ...
]
```

Dodavanje `DjangoQLSearchMixin` u model admin će zameniti standardnu `search` funkcionalnost sa DjangoQL search-om.

Primer:

```py
from django.contrib import admin
from djangoql.admin import DjangoQLSearchMixin
from .models import Book
@admin.register(Book)
class BookAdmin(DjangoQLSearchMixin, admin.ModelAdmin):
    pass
```

[Sadržaj](#sadržaj)

### Korišćenje DjangoQL sa standardnim Django admin search-om

`DjangoQL` će prepoznati ako imate definisano `search_fields` u ModelAdmin klasi.Vi ćete moći da izaberete izmedju naprednog DjangoQL search-a i standardnog Django search-a (spec. sa search_fields).

Primer:

```py
@admin.register(Book)
class BookAdmin(DjangoQLSearchMixin, admin.ModelAdmin):
    ...
    search_fields = ('title', 'author__name')
```

Za gornji primer jednan čekboks koji kontroliše search mode će se pojaviti blizu search input boksa. Ako je chekboks uključen, DjangoQL search se koristi. Takodje, tu je i opcija `djangoql_completion_enabled_by_default` (postavljena na `True` podrazumevano):

```py
@admin.register(Book)
class BookAdmin(DjangoQLSearchMixin, admin.ModelAdmin):
    search_fields = ('title', 'author__name')
    djangoql_completion_enabled_by_default = False
```

Ako ne želite dva serch moda, jednostavno uklonite `search_fields` iz ModelAdmin klase.

[Sadržaj](#sadržaj)

### Jezičke reference

`DjangoQL` se dobija sa izuzetnim `SyntaxHelp`, koji se može naći u Django admin (vidi `Syntax Help` link na auto-completion popup-u). Ovde je kratak zaključak:

> DjangoQL syntaksa je Python sintaksa, sa malim razlikama. U osnovi vi samo referencirate model polja kao što bi i u Pajton kodu, potom primenjujete komparacijske  i logičke operatore i zagrade.

DjangoQL je osetljiv na veličinu slova.

- Polja modela: tačno ista kao što su definisana u Pajton kodu.
- Pristup ugnježdenim osobinama preko dot operatora, na primer: `author.last_name`;
- Stringovi moraju biti double-quoted. Jednostruki navodnici nisu podržani.
- Da bi uradili escape treba koristiti duple navodnike \";
- Logičke i null vrednosti: True, False, None. Primetite da oni mogu biti u kombinaciji samo sa operatorima jednakosti, tako da možete pisati `published = False` ili `date_published = None`, ali `published >` False će izazvati grešku;
- Logički operatori: `and`, `or`;
- Operatori poredjenja: `=`, `!=`, `<`, `<=`, `>`, `>=` rade kao što se očekuje.
- `~` i `!~` test da li string ima ili nema substring (`icontains`);
- testna vrednost naspram liste: `in`, `not in`. Primer: `pk in (2, 3)`.

[Sadržaj](#sadržaj)

### DjangoQL šema

Šema defniniše ograničenja - šta možete uraditi sa `DjangoQL` upitom. Ako ne specificirate šemu, `DjangoQL` će vam pružiti default šemu. On će se kretati rekurzivno kroz polja modela i veze, i uključiti sve što nadje u šemi, tako da će korisnik moći da pretražuje preko svega.

Ponekad to nije ono što želite, bilo zbog db performansi bilo zbog bezbednosti. Ako bi želeli da ograničite search modela ili polja, trebalo bi da definišete šemu. Ovde je jedan primer:

```py
class UserQLSchema(DjangoQLSchema):
    exclude = (Book,)
    suggest_options = {
        Group: ['name'],
    }

    def get_fields(self, model):
        if model == Group:
            return ['name']
        return super(UserQLSchema, self).get_fields(model)

@admin.register(User)
class CustomUserAdmin(DjangoQLSearchMixin, UserAdmin):
    djangoql_schema = UserQLSchema
```

U primeru smo kreirali šemu koja radi tri stvari:

- isključuje model "Book" iz pretrage preko `exclude` opcije, takodje možete koristiti opciju `include`, koja ograničava pretragu samo po izlistanim modelima;

- ograničava raspoloživa polja za pretragu za Group model na polje `name`, u `get_fields()` metodi;

- omogućuje kompletirajuću opciju za Group names preko `suggest_options`.

  Primedba za `suggest_options`:
  - ona gleda za choices model polje parametre prvo,
  - ako nije spec., sinhrono uzima sve vrednosti za data model polja,
  - tako da bi trebali da sprečite velike querset-ove ovde.

    Ako želite da definišete prilagodjenu suggest_options, vidi ispod.

[Sadržaj](#sadržaj)

### Prilagodjena polja za pretragu

Dublje prilagodjenje pretrage može biti postignuto sa prilagodjenim `search` poljima. Prilagodjena `search` polja mogu biti korišćenja:

- za pretragu po annotacijama,
- za definisanje `suggest_options`, ili
- definisanju pune prilagodjene search logike.

U d`jangoql.schema`, DjangoQL definiše sledeća polja bazne klase koje mogu biti nasledjene za definisanje sopstvenog ponašanja:

- IntField
- FloatField
- StrFieldBoolField
- DateField
- DateTimeField
- RelationField

Ovde su primeri za česte slučajeve upotrebe:

[Sadržaj](#sadržaj)

#### Search po queryset annotacijama

```py
from djangoql.schema import DjangoQLSchema, IntField

class UserQLSchema(DjangoQLSchema):

    def get_fields(self, model):
        fields = super(UserQLSchema, self).get_fields(model)
        if model == User:
            fields += [IntField(name='groups_count')]
            return fields

@admin.register(User)
class CustomUserAdmin(DjangoQLSearchMixin, UserAdmin):
    djangoql_schema = UserQLSchema

    def get_queryset(self, request):
        qs = super(CustomUserAdmin, self).get_queryset(request)
        return qs.annotate(groups_count=Count('groups'))
```

Prvo smo dodali "groups_count" annotaciju na queryset koji koristi Django admin u
"CustomUserAdmin.get_queryset()" metodi. On bi trebao da sadrži broj grupa kojima jedan korisnik pripada.

Kada naš queryset sada podigne tu kolonu, možemo filtrirati po njoj. Samo treba da bude uključena u šemu.

U "UserQLSchema.get_fields()" definišemo prilagodjeno integer search polje za "User" model. Njegovo ime treba da je isto sa imenom kolone u queryset-u.

[Sadržaj](#sadržaj)

#### Prilagođene suggest_options

```py
from djangoql.schema import DjangoQLSchema, StrField

class GroupNameField(StrField):
    model = Groupname = 'name'
    suggest_options = True

    def get_options(self, search):
        return super(GroupNameField, self) \
    
    .get_options(search) \
        .annotate(users_count=Count('user'))\
            .order_by('-users_count')

class UserQLSchema(DjangoQLSchema):def get_fields(self, model):
    if model == Group:
        return ['id', GroupNameField()]
    return super(UserQLSchema, self).get_fields(model)

@admin.register(User)
class CustomUserAdmin(DjangoQLSearchMixin, UserAdmin):

djangoql_schema = UserQLSchema
```

U ovom primeru definisali smo prilagođeno GroupNameField koje sortira sugestije za grupu imena po popularnosti, (broj korisnika u grupi) umesto default alphabetical sortinga.

[Sadržaj](#sadržaj)

#### Prilagodjeni search lookup

DjangoQL bazna polja pružaju dva osnovna metoda koje možete prepisati radi zamene ili kolone za pretragu, vrednosti pretrage ili oba.

.get_lookup_name() i .get_lookup_value(value):

```py
class UserDateJoinedYear(IntField):name = 'date_joined_year'
def get_lookup_name(self): return 'date_joined year'

class UserQLSchema(DjangoQLSchema):def get_fields(self, model):
fields = super(UserQLSchema, self).get_fields(model)
if model == User:

fields += [UserDateJoinedYear()]return fields
@admin.register(User)
class CustomUserAdmin(DjangoQLSearchMixin, UserAdmin):djangoql_schema = UserQLSchema
```

U ovom primeru mi smo definisali prilagodjeno date_joined_year search polje za korisnike. I koristili smo ugradjeni Django year filter opciju u .get_lookup_name() za filtriranje po godini samo.

Slično možete koristiti .get_lookup_value(value) hook za promenu search vrednosti pre nego se koristi u filteru.

[Sadržaj](#sadržaj)

#### Puni prilagodjeni search lookup

.get_lookup_name() i .get_lookup_value(value) hooks pokrivaju više jednostavnih slučajeva korišćenja. Ali ponekad nisu dovoljni i vi želite punu prilagodjenu search logiku. U tim slučajevima možete nadjačati glavni .get_lookup() metod polja.

Primer ispod demonstrira pretragu po godinama korisnika:

```py
from djangoql.schema import DjangoQLSchema, IntFieldclass UserAgeField(IntField):
"""
Search by given number of full years
"""
model = Username = 'age'

def get_lookup_name(self):
    """
    We'll be doing comparisons vs. this model field
    """
    return 'date_joined'

def get_lookup(self, path, operator, value):
    """
    The lookup should support with all operators compatible with IntField
    """
    if operator == 'in':
        result = None
        for year in value:
            condition = self.get_lookup(path, '=', year)
            result = condition if result is None else result | condition
        return result

    elif operator == 'not in':result = None
        for year in value:
            condition = self.get_lookup(path, '!=', year)
            result = condition if result is None else result & condition
        return result
    
    value = self.get_lookup_value(value)
    search_field = ' '.join(path + [self.get_lookup_name()])
    year_start =  self.years_ago(value + 1)
    year_end = self.years_ago(value)
    
    if operator == '=':
        return ( Q(**{'%s gt' % search_field: year_start}) &Q(**{'%s lte' % search_field: year_end})
    )
    elif operator == '!=':
        return (Q(**{'%s lte' % search_field: year_start}) |Q(**{'%s gt' %
            search_field: year_end})
    )
    elif operator == '>':
        return Q(**{'%s lt' % search_field: year_start})
    elif operator == '>=':
        return Q(**{'%s lte' % search_field: year_end})
    elif operator == '<':
        return Q(**{'%s gt' % search_field: year_end})
    elif operator == '<=':
        return Q(**{'%s gte' % search_field: year_start})

def years_ago(self, n):
    timestamp = now() 
    
    try:
        return timestamp.replace(year=timestamp.year - n)
    except ValueError:
        February 29
    return timestamp.replace(month=2, day=28, year=timestamp.year - n)

class UserQLSchema(DjangoQLSchema):
    
    def get_fields(self, model):
        fields = super(UserQLSchema, self).get_fields(model)
        if model == User:
            fields += [UserAgeField()]return fields

@admin.register(User)
class CustomUserAdmin(DjangoQLSearchMixin, UserAdmin):
    djangoql_schema = UserQLSchema
```

[Sadržaj](#sadržaj)

### Mogu li koristiti Django QL van Django admina

Da. Možete dodati DjangoQL search funkcionalnost na bilo koji Django model korišćenjem
DjangoQLQuerySet:

```py
from django.db import models
from djangoql.queryset import DjangoQLQuerySet

class Book(models.Model):
    name = models.CharField(max_length=255)
    author = models.ForeignKey('auth.User')
    objects = DjangoQLQuerySet.as_manager()
```

Sa gornjim primerom možemo raditi pretraživanja kao:

```py
qs = Book.objects.djangoql(
    'name ~ "war" and author.last_name = "Tolstoy"'
)
```

Vraća normalni queryset, možete ga proširiti i ponovo koristiti ako je neophodno.

```py
print(qs.count())
```

Alternativno možete dodati DjangoQL search na svaki queryset, čak iako nije instanca `DjangoQLQuerySet`:

```py
from django.contrib.auth.models import User from djangoql.queryset import apply_search
qs = User.objects.all()
qs = apply_search(qs, 'groups = None') print(qs.exists())
```

Šema može bti spec. bilo kao `queryset` opcija, ili prosledjena `djangoql()` queryset metod direktno:

```py
class BookQuerySet(DjangoQLQuerySet):
    djangoql_schema = BookSchema

class Book(models.Model):
    ...
    objects = BookQuerySet.as_manager()
```

Sada, "Book.objects.djangoql()" će koristiti BookSchema po default-u:

```py
Book.objects.djangoql('name ~ "Peace") uses BookSchema
```

Prepisivanje default queryset šeme sa `AnotherSchema`:

```py
Book.objects.djangoql('name ~ "Peace", schema=AnotherSchema)
```

Možete obezbediti šemu kao opciju za `apply_search()`

```py
qs = User.objects.all()
Book.objects.djangoql('name ~ "Peace", schema=AnotherSchema)
```

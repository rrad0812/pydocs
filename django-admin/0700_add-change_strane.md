# Add/Change strane

## Sadržaj

[Nazad](sadrzaj.md)

- [Kako prikazati sliku iz Imagefield na Django adminu?](#kako-prikazati-sliku-iz-imagefield-na-django-adminu)
- [Kako povezati model sa trenutnim korisnikom koji upisuje/menja model?](#kako-povezati-model-sa-trenutnim-korisnikom-koji-upisujemenja-model)
- [Kako označiti polje kao readonly u adminu?](#kako-označiti-polje-kao-readonly-u-adminu)
- [Kako prikazati needitabilna polja u adminu?](#kako-prikazati-needitabilna-polja-u-adminu)
- [Kako napraviti polje editabilnim kada se objekat kreira, inače readonly?](#kako-napraviti-polje-editabilnim-kada-se-objekat-kreira-inače-readonly)
- [Kako filtrirati FK vrednosti iz padajućih lista u Django adminu?](#kako-filtrirati-fk-vrednosti-iz-padajućih-lista-u-django-adminu)
- [Kako upravljati modelom sa FK vezom prema modelu sa velikim brojem objekata?](#kako-upravljati-modelom-sa-fk-vezom-prema-modelu-sa-velikim-brojem-objekata)
- [Kako promeniti tekst na FK padajućoj listi?](#kako-promeniti-tekst-na-fk-padajućoj-listi)
- [Kako dodati prilagodjeno dugme na Django changeview stranu?](#kako-dodati-prilagodjeno-dugme-na-django-changeview-stranu)
- [Kako upravljati History/Model logovima u Django adminu?](#kako-upravljati-historymodel-logovima-u-django-adminu)
- [Kako prilagoditi add/change formu modela?](#kako-prilagoditi-addchange-formu-modela)
- [Kako nadjačati save ponašanje za admin?](#kako-nadjačati-save-ponašanje-za-django-admin)

### Kako prikazati sliku iz Imagefield na Django adminu?

U Hero modelu, imate jedno polje sa slikom:

```py
headshot = models.ImageField(null=True, blank=True, upload_to="hero_headshots/")
```

Od vas može biti traženo da promenite podrazumevano ponašanje i da se slika prikazuje na `changeview` strani:

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    readonly_fields = [..., "headshot_image"]

    def headshot_image(self, obj):
        return mark_safe('<img src="{url}" width="{width}" height={height} />'
            .format( url = obj.headshot.url, width=obj.headshot.width,
                height=obj.headshot.height, )
)
```

[Sadržaj](#sadržaj)

### Kako povezati model sa trenutnim korisnikom koji upisuje/menja model?

"Hero" model ima "added_by" polje:

```py
added_by = models.ForeignKey(settings.AUTH_USER_MODEL, null=True, blank=True,
    on_delete=models.SET_NULL)
```

Vi želite da "added_by" polje bude automatski postavljeno na trenutnog korisnika kada se objekat kreira u adminu:

```py
def save_model(self, request, obj, form, change):
    if not obj.pkobj.pk:
        # Only set added_by during the first save.
        obj.added_by = request.user
        super().save_model(request, obj, form, change)
```

Umesto toga, ako želite da sačuvate tekućeg korisnika prilikom promena zapisa:

```py
def save_model(self, request, obj, form, change):
obj.updated_by = request.user
super().save_model(request, obj, form, change)
```

Ako želite sa sakrijete "added_by", "updated_by" polja od prikazivanja na `changeview` strani:

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    exclude = ['added_by', 'updated_by',]
```

[Sadržaj](#sadržaj)

### Kako označiti polje kao readonly u adminu?

UMSRA je odlučila da privremeno zaustavi praćenje drveta familija mitoloških entiteta. Od vas je traženo da napravite father, mother i spouse polja readonly.

> [!Note]
>
> **Readonly** polja se mogu postaviti samo ona koja su na `listview` strani, tj., ne možete proglasiti polje `readonly` ako se ono ne prikazuje na `listview` strani.

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    readonly_fields = ["father", "mother", "spouse"]
```

[Sadržaj](#sadržaj)

### Kako prikazati needitabilna polja u adminu?

Ako imate polje sa `editable=False` u modelu, to je polje automatski sakriveno na vašem modelu. To se takodje dogadja sa poljima koja su markirana sa `auto_now` ili `auto_now_add`, jer su ovi atributi postavljaju na `editable=False`.

Ako želite da ta polja budu prikazana, možete ih dodati u `readonly_fields`:

```py
@admin.register(Villain)
class VillainAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    readonly_fields = ["added_on"]
```

[Sadržaj](#sadržaj)

### Kako napraviti polje editabilnim kada se objekat kreira, inače readonly?

Potrebno je, na primer, da polja "name" i "category" budu readonly pošto je objekat kreiran. Međutim, prilikom prvog upisivanja polja moraju se uređivati. To možete napraviti nadjačavanjem metode `get_readonly_fields`:

```py
def get_readonly_fields(self, request, obj=None):
    if obj:
        return ["name", "category"] else:
    return []
```

`obj` je `None` za vreme kreiranja, dok je za vreme editovanja postavljen.

[Sadržaj](#sadržaj)

### Kako filtrirati FK vrednosti iz padajućih lista u Django adminu?

"Hero" model ima `ForeignKey` vezu na "Category". Tako se svi category objekti pokazuju u padajućoj listi za polje "category". Ako umesto toga, želimo da se pojavi poskup, Django pruža mogućnost prilagodjavanja nadjačavanjem `formfield_for_foreignkey` metoda:

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    def formfield_for_foreignkey(self, db_field, request, **kwargs):
        if db_field.name == "category":
            kwargs["queryset"] = Category.objects.filter(name in=['God', 'Demi  
                God'])
        return super().formfield_for_foreignkey(db_field, request, **kwargs)
```

[Sadržaj](#sadržaj)

### Kako upravljati modelom sa FK vezom prema modelu sa velikim brojem objekata?

Možete kreirati veliki broj kategorija ovako:

```py
categories = [Category(**{"name": "cat-{}".format(i)}) for i in range(100000)]
Category.objects.bulk_create(categories)
```

Sada "Category" model ima više od 100000 objekata, kada odete na "Hero" admin, imaćete padajuću listu sa 100000 category izbora. To će napraviti stranicu sporom i padajuću listu teškom za korišćenje.

Možete promeniti "Hero" admin tako da za polje category uvedete u `raw_id_fields` listu:

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    raw_id_fields = ["category"]
```

[Sadržaj](#sadržaj)

### Kako promeniti tekst na FK padajućoj listi?

"Hero" model ima FK vezu na Category model. U padajućoj listi, umesto samo imena, možda želite da prikažete tekst: "Category: name".

Možete promeniti `str` metod na modelu "Category", ali vi želite samo promenu na adminu. Možete to uraditi nasledjivanjem `forms.ModelChoiceField` sa prilagodjenom `label_from_instance`:

```py
class CategoryChoiceField(forms.ModelChoiceField):

    def label_from_instance(self, obj):
        return "Category: {}".format(obj.name)
```

Sada možete nadjačati `formfield_for_foreignkey` za korišćenje novog tipa polja za polje "category":

```py
def formfield_for_foreignkey(self, db_field, request, **kwargs):
    if db_field.name == 'category':
        return CategoryChoiceField(queryset=Category.objects.all())
    return super().formfield_for_foreignkey(db_field, request, **kwargs)
```

[Sadržaj](#sadržaj)

### Kako dodati prilagodjeno dugme na Django changeview stranu?

Model Villain ima polje `is_unique`:

```py
class Villain(Entity):
...
is_unique = models.BooleanField(default=True)
```

Želite da dodate dugme na Villain `changelist` stranu sa nazivom “Napravi Unique”. Svi drugi villain objekti sa istim imenom trebalo bi obrisati.

Počinjemo proširenjem `change_form` šablona dodavanjem novog dugmeta

`(entities/vilian_changeform.html)`

```html
{% extends 'admin/change_form.html' %}
{% block submit_buttons_bottom %}
{{ block.super }}
<div class="submit-row">

<input type="submit" value="Make Unique" name="_make-unique">
</div>
{% endblock %}
{% block submit_buttons_bottom %}
{{ block.super }}

<div class="submit-row">

<input type="submit" value="Make Unique" name="_make-unique">
</div>
{% endblock %}
```

Tada možemo nadjačati response_change i povezati šablon sa VillainAdmin-om.:

```py
@admin.register(Villain)
class VillainAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    change_form_template = "entities/villain_changeform.html"
    def response_change(self, request, obj):
        if "_make-unique" in request.POST:
            matching_names_except_this =
                self.get_queryset(request).filter(name=obj.name)
                    .exclude(pk=obj.id)
            matching_names_except_this.delete()
        
            obj.is_unique = True
            obj.save()
            self.message_user(request, "This villain is now unique")
            return HttpResponseRedirect(".")
        return super().response_change(request, obj)
```

[Sadržaj](#sadržaj)

### Kako upravljati History/Model logovima u Django adminu?

Django history je jednostavna proširiva Django admin lokaciju koju možete koristiti. Naprimer, smeštanjem više odgovarajućih podataka o promenama napravljenim u Django adminu. Kreirajmo model:

```py
from django.db import models

class School(models.Model):
    school_name = models.CharField(max_length=30)
    address = models.CharField(max_length=30)
    principle_name = models.CharField(max_length=30)

    def str (self):
        return str(self.school_name)
```

Napravimo `makemigrations/migrate`

```shell
python manage.py makemigrations
python manage.py migrate
```

Registrujmo novi model na admin.

```py
from django.contrib import admin
from schoolapp.models import School

@admin.register(School)
class SchoolAdmin(admin.ModelAdmin):
    pass
```

Ako pogledate history stranu modela videćete koje su promene napravljene na objektu. Podrazumevao, smeštaju se samo promene napravljene na samom objektu. Recimo da želimo više podataka da sačuvamo o promenama:

```py
from django.contrib import admin
from django.contrib.admin.options import get_content_type_for_model
from django.contrib.admin.models import LogEntry, ADDITION
from schoolapp.models import School

@admin.register(School)
class SchoolAdmin(admin.ModelAdmin):

    def save_model(self, request, obj, form, change):
        super().save_model(request, obj, form, change)
        if change:
            change_message = '{} -{} -{}'.format(obj.school_name,
            obj.address, obj.principle_name)
            LogEntry.objects.create(

    user=request.user,content_type=get_content_type_for_model(obj),
    object_id=obj.id,action_flag=2,
    change_message=change_message,object_repr=obj. str ()[:200]
)
```

Mi smo nadjačali prodrazumevanu implementaciju save_model metoda za dati model.Ovo će sačuvati sve u change_message, što je vidljivo u action koloni.

[Sadržaj](#sadržaj)

### Kako prilagoditi add/change formu modela?

Forme koje se koriste za dodavanje ili promenu objekta modela zasnivaju se na ModelForm. Django automatski generiše formu na osnovu modela koji se uređuje.

Uređivanjem opcija možete da kontrolišete koja su polja uključena, kao i njihov redosled. Izmenite svoj PersonAdmin objekat, dodajući atribut fields:

```py
@admin.register(Person)
class PersonAdmin(admin.ModelAdmin):

fields = ("first_name", "last_name", "courses")
...
```

Stranice Add i Change za Person model sada stavljaju atribut first_name ispred last_name, iako sam model određuje obrnuto.

ModelAdmin.get_form() metod odgovoran je za kreiranje ModelForm-e za vaš objekat. Ovu metodu možete nadjačati da biste promenili formu. Dodajte sledeći kod u PersonAdmin:

```py
def get_form(self, request, obj=None, **kwargs):
    form = super().get_form(request, obj, **kwargs)
    form.base_fields["first_name"].label = "First Name (Humans only!):"
    return form
```

Sada, kada se prikaže stranica Add ili Change, oznaka polja first_name biće prilagođena.

Promena labele polja možda neće biti dovoljna da spreči vampire da se registruju kao studenti. Ako vam se ne sviđa ModelForm koji je Django admin kreirao za vas, tada možete da koristite form atribut da biste registrovali svoj prilagođeni obrazac.

Napravite sledeće dodatke i promene na core/admin.py:

```py
from django import forms

class PersonAdminForm(forms.ModelForm):

class Meta:
    model = Person
    fields = " all "
    
    def clean_first_name(self):
        if self.cleaned_data["first_name"] == "Spike"
            raise forms.ValidationError("No Vampires")
        return self.cleaned_data["first_name"]

@admin.register(Person)
class PersonAdmin(admin.ModelAdmin):
    form = PersonAdminForm
    ...
```

Gornji kod primenjuje dodatnu proveru valjanosti na stranicama Person Add i Change. Objekti ModelForm imaju bogat mehanizam za proveru valjanosti. U ovom slučaju, polje first_name se proverava prema imenu „Spike“. Greška ValidationError sprečava studente sa ovim imenom da se registruju. Nadjačavanjem ModelForm klase možete u potpunosti kontrolisati izgled i proveru valjanosti stranica koje koristite za stranice formi za dodavanje ili promenu objekata modela.

[Sadržaj](#sadržaj)

### Kako nadjačati save ponašanje za Django admin?

ModelAdmin ima save_model metod, koji se koristi za kreiranje i ažuriranje objekata modela. Njegovim nadjačavanjem, možemo prilagoditi ponašanje save za admin. Hero model ima sledeće polje:

```py
added_by = models.ForeignKey(settings.AUTH_USER_MODEL, null=True, blank=True,
    on_delete=models.SET_NULL)
```

Ako želite da uvek sačuvate tekućeg korisnika kada je Hero ažurirao, uradite:

```py
def save_model(self, request, obj, form, change):
    obj.added_by = request.user
    super().save_model(request, obj, form, change)
```

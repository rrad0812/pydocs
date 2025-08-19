# Admin akcije

## Sadržaj

[Nazad](sadrzaj.md)

- [Pisanje akcija](#pisanje-akcija)
  - [Pisanje akcionih funkcija](#pisanje-akcionih-funkcija)
  - [Dodavanje akcija u ModelAdmin](#dodavanje-akcija-u-modeladmin)
  - [Rukovanje greškama u akcijama](#rukovanje-greškama-u-akcijama)
  - [Akcije kao ModelAdmin metode](#akcije-kao-modeladmin-metode)
- [Omogućavanje-onemogućavanje akcija](#omogućavanje-onemogućavanje-akcija)
  - [Onemogućavanje akcije na celoj lokaciji](#onemogućavanje-akcije-na-celoj-lokaciji)
  - [Onemogućavanje svih akcija za određeni ModelAdmin](#onemogućavanje-svih-akcija-za-određeni-modeladmin)
  - [Uslovno omogućavanje ili onemogućavanje akcija](#uslovno-omogućavanje-ili-onemogućavanje-akcija)
- [Postavljanje dozvola za akcije](#postavljanje-dozvola-za-akcije)
- [Prilagodjene Django admin akcije sa prolaznom stranom](#prilagodjene-django-admin-akcije-sa-prolaznom-stranom)
  - [Dodavanje prolazne strane](#dodavanje-prolazne-strane)
  - [Dodavanje forme da izvrši našu akciju](#dodavanje-forme-da-izvrši-našu-akciju)
- [Admin akciju sa ili bez prolazne strane](#admin-akcija-sa-ili-bez-prolazne-strane)
  - [Admin akcija bez prolazne strane](#admin-akcija-bez-prolazne-strane)
  - [Admin akcija sa prolaznom stranom](#admin-akcija-sa-prolaznom-stranom)
- Kako dodati prilagodjenu akcijsku dugmad na listview stranu?
- Prilagodjene akcije na pojedinačnim objektima
- Prilagodjene akcije po objektu modela

Osnovni tok Django admina je, ukratko, "izaberite objekat, pa ga promenite, zatim gsačuvajte promene“. Ovo dobro funkcioniše za većinu slučajeva upotrebe. Međutim, ako trebate da napravite istu promenu na više objekata odjednom, ovaj tok posla može biti prilično dosadan. U tim slučajevima, Django admin vam omogućava da napišete i registrujete "akcije" - funkcije koje se pozivaju sa listom objekata izabranih na `listview` stranici.

Ako pogledate bilo koju listu zapisa u adminu, videćete ovu funkciju na delu, Django se isporučuje sa akcijom "brisanje izabranih objekata" dostupnom svim modelima.

> [!Warning]
>
> Akcija "brisanje izabranih objekata" koristi `QuerySet.delete()` iz razloga efikasnosti, što ima važnu posledicu: `delete()` metoda vašeg modela neće biti pozvana. Ako želite da zamenite ovo ponašanje, možete da zamenite `ModelAdmin.delete_queryset()` ili napišete prilagođenu akciju koja vrši brisanje na vaš omiljeni način na primer, pozivanjem `Model.delete()` svake od izabranih stavki.

### Pisanje akcija

Uobičajeni slučaj upotrebe za admin akcije je grupno ažuriranje modela. Zamislite aplikaciju za vesti sa `ArticleModel`:

```py
from django.db import models

STATUS_CHOICES = [
    ('d', 'Draft'),
    ('p', 'Published')
    ('w', 'Withdrawn'),
]

class Article(models.Model):
    title = models.CharField(max_length=100)
    body = models.TextField()
    status = models.CharField(max_length=1, choices=STATUS_CHOICES)
    
    def str (self) str (self):
        return self.title
```

Uobičajeni zadatak koji bismo mogli obaviti sa ovakvim modelom je ažuriranje statusa artikla sa "Draft" na "Published". To bismo lako mogli da radimo na admin zapisu, ali ako bismo želeli da promenimo grupu zapisa, bilo bi zamorno. Dakle, napišimo akciju koja nam omogućava da status članka promenimo u "Published".

[Sadržaj](#sadržaj)

#### Pisanje akcionih funkcija

Prvo ćemo morati da napišemo funkciju koja se poziva kada se akcija pokrene u adminu. Akcione funkcije su regularne funkcije koje uzimaju tri argumenta:

- `HttpRequest` koji predstavlja trenutni zahtev,
- `QuerySet` koji sadrži skup objekata koje je korisnik izabrao.
- `ModelAdmin` objekat,

Našoj funkciji za ažuriranje zapisa neće trebati `ModelAdmin objekt` ili `request` ali ćemo koristiti `queryset`:

```py
def make_published(modeladmin, request, queryset):
    queryset.update(status='p')
```

> [!Note]
>
> Za najbolje performanse koristimo metodu ažuriranja queryseta. Druge vrste
> akcija će možda trebati da se bave svakim objektom pojedinačno, u ovim
> slučajevima bismo iterirali preko queryseta:
>
> ```py
> for obj in queryset:
>     do_something_with(obj)
> ```  

To je zapravo sve što je potrebno za pisanje akcije! Međutim, preduzećemo još jedan neobavezan, ali koristan korak koji će dati akciji "lep" naslov u adminu. Podrazumevano, ova akcija bi se na listi akcija pojavila kao "Make published" sa donjim crtama zamenjenim razmacima. To je u redu, ali možemo pružiti bolje, čoveku prihvatljivije ime pomoću `action()` dekoratora i `make_published` funkcije:

```py
from django.contrib import admin

@admin.action(description='Mark selected stories as published')
def make_published(modeladmin, request, queryset):
    queryset.update(status='p')
```

> [!Note]
>
> Ovo bi moglo izgledati poznato, admin `list_display` opcija koristi sličnu tehniku sa `display()` dekoratorom da pruži čoveku čitljive opise za funkcije povratnog poziva koje su tamo takođe registrovane.  
>
> **Promenjeno Django v3.2**: `description` argument u `action()` dekoratoru ekvivalentan je postavljanju `short_description` atributa akcionoj funkciji u prethodnim verzijama. Direktno postavljanje atributa i dalje je podržano radi povratne kompatibilnosti.

[Sadržaj](#sadržaj)

#### Dodavanje akcija u ModelAdmin

Dalje, moraćemo da obavestimo ModelAdmin o akciji. Ovo funkcioniše kao i svaka druga opcija konfiguracije. Dakle, komplet `admin.py` sa akcijom i njenom registracijom bi izgledao ovako:

`admin.py`

```py
from django.contrib import admin
from myapp.models import Article

@admin.action(description='Mark selected stories as published')
def make_published(modeladmin, request, queryset):
    queryset.update(status='p')

class ArticleAdmin(admin.ModelAdmin):
    list_display = ['title', 'status']
    ordering = ['title']
    actions = [make_published]
```

[Sadržaj](#sadržaj)

#### Rukovanje greškama u akcijama

Ako postoje predvidivi uslovi greške koji se mogu pojaviti tokom pokretanja vaše akcije, trebali biste pažljivo obavestiti korisnika o problemu. To znači rukovanje izuzecima i korišćenje `django.contrib.admin.ModelAdmin.message_user()` za prikaz korisničkog opisa problema u odgovoru.

[Sadržaj](#sadržaj)

#### Akcije kao ModelAdmin metode

Gornji primer prikazuje `make_published` akciju definisanu kao funkcija. To je sasvim u redu, ali sa stanovišta dizajna koda nije savršeno: budući da je akcija čvrsto povezana sa `Article` objektom, ima smisla akciju povezati sa samim `ArticleAdmin` objektom.

```py
class ArticleAdmin(admin.ModelAdmin):
    ...
    actions = ['make_published']

    @admin.action(description='Mark selected stories as published')
    def make_published(self, request, queryset):
        queryset.update(status='p')
```

Preselili smo funkciju `make_published` u klasu `ModelAdmin`, prvi parametar je sada `self`, i na kraju `make_published` je string u listi actions, umesto direktne reference na callable. Ovo govori da je akcija ModelAdmin metoda.

Definisanje akcija kao metoda daje akciji idiomatičniji pristup ModelAdmin-a samog sebi, omogućavajući akciji da pozove bilo koju od metoda koje pruža admin.

Na primer, možemo koristiti self za fleširanje poruke korisniku obaveštavajući ga da je akcija uspela:

```py
from django.contrib import messages
from django.utils.translation import ngettext

class ArticleAdmin(admin.ModelAdmin):

    def make_published(self, request, queryset):
        updated = queryset.update(status='p')
        self.message_user(request, ngettext(
            '%d story was successfully marked as published.',
            '%d stories were successfully marked as published.', updated,
        ) % updated, messages.SUCCESS)
```

[Sadržaj](#sadržaj)

### Omogućavanje-onemogućavanje akcija

Neke akcije su najbolje ako su dostupne bilo kom objektu na admin veb lokaciji. Recimo akcija izvoza bila bi dobar kandidat. Možete da učinite akciju globalno dostupnom pomoću `AdminSite.add_action()`. Na primer:

```py
from django.contrib import admin

admin.site.add_action(export_selected_objects)
```

Ovo čini `export_selected_objects` akciju globalno dostupnom. Akciji možete dati izričito ime - ako kasnije želite programski promeniti ime akcije prosleđivanjem drugog argumenta `AdminSite.add_action()`:

```py
admin.site.add_action(export_selected_objects, 'export_selected')
```

[Sadržaj](#sadržaj)

#### Onemogućavanje akcije na celoj lokaciji

Ako želite da onemogućite akciju na celoj lokaciji, možete da pozovete `AdminSite.disable_action()`. Na primer, ovom metodom možete ukloniti ugrađenu akciju `"brisanje izabranih objekata"`:

```py
admin.site.disable_action('delete_selected')
```

Kada učinite gore navedeno, ta akcija više neće biti dostupna na celoj veb lokaciji. Ako, međutim, trebate da ponovo omogućite globalno onemogućenu aciju za jedan određeni model, navedite je u `ModelAdmin.actions` listi:

Ovaj ModelAdmin neće imati `delete_selected` raspoloživu klasu akciju:

```py
SomeModelAdmin(admin.ModelAdmin):
    actions = ['some_other_action']
    ...
```

A ovaj će imati

```py
class AnotherModelAdmin(admin.ModelAdmin):
    actions = ['delete_selected', 'a_third_action']
    ...
```

[Sadržaj](#sadržaj)

#### Onemogućavanje svih akcija za određeni ModelAdmin

Ako želite da nema dozvoljenih akcija na raspolaganju za dati ModelAdmin, postavite ModelAdmin.actions na None:

```py
class MyModelAdmin(admin.ModelAdmin):
    actions = None
```

Ovo govori da ModelAdmin ne prikazuje ili dozvoljavaja bilo kakve akcije korisnicima, uključujući bilo koje akcije na celoj lokaciji.

[Sadržaj](#sadržaj)

#### Uslovno omogućavanje ili onemogućavanje akcija

Konačno, možete uslovno da omogućite ili onemogućite akcije po zahtevu (a time i po korisniku) nadjačavanjem `ModelAdmin.get_actions()`. Ovo vraća rečnik dozvoljenih akcija. Ključevi su imena akcija, a vrednosti su slogovi.

Na primer, ako želite da korisnici čija imena počinju sa „J“ mogu da obrišu objekte u velikom broju:

```py
class MyModelAdmin(admin.ModelAdmin):
    ...
    def get_actions(self, request):
        actions = super().get_actions(request)
        if request.user.username[0].upper() != 'J':
            if 'delete_selected' in actions:
                del actions['delete_selected']
        return actions
```

[Sadržaj](#sadržaj)

#### Postavljanje dozvola za akcije

Akcije mogu biti ograničene na korisnike sa određenim dozvolama, umotavanjem funkcije akcije u `action()` dekorater i prosleđivanjem `permissions` argumenta:

```py
@admin.action(permissions=['change'])
def make_published(modeladmin, request, queryset):
    queryset.update(status='p')
```

`make_published()` akcija će biti dostupna samo korisnicima koji prođu `ModelAdmin.has_change_permission()` proveru. Ako permissions ima više dozvola, akcija će biti dostupna sve dok korisnik prođe bar jednu proveru.

Dostupne vrednosti za `permissions` i odgovarajuće provere metoda su:

- `'add'`: `ModelAdmin.has_add_permission()`
- `'change'`: `ModelAdmin.has_change_permission()`
- `'delete'`: `ModelAdmin.has_delete_permission()`
- `'view'`: `ModelAdmin.has_view_permission()`

Možete odrediti bilo koju drugu vrednost sve dok primenite odgovarajući metod na:

```py
ModelAdmin.has_<value>_permission(self, request)
```

Na primer:

```py
from django.contrib import admin
from django.contrib.auth import get_permission_codename

class ArticleAdmin(admin.ModelAdmin):actions = ['make_published']
    @admin.action(permissions=['publish'])
    def make_published(self, request, queryset):
        queryset.update(status='p')
    
    def has_publish_permission(self, request):
        """Does the user have the publish permission?"""opts = self.opts
        codename = get_permission_codename('publish', opts)
        return request.user.has_perm('%s.%s' % (opts.app_label, codename))
```

> [!Note]
>
> **Promenjeno u Django 3.2**: `permissions` argument u `action()` dekoratoru ekvivalentan je postavljanju `allowed_permissions` atributa akcionoj funkciji direktno u prethodnim verzijama.
>
> Direktno postavljanje atributa i dalje je podržano radi povratne kompatibilnosti.

[Sadržaj](#sadržaj)

### Prilagodjene Django admin akcije sa prolaznom stranom

Kreiranje akcije u Django adminu je vrlo jednostavno. Morate definisati funkciju koja je potom referencirana u ModelAdmin definiciji akcija. Ova funkcija če prihvatiti tri argumenta:

- ModelAdmin
- HTTP request
- queryset izabranih modela

OrderModel sa korisničkom akcijom koja ažurira status za izabrane stavke:

`admin.py`

```py
class OrderAdmin(admin.ModelAdmin):
    actions = ['update_status']
    
    def update_status(self, request, queryset):
        queryset.update(status='NEW_STATUS')
        update_status.short_description = "Update status"
```

`short_description` property se koristi za prilagodjenu labelu u padajućoj listi akcija.

[Sadržaj](#sadržaj)

#### Dodavanje prolazne strane

Da biste dodali prolaznu stranu, nastavićete sa gornjim kodom, ali umesto da odmah izvršite ažuriranje, tretiraćete ga kao prikaz i vratiti HTTP response koji uključuje novi obrazac i kontekst. Da bi počeli vratimo šablon sa praznim kontekstom:

`admin.py`

from django.shortcuts import render
class OrderAdmin(admin.ModelAdmin):

actions = ['update_status']
def update_status(self, request, queryset):

return render(request, 'admin/order_intermediate.html', context={})
update_status.short_description = "Update status"

Da zadržimo admin look & feel proširimo djangov admin osnovni šablon:

`admin/order_intermediate.html`

{% extends "admin/base_site.html" %}
{% block content %}
Are you sure you want to execute this action?
{% endblock %}

Pokušajte sa ovim kodom i bičete odvedeni na prolaznu stranu koja vas pita da li želite da izvršite izabranu
akciju. Medjutim ne želite opciju koja ne radi ništa. Kakoto da poboljšamo?

[Sadržaj](#sadržaj)

#### Dodavanje forme da izvrši našu akciju

Prvo dodajmo jednostavnu formu na naš šablon. Koristimo django form iz našeg konteksta, za sada pišemo kod ručno.Tri su važne form osobine koje morate da uključite:

1. action key, koji bi trebao da mapira na prilagodjenu akciju,
2. apply key, koji može biti bilo šta. Ovo je za hvatanje našeg submission kasnije u pogledu.
3. Izabrane stavke, u formi selected_action koji bi trebalo da su lista pk izabranih stavki.

Sada naš šablon izgleda ovako:

`'admin/order_intermediate.html'`

```html
{% extends "admin/base_site.html" %}
{% block content %}

<form action="" method="post">{% csrf_token %}
<p>
Are you sure you want to execute this action on the selected items?
</p>
{% for order in orders %}

<p>
{{ order }}

</p>
<input type="hidden" name="_selected_action" value="{{ order.pk }}" />

{% endfor %}
<input type="hidden" name="action" value="update_status" />
<input type="submit" name="apply" value="Update status"/>

{% endblock %}
```

Primetite da smo dodali hidden polja za _selected_action vrednosti kao i action property. Sada za hvatanje form submit-a, u našoj admin akciji:

`admin.py`

```py
from django.shortcuts import render
from django.http import HttpResponseRedirect
class OrderAdmin(admin.ModelAdmin):

actions = ['update_status']
def update_status(self, request, queryset):

    """All requests here will actually be of type POST
    so we will need to check for our special key
    'apply' rather than the actual request type"""

    if 'apply' in request.POST:
        # The user clicked submit on the intermediate form.
        # Perform our update action:
        queryset.update(status='NEW_STATUS')
        # Redirect to our admin view after our update has
        # completed with a nice little info message
        saying# our models have been updated:
        self.message_user(request,
            "Changed status on {} orders".format(queryset.count()))

        return HttpResponseRedirect(request.get_full_path())

    return render(request, 'admin/order_intermediate.html',
        context={'orders':queryset})
```

### Admin akcija sa ili bez prolazne strane

#### Admin akcija bez prolazne strane

Jedan od mojih projekata ima deo za parsiranje. Ponekad moram ručno ponovo da parsiramplejlistu (na primer kada ažuriram kod parsera nakon ispravke greške). Tako sam pronašao kul način da dodam metodu u Django ModelAdmin koja se može pokrenuti iz admina.

`your_app/admin.py`

```py
from django.contrib import admin
from .models import Playlist
from .tasks import parse_playlist
@admin.register(Playlist)
class PlaylistAdmin(admin.ModelAdmin):

actions = ['parse']
def parse(self, request, queryset):

queryset.update(is_parsed=False)
for playlist in queryset.all():

parse_playlist.delay(url=playlist.url)
self.message_user(request, "Playlists will be reparsed soon")
```

Interesantni delovi:

- Samo menjam Playlist.is_parsed polje na False, jer će moja lista biti parsirana uskoro.

queryset.update(is_parsed=False)

- Celery task parse_playlist i u toj liniji se task odlaže sa .delay() metodom.
parse_playlist.delay(url=playlist.url)

- Poruka korisniku koja če se pojaviti kada se parsiranje završi.
self.message_user()

#### Admin akcija sa prolaznom stranom

Dobro je pokretati taskove kao što sam prikazao gore. Ali postoje slučajevi kada treba da obezbedite više podataka da bi pokrenuli task. Treba da napišemo jedan broadcast metod koji če slati poruke nekim Telegram bot korisnicima.

Plan je: Izabrati korisnike, klikni na broadcast akciju, prikaži broadcast tekst poruku, pritisni Run i dozvoli pozadinskim taskovima da odrade posao.

Potrebno je kreirati prolaznu stranu sa formom podataka od Django admin korisnika. Treba kreirati/editovati fajlove aplikacije:

- your_app/forms.py,
- your_app/admin.py,
- your_app/templates/admin/broadcast_message.html.

`your_app/forms.py`

from django import
forms
from .models import Botclass
BroadcastForm(forms.Form):

_selected_action = forms.CharField(widget=forms.MultipleHiddenInput)
broadcast_text = forms.CharField(widget=forms.Textarea)
bot = forms.ModelChoiceField(Bot.objects)

Ovde je:

- _selected_action - Ovo če biti za transfer izabranih stavki podataka iz admina u kod.
- broadcast_text - Tekst brodcast poruke.
- bot - U bek-endu imam nekoliko Telegram botova.

Zato želim da izaberem Bot instancu koja će da brodkastuje poruku.

Fajl akcije treba da renderuje stranu sa formom gde ćete dodati parametre. Kada je prolazna strana napunjena
i korisnik pritisnuo < submit > dugme, treba da izdvojimoparametre iz POST request-a i pokrenemo željeni
pozadinski task.

`your_app/admin.py`

...
imports
@admin.register(User)
class UserAdmin(admin.ModelAdmin):

actions = ['broadcast'] register method as action

def broadcast(self, request, queryset):

if 'apply' in request.POST: if user pressed 'apply' on intermediate page
# Cool thing is that params will have the same names as in forms.py
broadcast_message_text = request.POST["broadcast_text"]
bot_id = request.POST["bot"]
# Get data from models using the params - maybe not the best way,
bot_token = Bot.objects.filter(id=bot_id).first().token
user_ids = [u.user_id for u in queryset]
# Run background task that will send broadcast messages
broadcast_message.delay(bot_token, user_ids, broadcast_message_text)
# Show alert that everything is coolself.
message_user(request,

"Broadcasting of %s messages has been started" %len(user_ids))
# Return to previous page
return HttpResponseRedirect(request.get_full_path())
# Create form and pass the data which objects were selected
# before triggering 'broadcast' action
# We create an intermediate page right here
form = BroadcastForm(initial={'_selected_action':

queryset.values_list('id', flat=True)})
# We need to create a template of intermediate

page
# with form - but this is really easy
return render(request, "admin/broadcast_message.html",

{'items': queryset, 'form': form})
<!-- my_django_app/templates/admin/broadcast_message.html
<!-- This block will be included to django admin basic template
{% extends "admin/base_site.html" %}
{% block content %}

<form action="" method="post">{% csrf_token %}
<!-- The code of the form with all input fields will be

automatically generated by Django -->
{{ form }}
<!-- Show the list of selected objects on the previous step-->
<p>Broadcast Message will be sent to these users:</p>
<ul>{{ items|unordered_list }}</ul>

<!-- Link the action name in hidden params
<input type="hidden" name="action" value="broadcast" />

<!-- Submit! Apply!
<input type="submit" name="apply" value="Send" />

</form>
{% endblock %}

Django forma će generisati polja forme - pogledaj {{ form }}. Ovo je najlepši deo ovog šablona. Možete copy-paste ovaj šablon za kreiranje više akcija.

Plan je:
1. Zadržati Django admin "look and feel"
2. Imati lepu admin poruku "X posts were successfully updated"

Direktno u ModelAdmin sekcji, kreiraj unutrašnju klasu za novu formu (za izbor taga), i funkciju za procesiranje forme kao u normalnom pogledu. Naravno, treba dodati novi metod u listu akcija.

actions = ['add_tag']
class AddTagForm(forms.Form):

_selected_action = forms.CharField(widget=forms.MultipleHiddenInput)
tag = forms.ModelChoiceField(Tag.objects)
def add_tag(self, request, queryset):form = None
if 'apply' in request.POST:

form = self.AddTagForm(request.POST)
if form.is_valid():

tag = form.cleaned_data['tag']
count = 0
for article in queryset: article.tags.add(tag) count += 1
plural = ''
if count != 1: plural = 's'
self.message_user(request,

"Successfully added tag %s to %d article%s." % (tag, count, plural))
return HttpResponseRedirect(request.get_full_path())

if not form:
form = self.AddTagForm(initial={'_selected_action': request.POST.

getlist(admin.ACTION_CHECKBOX_NAME)})
return render_to_response('admin/add_tag.html', {'articles': queryset,

'tag_form': form, })
add_tag.short_description = "Add tag to articles"

# I ovde je prolazna strana, koja koristi admin look & feel i locirana je u
#templates/admin/add_tag.html.

{% extends "admin/base_site.html" %}
{% block content %}

<p>Select tag to apply:</p>

<form action="" method="post">
{{ tag_form }}
<p>The tag will be applied to:</p>
<ul>{{ articles|unordered_list }}</ul>
<input type="hidden" name="action" value="add_tag" />
<input type="submit" name="apply" value="Apply tag" />

</form>
{% endblock %}

Sakriveno polje je tu da bi Django prepoznao form submission kao admin akciju.

### Kako dodati prilagodjenu akcijsku dugmad na listview stranu?

UMSRA je odlučila da su, ukoliko imaju dovoljno kriptonita, svi Heroji su smrtni. Međutim, žele da mogu da se predomisle i kažu da su svi heroji besmrtni. Od vas je zatraženo da dodate dva dugmeta - jedno koje sve heroje čini smrtnim i jedno koje čini sve besmrtnim.

Budući da utiče na sve junake, bez obzira na odabir, ovo mora biti zasebno dugme, a ne padajući meni akcije. Prvo ćemo promeniti šablon na HeroAdmin-u kako bismo mogli da dodamo dva dugmeta:

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

change_list_template =
"entities/heroes_changelist.html"

Tada ćemo nadjačati get_urls, i dodati set_immortal i set_mortal metode na ModelAdmin.

def get_urls(self):
urls = super().get_urls()
my_urls = [

path('immortal/', self.set_immortal),
path('mortal/', self.set_mortal),

]
return my_urls + urls

def set_immortal(self, request):
self.model.objects.all().update(is_immortal=True)
self.message_user(request, "All heroes are now immortal"
return HttpResponseRedirect("../")

def set_mortal(self, request):
self.model.objects.all().update(is_immortal=False)
self.message_user(request, "All heroes are now mortal")
return HttpResponseRedirect("../")

Na kraju, kreirajmo entities/heroes_changelist.html šablon proširenjem admin/change_list.html:

{% extends 'admin/change_list.html' %}
{% block object-tools %}

<div>
<form action="immortal/" method="POST"> {% csrf_token %}

<button type="submit">Make Immortal</button>
</form>

</div>
<br />
{{ block.super }}

{% endblock %}

### Prilagodjene akcije na pojedinačnim objektima

Grupne admin akcije su neefikasne na pojedinačnim objektima. Na primer, za brisanje jednog korisnika
treba da sledimo sledeće korake.

1. Izabrati objekat.
2. Klikuti na akcijsku padajuću listu.
3. Izabrati “Delete selected” akciju.
4. Kliknuti na Go dugme.
5. Potvrditi da objekat treba da bude obrisan.

Za brisanje jednog zapis imamo pet klikova. To je previše za jednu akciju. Da pojednostavimo proces, napravimo dugme na nivou vrste (objekta). To može biti postignuto pisanjem funkcije koja će insertovati delete dugme za svaki zapis.

ModelAdmin instance pružaju set imenovanih URL-ova za CRUD operacije. Dobijanje objekt url-a za jednu stranu biće:

{{ app_label }}_{{model_name }}_{{ page }}

Na primer, za dobijanje delete URL-a za book objekat možemo pozvati

reverse(‘admin:book_book_delete’, args=[book_id])

Možemo dodati delet edugme i dodati ga u list_display tako da je delete dugme raspoloživo za svaki pojedinačni objekat.

from django.contrib import admin
from django.utils.html import
format_htmlfrom book.models import Book
class BookAdmin(admin.ModelAdmin):

list_display = ('id', 'name', 'author', 'is_available', 'delete')
def delete(self, obj):

view_name = "admin:{}_{}_delete".format(obj._meta.app_label,
obj._meta.model_name)
link = reverse(view_name, args=[book.pk])
html = '<input type="button"
onclick="location.href=\'{}\'"value = "Delete"
/>'.format(link)
return format_html(html)

Sada u adminu imamo delete dugme za svaki pojedinačni objekat. Za brisanje jednog objekta je potreban klik na delete dugme, i potom drugi klik za potvrdu brisanja.

U prethodnom primeru mi smo koristili ugradjeni admin deletepogled. Možemo takodje napraviti prilagodjeni pogled i povezati ga sa prilagodjenom akcijom na individualni objekat. Na primer, možemo dodati dugme koje će markirati status knjige na raspoloživ.

### Prilagodjene akcije po objektu modela

Ponekad želite da izvršite odredjenu akciju samo na jednom objektu. ‘actions’ padajućalista pravi to mogućim, al je potrebno nekoliko klikova da bi se izvršila. Postoji i jednostavniji način, implementiranje hiperlinka za svaku vrstu pojedinačno sa željenom akcijom:

class PictureAdmin(admin.ModelAdmin):
    list_fields = (..., 'mail_link', )
    def mail_link(self, obj):
        dest = reverse('admin:myapp_pictures_mail_author',
        kwargs={'pk': obj.pk})
        return format_html(

'<a href="{url}">{title}</a>',url=dest, title='send mail')
mail_link.short_description = 'Show some love'
mail_link.allow_tags = True
def get_urls(self):

urls = [
url('^(?P<pk>\d+)/sendaletter/?$',

self.admin_site.admin_view(self.mail_view),
name='myapp_pictures_mail_author'),

]
return urls + super(PictureAdmin, self).get_urls()

def mail_view(self, request, *args, **kwargs):
obj = get_object_or_404(Picture, pk=kwargs['pk'])
send_mail('Feel the granny\'s love', 'Hey, she loves your pet!',

'granny@yoursite.com', [obj.author.email])
self.message_user(request, 'The letter is on its way')
return redirect(reverse('admin:myapp_picture_changelist'))

Link se pojavljuje pored svake vrste, na kraju, klikom na njega biće poslat mejl.

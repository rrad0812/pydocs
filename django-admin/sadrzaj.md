
# DJANGO ADMIN KUVAR

Prošireno internet izdanje

RADOSAV RADOVANOVIĆ
Lazarevac, avgust 2025.

## Sadržaj

- [Uvod](uvod.md)
  - [Prvi Django admin](uvod.md#prvi-django-admin)
  - [Modeli koji se koriste u ovom tutorijalu](uvod.md#modeli-koji-se-koriste-u-ovom-tutorijalu)

- [Generalno](generalno.md)  
  - [Kako promeniti 'Django administration' tekst?](generalno.md#kako-promeniti-django-administration-tekst)
  - [Kako postaviti naslov modela u množini?](generalno.md#kako-postaviti-naslov-modela-u-množini)
  - [Kako kreirati dva nezavisna sajta?](generalno.md#kako-kreirati-dva-nezavisna-sajta)
  - [Kako ukloniti podrazumevane aplikacije iz Django admina?](generalno.md#kako-ukloniti-podrazumevane-aplikacije-iz-django-admina)
  - [Kako dodati logo u Django admin?](generalno.md#kako-dodati-logo-u-django-admin)
  - [Kako nadjačati Django admin šablone?](generalno.md#kako-nadjačati-django-admin-šablone)
  - [Kako da postavimo prilagodjeni poredak aplikacija i modela?](generalno.md#kako-da-postavimo-prilagodjeni-poredak-aplikacija-i-modela)
  - [Kako dodati jedan model dva puta na admin?](generalno.md#kako-dodati-jedan-model-dva-puta-na-admin)
  - [Kako dodati db view na Django admin?](generalno.md#kako-dodati-db-view-na-django-admin)

- [Listview strane](listview_strane.md)
  - [Kako prikazati veliki broj zapisa na listview strani?](listview_strane.md#kako-prikazati-veliki-broj-zapisa-na-listview-strani)
  - [Kako ukinuti Django admin paginaciju?](listview_strane.md#kako-ukinuti-django-admin-paginaciju)
  - [Kako dodati datumski bazirano filtriranje na Django admin?](listview_strane.md#kako-dodati-datumski-bazirano-filtriranje-na-django-admin)
  - [Kako prikazati unutrašnju ManyToMany ili ManyToOne vezu na listview strani?](listview_strane.md#kako-prikazati-unutrašnju-manytomany-ili-manytoone-vezu-na-listview-strani)

- [Izračunata polja](izracunata_polja.md)
  - [Kako prikazati izračunato polje na listview strani?](izracunata_polja.md#kako-prikazati-izračunato-polje-na-listview-strani)
  - [Kako opitimizovati upite u Django adminu sa annotacijama?](izracunata_polja,md#kako-opitimizovati-upite-u-django-adminu-sa-annotacijama)
  - [Kako omogućiti sortiranje na izračunatim poljima?](izracunata_polja.md#kako-omogućiti-sortiranje-na-izračunatim-poljima)
  - [Kako omogućiti filtriranje po izračunatim poljima?](izracunata_polja.md#kako-omogućiti-filtriranje-po-izračunatim-poljima)
  - [Kako prikazati ’on’ ili ’off’ ikone umesto izračunatih boolean polja?](izracunata_polja.md#kako-prikazati-on-ili-off-ikone-umesto-izračunatih-boolean-polja)

- Polja za pretragu
  - Polja za pretragu
  - DjangoQL
    - Instalacija
    - Korišćenje DjangoQL sa standardnim Django admin search-om
    - Mogu li koristiti Django QL van Django admina

- Admin akcije
  - Pisanje akcija
    - Pisanje akcionih funkcija
    - Dodavanje akcija u ModelAdmin
    - Rukovanje greškama u akcijama
    - Akcije kao ModelAdmin metode
  - Omogućavanje-onemogućavanje akcija
    - Kako onemogućiti akciju na celoj lokaciji
    - Kako onemogućiti svih akcija za određeni ModelAdmin
    - Uslovno omogućavanje ili onemogućavanje akcija
  - Postavljanje dozvola za akcije
  - Prilagodjene Django admin akcije sa prolaznom stranom
    - Dodavanje prolazne strane
    - Dodavanje forme da izvrši našu akciju
  - Kako kreirati Django admin akciju sa ili bez prolazne strane
    - Django admin akcija bez prolazne strane
    - Django admin akcija sa prolaznom stranom
  - Kako dodati prilagodjenu akcijsku dugmad na listview stranu?
  - Prilagodjene akcije na pojedinačnim objektima
  - Prilagodjene akcije po objektu modela

- Add/Change strane
  - Kako prikazati sliku iz Imagefield na Django adminu?
  - Kako povezati model sa trenutnim korisnikom koji upisuje/menja model?
  - Kako označiti polje kao readonly u adminu?
  - Kako prikazati needitabilna polja u adminu?
  - Kako napraviti polje editabilnim kada se objekat kreira, inače readonly?
  - Kako filtrirati FK vrednosti iz padajućih lista u Django adminu?
  - Kako upravljati modelom sa FK vezom prema modelu sa velikim brojem objekata?
  - Kako promeniti tekst na FK padajućoj listi?
  - Kako dodati prilagodjeno dugme na Django changeview stranu?
  - Kako upravljati History/Model logovima u Django adminu?
  - Kako prilagoditi add/change formu modela?
  - Kako nadjačati save ponašanje za Django admin?

- Veze
  - Povezana polja na admin listview strani
  - Navigacija po povezanim poljima
    - to_parent_link
    - to_child_link
  - Pružanje veza do drugih stranica sa listama objekata modela
  - Reverzne veze
    - related_name
    - related_query_name
  - Autokompletiranje za povezana polja
  - Hiperlinkovi
  - Kako dobiti Django admin urls za specifične objekte?

- Inlines
  - Kako editovati višestruke modele iz jednog Django admina?
  - Kako dodati OneToOne vezu kao inline?
  - Kako dodati ugnježdene inlines u Django adminu?
  - Kako kreirati jedan Django admin iz dva različita modela?
  - Kako pristupiti parent instanci?
    - Odgovor_1:
    - Odgovor_2:
    - Odgovor_3:
    - Odgovor_4:
  - Kako dobiti objekt iz formfield_for_foreignkey?
    - Odgovor_1:
    - Odgovor_2:
    - Odgovor_3:
    - Odgovor_4:
    - Odgovor_5:
  - Pristup parent model instanci iz modelforme admin inline-a
    - Odgovor_1:
    - Odgovor_2:
    - Odgovor_3:
    - Odgovor_4:

- Filtriranje
  - Kako dodati osnovno filtriranje u interfejs?
  - Kako dodati prilagođeni filter u django admin?
  - Kako ulančati filtere u Django adminu?
  - Kako dodati podrazumevani filter u Django adminu?
  - Podrazumevani filteri - II način
  - Prilagodjeni tekst filteri
  - Prilagodjena lista filtera nad datumskim poljem
  - Napredni filteri

- Validacija Django prilagođene forme
  - Django-ova ugrađena validacija forme
    - Validacija pojedinačnog polja
    - Validacija više polja
    - Zaključak

- Djangove optimizacija QuerySet upita
  - select_related()
    - *fields parametri
    - depth parameter
    - *parameters
    - Zaključak
  - prefetch_related()
    - *lookups parameter
    - Prefetch objekat
    - Zaključak
  - prefetch_related() vs selected related()
    - Zaključak
  - Kombinovano korišćenje
  - Sve što treba da znate o prefetching-u u Django
    - Šta je problem?
    - prefetch_related
    - Uvod u Prefetch objekat

- Efikasna paginacija u Djangu i Postgresu
  - Razumevanje naivne paginacije
  - Predstava naivne paginacije
  - Rukovanje paginacijom u Djangu
  - Korišćenje dodatka dj-pagination za Django
  - Zaključak

- Kako skalirati Django admin
  - Evidentiranje
  - N + 1 problem
  - Nadjačavanje get_queryset
  - Poboljšani search sa DjangoQL
  - Elminisanje COUNT upita
  - show_full_result_count
  - deffer
  - Paginator
  - readonly_fields
  - Skaliranje Django admin date_hierarchy
    - Paket
    - date_hierarchy

- JavaScript u Django adminu
  - Događaji u inline formi
  - Kako raditi sa AJAX Request u Django
    - Kao Ajax radi u Django
  - Pogled na Django
    - Korišćenje jQuery za AJAX u Django
      - Zašto koristiti jQuery
      - Inicijalni setup
    - AJAX Request implementacija
    - Najbolje prakse za AJAX JavaScript u Django-u
  - Ajax request u Django adminu na edit strani
  - Izgradnja Ajax request-a od nule u Django admin-u

- CSV export-import
  - CSV Export iz Django admina?
  - CSV Import u Django admin
  - CSV Export preko komandne linije
  - CSV Import preko komandnde linije

- Dozvole
  - Dozvole za model
  - Kako proveriti dozvole
    - Kako primeniti dozvole
  - Django admin i dozvole za model
  - Primenite prilagođene poslovne uloge u Django Admin
    - Postavka prilagođenog User admina
    - Sprečavanje ažuriranja polja User modela
    - Uslovno sprečavanje ažuriranja polja
    - Sprečavanje ne superuser-a da daju superuser prava
    - Django admin User forma u dva koraka
    - Dodelite dozvole samo koristeći grupe
    - Sprečavanje ne superuser-a od promena njihovih vlastitih dozvola
    - Nadjačavanje dozvola na User modelu
    - Ograničite pristup prilagođenim akcijama na User modelu
    - Kako dozvoliti kreiranje samo jednog objekta u adminu?
    - Kako ukloniti ‘Add’/’Delete’ dugme iz admina?

- Kako napraviti Django admin sigurnijim?
  - Koristite SSL
  - Promenite URL
  - Koristite 'django-admin-honeypot'
  - Zahtevajte jače lozinke
  - Koristite dvofaktorsku autentifikaciju
  - Koristite najnoviju verziju Django-a
  - Nikada nemojte pokretati `DEBUG` u produkciji
  - Zapamtite svoje okruženje
  - Proverite da li postoje greške
  - Proverite veb sajt

- Prevodi
  - I18N Pregled
  - Početno podešavanje
    - Omogućavanje internacionalizacije
    - Promena lokalnih puteva
    - Middleware softver za automatski prevod
    - Prefiks jezika šablona URL-a
    - Ograničavanje izbor jezika
  - Prevodjenje
    - Gotove fraze
    - Prilagođeni admin naslovi
    - Imena aplikacija
    - Imena modela
    - Pokreni komande
  - Bonus: Dugmad za jezike
    - Prekidač prefiksa jezičkog koda
    - Prekidač prefiksa šablonskog filtera
    - Nadjačavanje Admin šablona
    - Završni prikaz

[Sadržaj](#sadržaj)

7 Admin akcije
Osnovni tok Django admina je, ukratko, „izaberite objekat, pa ga promenite“. Ovo dobro funkcioniše za
većinu slučajeva upotrebe. Međutim, ako trebate da napravite istu promenu na više objekata odjednom,
ovaj tok posla može biti prilično dosadan. U tim slučajevima, Django admin vam omogućava da napišete
i registrujete „akcije“ - funkcije koje se pozivaju sa listom objekata izabranih na stranici liste zapisa.

Ako pogledate bilo koju listu zapisa u adminu, videćete ovu funkciju na delu, Django se isporučuje sa
akcijom "brisanje izabranih objekata" dostupnom svim modelima.

Upozorenje: Akcija „brisanje izabranih objekata“ koristi QuerySet.delete() iz razloga efikasnosti, što ima
važnu posledicu: delete() metoda vašeg modela neće biti pozvana. Ako želite da zamenite ovo ponašanje,
možete da zamenite ModelAdmin.delete_queryset() ili napišete prilagođenu akciju koja vrši brisanje na
vaš omiljeni način na primer, pozivanjemModel.delete() svake od izabranih stavki.

Pisanje akcija
Uobičajeni slučaj upotrebe za admin akcije je grupno ažuriranje modela. Zamislite aplikaciju za vesti sa
ArticleModel-om:

from django.db import
models
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
Uobičajeni zadatak koji bismo mogli obaviti sa ovakvim modelom je ažuriranje statusa artukla sa
„Draft“ na „Published“. To bismo lako mogli da radimo u admin zapisu, ali ako bismo želeli da
promenimo grupu zapisa, bilo bi zamorno. Dakle, napišimo akciju koja nam omogućava da status
članka promenimo u „Published“:

7.1.1 Pisanje akcionih funkcija
Prvo ćemo morati da napišemo funkciju koja se poziva kada se akcija pokrene u adminu. Akcijske funkcije su
regularne funkcije koje uzimaju tri argumenta:

- HttpRequest koji predstavlja trenutni zahtev,
- QuerySet koji sadrži skup objekata koje je korisnik izabrao.
- ModelAdmin objekat,

Našoj funkciji za ažuriranje zapisa neće trebati ModelAdmin objekt ili request ali ćemo koristiti queryset:

def make_published(modeladmin, request, queryset):
queryset.update(status='p')



P a g e | 30

Beleška: Za najbolje performanse koristimo metodu ažuriranja queryseta. Druge vrste akcija će možda trebati
da se bave svakim objektom pojedinačno,u ovim slučajevima bismo prešli preko queryseta:

for obj in queryset:
do_something_with(obj)

To je zapravo sve što je potrebno za pisanje akcije! Međutim, preduzećemo još jedan neobavezan, ali
koristan korak koji će dati akciji „lep“ naslov u adminu. Podrazumevano, ova akcija bi se na listi akcija
pojavila kao „Make published“ sa donjim crtama zamenjenim razmacima. To je u redu, ali možemo pružiti
bolje, čoveku prihvatljivije ime pomoću action() dekoratora make_published funkcije:

from django.contrib import admin
...
@admin.action(description='Mark selected stories as published')

def make_published(modeladmin, request, queryset):
queryset.update(status='p')

Beleška: Ovo bi moglo izgledati poznato, admin list_display opcija koristi sličnu tehniku sa display()
dekoratorom da pruži čoveku čitljive opise za funkcije povratnog poziva koje su tamo takođe registrovane.

Promenjeno Django v3.2: description argument u action() dekoratoru ekvivalentan je postavljanju
short_description atributa akcionoj funkciji u prethodnim verzijama. Direktno postavljanje atributa i dalje je
podržano radi povratne kompatibilnosti.

7.1.2 Dodavanje akcija u ModelAdmin
Dalje, moraćemo da obavestimo ModelAdmin o akciji. Ovo funkcioniše kao i svaka druga opcija
konfiguracije. Dakle, komplet admin.py sa akcijom i njenom registracijom bi izgledao ovako:

#admin.py
from django.contrib import admin
from myapp.models import Article
@admin.action(description='Mark selected stories as published')
def make_published(modeladmin, request, queryset):

queryset.update(status='p').update(status='p')
class ArticleAdmin(admin.ModelAdmin):

list_display = ['title', 'status']ordering = ['title']
actions = [make_published]

7.1.3 Rukovanje greškama u akcijama
Ako postoje predvidivi uslovi greške koji se mogu pojaviti tokom pokretanja vaše akcije, trebali biste
pažljivo obavestiti korisnika o problemu. To znači rukovanjeizuzecima i korišćenje
django.contrib.admin.ModelAdmin.message_user() za prikaz korisničkog opisa problema u odgovoru.

7.1.4 Akcije kao ModelAdmin metode
Gornji primer prikazuje make_published akciju definisanu kao funkcija. To je sasvim u redu, ali sa
stanovišta dizajna koda nije savršeno: budući da je akcija čvrsto povezanasa Article objektom, ima smisla
akciju povezati sa samim ArticleAdmin objektom.



P a g e | 31

class ArticleAdmin(admin.ModelAdmin):
...
actions = ['make_published']
@admin.action(description='Mark selected stories as published')
def make_published(self, request, queryset):

queryset.update(status='p')

Preselili smo funkciju make_published u klasu ModelAdmin, prvi parametar je sada self, i na kraju

'make_published' je string u listi actions, umesto direktne reference na callable. Ovo govori da je akcija
ModelAdmin metoda.

Definisanje akcija kao metoda daje akciji idiomatičniji pristup ModelAdmin-a samog sebi, omogućavajući
akciji da pozove bilo koju od metoda koje pruža admin.

Na primer, možemo koristiti self za fleširanje poruke korisniku obaveštavajući ga da je akcija uspela:

from django.contrib import messages
from django.utils.translation import ngettext
class ArticleAdmin(admin.ModelAdmin):

...
def make_published(self, request, queryset):

updated = queryset.update(status='p')
self.message_user(request, ngettext(

'%d story was successfully marked as published.',
'%d stories were successfully marked as published.', updated,

) % updated, messages.SUCCESS)

Omogućavanje-onemogućavanje akcija
Neke akcije su najbolje ako su dostupne bilo kom objektu na admin veb lokaciji gore opisana akcija
izvoza bila bi dobar kandidat. Možete da učinite akciju globalno dostupnom pomoću
AdminSite.add_action(). Na primer:

from django.contrib import admin
admin.site.add_action(export_selected_objects)

Ovo čini export_selected_objects akciju globalno dostupnom kao akciju pod nazivom
„export_selected_objects“. Akciji možete dati izričito ime - ako kasnije želite programski promeniti ime
akcije prosleđivanjem drugog argumenta AdminSite.add_action():

admin.site.add_action(export_selected_objects, 'export_selected')

7.2.1 Onemogućavanje akcije na celoj lokaciji
Ako želite da onemogućite akciju na celoj lokaciji, možete da pozovete AdminSite.disable_action(). Na
primer, ovom metodom možete ukloniti ugrađenu akciju „brisanje izabranih objekata“:

admin.site.disable_action('delete_selected')

Kada učinite gore navedeno, ta akcija više neće biti dostupna na celoj veb lokaciji. Ako, međutim, trebate



P a g e | 32
da ponovo omogućite globalno onemogućenu aciju za jedan određeni model, navedite je u
ModelAdmin.actions listi:

Ovaj ModelAdmin neće imate delete_selected raspoloživu klasu akciju:

SomeModelAdmin(admin.ModelAdmin):
actions = ['some_other_action']
...

A ovaj će imati

class AnotherModelAdmin(admin.ModelAdmin):
actions = ['delete_selected', 'a_third_action']
...

7.2.2 Onemogućavanje svih akcija za određeni ModelAdmin
Ako želite da nema dozvoljenih akcija na raspolaganju za dati ModelAdmin, postavite ModelAdmin.actions
na None:

class MyModelAdmin(admin.ModelAdmin):
actions = None

Ovo govori ModelAdmin da ne prikazuje ili dozvoljavaja bilo kakve akcije korisnicima, uključujući bilo koje
akcije na celoj lokaciji.

7.2.3 Uslovno omogućavanje ili onemogućavanje akcija
Konačno, možete uslovno da omogućite ili onemogućite akcije po zahtevu (a time i po korisniku)
nadjačavanjemModelAdmin.get_actions(). Ovo vraća rečnik dozvoljenih akcija. Ključevi su imena akcija, a
vrednosti su slogovi.

Na primer, ako želite samo da korisnici čija imena počinju sa „J“ mogu da obrišu objekte u velikom broju:

class MyModelAdmin(admin.ModelAdmin):
...
def get_actions(self, request):

actions = super().get_actions(request)
if request.user.username[0].upper() != 'J':

if 'delete_selected' in actions:
del actions['delete_selected']
return actions

Postavljanje dozvola za akcije
Akcije mogu biti ograničene na korisnike sa određenim dozvolama, umotavanjem funkcije akcije u action()
dekorater i prosleđivanjem permissions argumenta:

@admin.action(permissions=['change'])
def make_published(modeladmin, request, queryset):

queryset.update(status='p')
make_published() akcija će biti dostupna samo korisnicima koji prođu
ModelAdmin.has_change_permission() proveru. Ako permissions ima više dozvola,akcija će biti dostupna



P a g e | 33
sve dok korisnik prođe bar jednu proveru.

Dostupne vrednosti za permissions i odgovarajuće provere metoda su:
- 'add': ModelAdmin.has_add_permission()
- 'change': ModelAdmin.has_change_permission()
- 'delete': ModelAdmin.has_delete_permission()
- 'view': ModelAdmin.has_view_permission()

Možete odrediti bilo koju drugu vrednost sve dok primenite odgovarajući metod na:

ModelAdmin.has_<value>_permission(self, request)

Na primer:

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

Promenjeno u Django 3.2: permissions argument u action() dekoratoru ekvivalentan je postavljanju
allowed_permissions atributa akcionoj funkciji direktno u prethodnim verzijama.

Direktno postavljanje atributa i dalje je podržano radi povratne kompatibilnosti.

Prilagodjene Django admin akcije sa prolaznom stranom
Kreiranje akcije u Django adminu je vrlo jednostavno. Morate definisati funkciju koja je potom referencirana
u model admin definiciji akcija. Ova funkcija če prihvatiti tri argumenta:

1. ModelAdmin
2. HTTP request
3. queryset izabranih modela

OrderModel sa korisničkom akcijom koja ažurira status za izabrane stavke:

# admin.py
class OrderAdmin(admin.ModelAdmin):

actions = ['update_status']
def update_status(self, request, queryset):

queryset.update(status='NEW_STATUS')
update_status.short_description = "Update status"

short_description property se koristi za prilagodjenu labelu u padajućoj listi akcija.



P a g e | 34

7.4.1 Dodavanje prolazne strane
Da biste dodali prolaznu stranu, nastavićete sa gornjim kodom, ali umesto da odmah izvršite ažuriranje,
tretiraćete ga kao prikaz i vratiti HTTP response koji uključuje novi obrazac i kontekst. Da bi počeli
vratimo šablon sa praznim kontekstom:

# admin.py
from django.shortcuts import render
class OrderAdmin(admin.ModelAdmin):

actions = ['update_status']
def update_status(self, request, queryset):

return render(request, 'admin/order_intermediate.html', context={})
update_status.short_description = "Update status"

Da zadržimo admin look & feel proširimo djangov admin osnovni šablon:

#admin/order_intermediate.html
{% extends "admin/base_site.html" %}
{% block content %}
Are you sure you want to execute this action?
{% endblock %}

Pokušajte sa ovim kodom i bičete odvedeni na prolaznu stranu koja vas pita da li želite da izvršite izabranu
akciju. Medjutim ne želite opciju koja ne radi ništa. Kakoto da poboljšamo?

7.4.2 Dodavanje forme da izvrši našu akciju
Prvo dodajmo jednostavnu formu na naš šablon. Koristimo django form iz našeg konteksta, za sada pišemo
kod ručno.Tri su važne form osobine koje morate da uključite:

1. action key, koji bi trebao da mapira na prilagodjenu akciju,
2. apply key, koji može biti bilo šta. Ovo je za hvatanje našeg submission kasnije u pogledu.
3. Izabrane stavke, u formi selected_action koji bi trebalo da su lista pk izabranih stavki.

Sada naš šablon izgleda ovako:

# 'admin/order_intermediate.html'
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



P a g e | 35
</form>

{% endblock %}

Primetite da smo dodali hidden polja za _selected_action vrednosti kao i action property. Sada za
hvatanje form submit-a, u našoj admin akciji:

# admin.py
from django.shortcuts import render
from django.http import HttpResponseRedirect
class OrderAdmin(admin.ModelAdmin):

actions = ['update_status']
def update_status(self, request, queryset):

# All requests here will actually be of type POST
# so we will need to check for our special key
'apply'# rather than the actual request type
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

Kako kreirati Django admin akciju sa ili bez prolazne strane –
II Django admin akcija bez prolazne strane

Jedan od mojih projekata ima deo za parsiranje. Ponekad moram ručno ponovo da parsiramplejlistu (na
primer kada ažuriram kod parsera nakon ispravke greške). Tako sam pronašao kul način da dodam
metodu u Django ModelAdmin koja se može pokrenuti iz admina.

# your_app/admin.py
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

Interesantni delovi:
- Samo menjam Playlist.is_parsed polje na False, jer će moja lista biti parsirana uskoro.



P a g e | 36
queryset.update(is_parsed=False)

- Celery task parse_playlist i u toj liniji se task odlaže sa .delay() metodom.
parse_playlist.delay(url=playlist.url)

- Poruka korisniku koja če se pojaviti kada se parsiranje završi.
self.message_user()

7.5.1 Django admin akcija sa prolaznom stranom
Dobro je pokretati taskove kao što sam prikazao gore. Ali postoje slučajevi kada treba da obezbedite više
podataka da bi pokrenuli task. Treba da napišemo jedan broadcast metod koji če slati poruke nekim
Telegram
bot korisnicima.

Plan je: Izabrati korisnike, klikni na broadcast akciju, prikaži broadcast tekst poruku, pritisni Run i dozvoli
pozadinskim taskovima da odrade posao.

Potrebno je kreirati prolaznu stranu sa formom podataka od Django admin korisnika. Treba kreirati/editovati
fajlove aplikacije:

- your_app/forms.py,
- your_app/admin.py,
- your_app/templates/admin/broadcast_message.html.

# your_app/forms.py
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

# your_app/admin.py
...
imports
@admin.register(User)
class UserAdmin(admin.ModelAdmin):

actions = ['broadcast'] register method as action



P a g e | 37
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

Django forma će generisati polja forme - pogledaj {{ form }}. Ovo je najlepši deo ovog šablona. Možete
copy-paste ovaj šablon za kreiranje više akcija.



P a g e | 38

Plan je:
1. Zadržati Django admin "look and feel"
2. Imati lepu admin poruku "X posts were successfully updated"

Direktno u ModelAdmin sekcji, kreiraj unutrašnju klasu za novu formu (za izbor taga), i funkciju za
procesiranje forme kao u normalnom pogledu. Naravno, treba dodati novi metod u listu akcija.
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



P a g e | 39

Kako dodati prilagodjenu akcijsku dugmad na listview stranu?
UMSRA je odlučila da su, ukoliko imaju dovoljno kriptonita, svi Heroji su smrtni. Međutim, žele da
mogu da se predomisle i kažu da su svi heroji besmrtni. Od vas je zatraženo da dodate dva dugmeta -
jedno koje sve heroje čini smrtnim i jedno koje čini sve besmrtnim.

Budući da utiče na sve junake, bez obzira na odabir, ovo mora biti zasebno dugme, a ne padajući meni
akcije. Prvo ćemo promeniti šablon na HeroAdmin-u kako bismo mogli da dodamo dva dugmeta:

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

Prilagodjene akcije na pojedinačnim objektima
Grupne admin akcije su neefikasne na pojedinačnim objektima. Na primer, za brisanje jednog korisnika
treba da sledimo sledeće korake.

1. Izabrati objekat.
2. Klikuti na akcijsku padajuću listu.
3. Izabrati “Delete selected” akciju.



P a g e | 40

4. Kliknuti na Go dugme.
5. Potvrditi da objekat treba da bude obrisan.

Za brisanje jednog zapis imamo pet klikova. To je previše za jednu akciju. Da pojednostavimo proces,
napravimo dugme na nivou vrste (objekta). To može biti postignuto pisanjem funkcije koja će insertovati
delete dugme za svaki zapis.

ModelAdmin instance pružaju set imenovanih URL-ova za CRUD operacije. Dobijanje objekt url-a za jednu
stranu biće:

{{ app_label }}_{{model_name }}_{{ page }}

Na primer, za dobijanje delete URL-a za book objekat možemo pozvati

reverse(‘admin:book_book_delete’, args=[book_id])
Možemo dodati delet edugme i dodati ga u list_display tako da je delete dugme raspoloživo za svaki
pojedinačni objekat.

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

Sada u adminu imamo delete dugme za svaki pojedinačni objekat. Za brisanje jednog objekta je potreban klik
na deletedugme, i potom drugi klik za potvrdu brisanja.

U prethodnom primeru mi smo koristili ugradjeni admin deletepogled. Možemo takodje napraviti
prilagodjeni pogled i povezati ga sa prilagodjenom akcijom na individualni objekat. Na primer, možemo
dodati dugme koje će markirati status knjige na raspoloživ.

Prilagodjene akcije po objektu modela
Ponekad želite da izvršite odredjenu akciju samo na jednom objektu. ‘actions’ padajućalista pravi to
mogućim, al je potrebno nekoliko klikova da bi se izvršila. Postoji i jednostavniji način, implementiranje
hiperlinka za svaku vrstu pojedinačno sa željenomakcijom:

class PictureAdmin(admin.ModelAdmin):
list_fields = (..., 'mail_link', )
def mail_link(self, obj):



P a g e | 41
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



P a g e | 42

8 Add/Change strane
Kako prikazati sliku iz Imagefield na Django adminu?

U Hero modelu, imate jedno polje sa slikom:

headshot = models.ImageField(null=True, blank=True, upload_to="hero_headshots/")
Od vas može biti traženo da promenite podrazumevano ponašanje i da se slika prikazuje na changeview strani:

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

readonly_fields = [..., "headshot_image"]
def headshot_image(self, obj):

return mark_safe('<img src="{url}" width="{width}" height={height} />'
.format( url = obj.headshot.url, width=obj.headshot.width,

height=obj.headshot.height, )
)

Kako povezati model sa trenutnim korisnikom koji upisuje/menja model?
Hero model ima added_by polje:

added_by = models.ForeignKey(settings.AUTH_USER_MODEL, null=True, blank=True,
on_delete=models.SET_NULL)

Vi želite da added_by polje bude automatski postavljeno na trenutnog korisnika kada se objekat kreira u
adminu:

def save_model(self, request, obj, form, change):
if not obj.pkobj.pk:

# Only set added_by during the first save.
obj.added_by = request.user
super().save_model(request, obj, form, change)

Umesto toga, ako želite da sačuvate tekućeg korisnika prilikom promena zapisa:

def save_model(self, request, obj, form, change):
obj.updated_by = request.user
super().save_model(request, obj, form, change)

Ako želite sa sakrijete added_by, updated_by polja od prikazivanja na changeview formi:

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

...
exclude = ['added_by',’updated_by’,]

Kako označiti polje kao readonly u adminu?
UMSRA je odlučila da privremeno zaustavi praćenje drveta familija mitoloških entiteta.Od vas je traženo
da napravite father, mother i spouse polja readonly.

Napomena: Readonly polja se mogu postaviti samo ona koja su u list_view listi, tj., ne možete proglasiti polje



P a g e | 43
readonly ako se ono ne prikazuje.

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

...
readonly_fields = ["father", "mother", "spouse"]

Kako prikazati needitabilna polja u adminu?
Ako imate polje sa editable=False u modelu, to je polje automatski sakriveno na vašem modelu. To se
takodje dogadja sa poljima koja su markirana sa auto_now ili auto_now_add, jer ih ovi atributi
postavljaju na editable=False.

Ako želite da ta polja budu prikazana, možete ih dodati u readonly_fields:

@admin.register(Villain)
class VillainAdmin(admin.ModelAdmin, ExportCsvMixin):

...
readonly_fields = ["added_on"]

Kako napraviti polje editabilnim kada se objekat kreira, inače readonly?
Potrebno je da polja name i category budu readonly pošto je objekat kreiran. Međutim, prilikom prvog
upisivanja polja moraju se uređivati. To možete napraviti nadjačavanjem metode get_readonly_fields:

def get_readonly_fields(self, request, obj=None):
if obj:

return ["name", "category"] else:
return []

obj je None za vreme kreiranja, do je za vreme editovanja postavljen.

Kako filtrirati FK vrednosti iz padajućih lista u Django adminu?
Hero model ima FK vezu na Category. Tako se svi category objekti pokazuju u padajućoj listi za polje
category. Ako umesto toga, želimo da se pojavi poskup, Django pruža mogućnost prilagodjavanja
nadjačavanjem formfield_for_foreignkey metoda:

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

...
def formfield_for_foreignkey(self, db_field, request, **kwargs):

if db_field.name == "category":
kwargs["queryset"] = Category.objects.filter(name in=['God', 'Demi God'])

return super().formfield_for_foreignkey(db_field, request, **kwargs)

Kako upravljati modelom sa FK vezom prema modelu sa velikim brojem
objekata?

Možete kreirati veliki broj kategorija ovako:

categories = [Category(**{"name": "cat-{}".format(i)}) for i in range(100000)]
Category.objects.bulk_create(categories)



P a g e | 44
Sada Category model ima više od 100000 objekata, kada odete na Hero admin, imaćete padajuću listu sa
100000 category izbora. To će napraviti stranicu sporom i padajuću listu teškom za korišćenje.

Možete promeniti Hero admin tako da za polje category uvedete u raw_id_fields listu:

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

...
raw_id_fields = ["category"]

Kako promeniti tekst na FK padajućoj listi?
Hero model ima FK vezu na Category model. U padajućoj listi, umesto samo imena, moždaželite da

prikažete tekst: ’Category: <name>’.

Možete promeniti str metod na modelu Category, ali vi želite samo promenu na adminu. Možete
to uraditi nasledjivanjem forms.ModelChoiceField sa prilagodjenom label_from_instance:

class CategoryChoiceField(forms.ModelChoiceField):
def label_from_instance(self, obj):

return "Category: {}".format(obj.name)
Sada možete nadjačati formfield_for_foreignkey za korišćenje novog tipa polja za polje category:

def formfield_for_foreignkey(self, db_field, request, **kwargs):
if db_field.name == 'category':

return CategoryChoiceField(queryset=Category.objects.all())
return super().formfield_for_foreignkey(db_field, request, **kwargs)

Kako dodati prilagodjeno dugme na Django changeview stranu?
Model Villain ima polje is_unique:

class Villain(Entity):
...
is_unique = models.BooleanField(default=True)

Želite da dodate dugme na Villain changelist stranu sa nazivom “Napravi Unique”. Svi drugi villain
objekti sa istim imenom trebalo bi obrisati.

Počinjemo proširenjem change_form šablona dodavanjem novog dugmet
(entities/vilian_changeform.html):

{% extends 'admin/change_form.html' %}
{% block submit_buttons_bottom %}
{{ block.super }}
<div class="submit-row">

<input type="submit" value="Make Unique" name="_make-unique">
</div>
{% endblock %}
{% block submit_buttons_bottom %}
{{ block.super }}



P a g e | 45
<div class="submit-row">

<input type="submit" value="Make Unique" name="_make-unique">
</div>
{% endblock %}

Tada možemo nadjačati response_change i povezati šablon sa VillainAdmin-om.:

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

Kako upravljati History/Model logovima u Django adminu?
Django history je jednostavna proširiva Django admin lokaciju koju možete koristiti. Naprimer,
smeštanjem više odgovarajućih podataka o promenama napravljenim u Django adminu. Kreirajmo model:

from django.db import models
class School(models.Model):

school_name = models.CharField(max_length=30)
address = models.CharField(max_length=30)
principle_name = models.CharField(max_length=30)
def str (self):

return str(self.school_name)

Napravimomakemigrations/migrate

python manage.py makemigrations
python manage.py migrate
Registrujmo novi model na admin.

from django.contrib import admin
from schoolapp.models import School
@admin.register(School)
class SchoolAdmin(admin.ModelAdmin):

pass

Ako pogledate history stranu modela videćete koje su promene napravljene na objektu. Podrazumevao,



P a g e | 46
smeštaju se samo promene napravljene na samom objektu. Recimo da želimoviše podataka da sačuvamo o
promenama:

from django.contrib import admin
from django.contrib.admin.options import
get_content_type_for_modelfrom django.contrib.admin.models import
LogEntry, ADDITION
from schoolapp.models import School
@admin.register(School)
class SchoolAdmin(admin.ModelAdmin):

def save_model(self, request, obj, form, change):
super().save_model(request, obj, form, change)
if change:

change_message = '{} -{} -{}'.format(obj.school_name,
obj.address, obj.principle_name)
LogEntry.objects.create(

user=request.user,
content_type=get_content_type_for_model(obj),
object_id=obj.id,
action_flag=2,
change_message=change_message,
object_repr=obj. str ()[:200]

)

Mi smo nadjačali prodrazumevanu implementaciju save_model metoda za dati model.Ovo će
sačuvati

sve u change_message, što je vidljivo u action koloni.

Kako prilagoditi add/change formu modela?
Forme koje se koriste za dodavanje ili promenu objekta modela zasnivaju se na ModelForm. Django
automatski generiše formu na osnovu modela koji se uređuje.

Uređivanjem opcija možete da kontrolišete koja su polja uključena, kao i njihov redosled. Izmenite svoj
PersonAdmin objekat, dodajući atribut fields:

@admin.register(Person)
class PersonAdmin(admin.ModelAdmin):

fields = ("first_name", "last_name", "courses")
...

Stranice Add i Change za Person model sada stavljaju atribut first_name ispred last_name, iako sam model
određuje obrnuto.

ModelAdmin.get_form() metod odgovoran je za kreiranje ModelForm-e za vaš objekat. Ovu metodu
možete nadjačati da biste promenili formu. Dodajte sledeći kod u PersonAdmin:

def get_form(self, request, obj=None, **kwargs):
form = super().get_form(request, obj, **kwargs)
form.base_fields["first_name"].label = "First Name (Humans only!):"
return form

Sada, kada se prikaže stranica Add ili Change, oznaka polja first_name biće prilagođena.



P a g e | 47

Promena labele polja možda neće biti dovoljna da spreči vampire da se registruju kao studenti. Ako
vam se ne sviđa ModelForm koji je Django admin kreirao za vas, tada možete da koristite form atribut
da biste registrovali svoj prilagođeni obrazac.

Napravite sledeće dodatke i promene na core/admin.py:
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

Gornji kod primenjuje dodatnu proveru valjanosti na stranicama Person Add i Change. Objekti
ModelForm imaju bogat mehanizam za proveru valjanosti. U ovom slučaju, polje first_name se
proverava prema imenu „Spike“. Greška ValidationError sprečava studente sa ovim imenom da se
registruju. Nadjačavanjem ModelForm klase možete u potpunosti kontrolisati izgled i proveru
valjanosti stranica koje koristite za stranice formi za dodavanje ili promenu objekata modela.

Kako nadjačati save ponašanje za Django admin?
ModelAdmin ima save_model metod, koji se koristi za kreiranje i ažuriranje objekata modela. Njegovim
nadjačavanjem, možemo prilagoditi ponašanje save za admin. Hero model ima sledeće polje:

added_by = models.ForeignKey(settings.AUTH_USER_MODEL, null=True, blank=True,
on_delete=models.SET_NULL)

Ako želite da uvek sačuvate tekućeg korisnika kada je Hero ažurirao, uradite:

def save_model(self, request, obj, form, change):
obj.added_by = request.user
super().save_model(request, obj, form, change)



P a g e | 48

9 Veze
Povezana polja na admin listview strani

Django admin ima ModelAdmin klasu koja pruža opcije i funkcionalnosti za modele u adminu. To su
list_display, list_filter, search_fields za specificiranje polja za odgovarajuću akciju.

search_fields, list_filter i druge opcije takodje pružaju uključivanje ForeignKey ili ManyToMany polja sa
lookup API notacijom. Na primer, da bi pretražili po polju name BestSellerAdminmodela, možemo
specificirati book name u search_fields.

from django.contrib import admin from book.models import BestSeller
class BestSellerAdmin(RelatedFieldAdmin):search_fields = ('book name', )

list_display = ('id', 'year', 'rank', 'book')
admin.site.register(Bestseller, BestsellerAdmin)

Primetite da je BestSellerAdmin klasa nasledjena iz RelatedFieldAdmin klase.

Django ne dozvoljava istu notaciju u list_display opciji. Da bi uključili ForeignKeypolje iliManyToMany
polje u list_display, moramo pisati prilagodjenu metodu i dodati je u list_display.

from django.contrib import admin
from book.models import BestSeller
class BestSellerAdmin(RelatedFieldAdmin):

list_display = ('id', 'rank', 'year', 'book', 'author')
search_fields = ('book name', )
def author(self, obj):

return obj.book.author
author.description = 'Author'

Dodavanje ForeignKey na taj način u list_display postaje zamorno, pogotovu ako je više polja sa ForeignKey
na povezane modele. Možemo napisati prilagodjenu klasu za dinamičko postavljanje atributa kao metode tako
da možemo postaviti ForeignKey polja u list_display.

def get_related_field(name, admin_order_field=None,short_description=None):related_names
= name.split(' ')
def dynamic_attribute(obj):

for related_name in related_names:
obj = getattr(obj, related_name)

return obj
dynamic_attribute.admin_order_field = admin_order_field or name
dynamic_attribute.short_description = short_description or

related_names[-1].title().replace('_', ' ')
return dynamic_attribute

class RelatedFieldAdmin(admin.ModelAdmin):
def getattr (self, attr):

if ' ' in attr:
return get_related_field(attr)

return self. getattribute (attr)



P a g e | 49

class BestSellerAdmin(RelatedFieldAdmin):
list_display = ('id', 'rank', 'year', 'book','book author')

Nasledjivanjem klase RelatedFieldAdmin, možemo direktno koristiti ForeignKey polja u
list_display. Medjutim to će dovesti do N+1 problema.

https://github.com/theatlantic/django-nested-admin.
Navigacija po povezanim poljima

Ponekad može biti korisno brzo kretanje između objekata. Nakon pokušaja da naučimo osoblje za podršku
da filtrira pomoću parametara URL-a, konačno smo odustali i stvorili dva jednostavna dekoratora.

9.2.1 to_parent_link
Napravite vezu do stranice povezanog modela:

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

Dekorator će prikazati vezu ( <a href="..."> ... </a> ) do povezanog modela. Ako na primer želimo da
dodamo vezu sa svakog proizvoda na stranicu sa detaljima o kategoriji, koristimo dekorater ovako:

@admin.register(models.Product)
class ProductAdmin(admin.ModelAdmin):

list_display = ( 'id', 'name', 'category_link', )



P a g e | 50
admin_select_related = ('category',)
@to_parent_link('category', _('Category'))
def category_link(self, category):

return category

9.2.2 to_child_link
Komplikovanije veze poput „svi proizvodi kategorije“ zahtevaju drugačiji pristup. Napravili smo dekorator
koji prihvata niz upita i povezuje se na prikaz liste povezanog modela:

def admin_changelist_url(model):
app_label = model._meta.app_label
model_name = model. name .lower()
return reverse('admin:{}_{}_changelist'.format(app_label, model_name))

def to_child_link(attr, short_description, empty_description='-', query_string=None):
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
field_func.allow_tags =
Truereturn field_func

return wrap

Da bismo dodali vezu iz kategorije u svoje proizvode, u CategoryAdmin uradimo sledeće:

@admin.register(models.Category)
class CategoryAdmin(admin.ModelAdmin):

list_display = ('id', 'name', 'products_link', )
@to_child_link('products', _('Products'), query_string=lambda c:
'category_id={}'.format(c.pk))
def products_link(self, products):

return _('Products')
Budite oprezni sa argumentom proizvoda. Veoma je primamljivo učiniti nešto poput ovog:



P a g e | 51
Loš primer:

@to_child_link('products',('Products'),
query_string=lambda c:'category_id={}'.format(c.pk))

def products_link(self, products):
“ Dont do that! “
return 'see {} products'.format(products.count())

Gornji primer rezultiraće dodatnim upitima.

Pružanje veza do drugih stranica sa listama objekata modela
Uobičajeno je da se objekti pozivaju na druge objekte pomoću stranih ključeva. List_display možete
usmeriti na metodu koja vraća HTML vezu. Unutar core/admin.py izmenite klasu CourseAdmin na sledeći
način:
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

Ovaj kod dovodi do toga da lista objekata modela Course ima tri kolone:
- Naziv kursa
- Godina u kojoj je kurs ponuđen
- Veza koja prikazuje broj studenata na kursu

Kada kliknete na 2 učenika, preusmerava vas na stranicu sa listom objekata modela Person sa
primenjenim filterom. Filtrirana stranica prikazuje samo studente na odgovarajućem kursu: Psich 101,
Buffy i Villow. Xander se nije prijavio na ovaj kurs.

Primer koda koristi reverse()metodu za dobijanje je URLadrese u Django adminu. Možete potražiti bilo koju
stranicu admina koristeći sledeću konvenciju:

"admin:%(app)s_%(model)s_%(page)"

Ova struktura deli se na sledeći način:
- admin: je prostor imena.
- app: je naziv aplikacije.
- model: je objekt modela.
- page: je tip stranice u Django adminu.

Za gornji primer view_students_link() koristite admin: core_person_changelist da biste dobili referencu na
stranicu liste objekta modela Personu core aplikacije.Evo dostupnih naziva URL-ova:



P a g e | 52

Strana URL Ime Namena
Listview %(app)s\_%(model)s\_changelist changelist Lista objekata modela
Addview %(app)s\_%(model)s\_add add Forma za dodavanje novog objekta modela
Historyview %(app)s\_%(model)s\_history history Strana sa istorijom promeneobjekta modela
Deleteview %(app)s\_%(model)s\_delete delete Uzima obj_id kao parameter
Changeview %(app)s\_%(model)s\_change change Uzima obj_id kao parametar

Stranicu liste objekata možete da filtrirate dodavanjem stringa na URL-u. Ovaj stringupita modifikuje
QuerySet koji se koristi za popunjavanje stranice. U gornjem primeru, string upita „?courses id =
{obj.id}“ filtrira listu osoba samo prema onim objektima koji imaju odgovarajuću vrednost u polju
Person.course.

Ovi filteri podržavaju pretragu polja QuerySet pomoću dvostrukih donjih crta (__). Možete pristupiti
atributima povezanih objekata, kao i koristiti modifikatore filtera kao što su exact i startswith.

Reverzne veze
9.4.1 related_name
related_name atribut u ForeignKey poljima je jako koristan. Daje nam mogućnost da definišemo značajno
ime za reverznu vezu.

Pravilo: Ako niste sigurni šta bi trebalo da bude related_name, koristite množinu imena modela.

class Company:
name = models.CharField(max_length=30)

class Employee:
first_name = models.CharField(max_length=30)
last_name = models.CharField(max_length=30)
company = models.ForeignKey(Company,
on_delete=models.CASCADE,related_name='employees')

To znači da Company model će imati poseban atribut employees, koji će vraćati QuerySet sa svim zaposlenim
instancama povezanih sa kompanijom.

google = Company.objects.get(name='Google')
google.employees.all()

Možete koristiti reverznu vezu za modifikovanje company polja na Employee instancama:

vitor = Employee.objects.get(first_name='Vitor')
google = Company.objects.get(name='Google')
google.employees.add(vitor)

9.4.2 related_query_name
Ova se vrsta veze primenjuje i na filterske upite. Na primer, ako želim da izlistam sve kompanije koji
zapošljavaju ljude sa imenom ‘Vitor’, uradiću sledeće:

companies = Company.objects.filter(employee first_name='Vitor')



P a g e | 53

Ako želite sa promenite ime veze, evo kako da to uradite:
class Employee:

first_name = models.CharField(max_length=30)
last_name = models.CharField(max_length=30)
company = models.ForeignKey(Company, on_delete=models.CASCADE,

related_name='employees', related_query_name='person'
)

Tada bi korišćenje bilo:

companies = Company.objects.filter(person first_name='Vitor')

Da bi bilo konzistentno related_name ide u množini a related_query_name ide u jednini.

Autokompletiranje za povezana polja
Hajdemo u BookAdmin i pokušajmo da dodamo novu knjigu. Da bi popravili podrazumevano
ponašanje, možemo pružiti autokompletirajuću opciju za polje author tako da korisnicimogu
pretraživati i izabrati

potrebnog autora.

from book.models import Book
class AuthorAdmin(admin.ModelAdmin):

search_fields = ('name',)
class BookAdmin(admin.ModelAdmin):

list_display = ('id', 'name', 'author')
autocomplete_fields = ('author',)

admin.site.register(Book, BookAdmin)

ModelAdmin pruža autocomplete_fields opciju za select2 autokompletirajući ulaz. Takodje treba da
definišemo search_fields na povezanom polju modela tako da admin može da pretražuje po njemu.

Hiperlinkovi
Hajde da pregledamo BookAdmin i izaberimo neku od knjiga.

from django.contrib import
admin
from .models import Book
class BookAdmin(admin.ModelAdmin):

list_display = ('id', 'name', 'author')
admin.site.register(Book, BookAdmin)

Ovde je book name polje povezano sa changeview stranom objekta book. Ali author polje je prikazano
kao običan tekst. Ako bi smo želeli da predjemo u odgovarajući objekat autoraradi promene njegovog
imena, morali bi da se vratimo ma kućnu stranicu, izaberemo list stranu modela autora, nadjemo
odgovarajućeg i tada preko linka odela autora, nadjemo odgovarajućeg i tada preko linka predjemo na



P a g e | 54
stranicu za promene i promenimo ime autora.

Ovo izgleda kao prilično dosadan niz akcija, pogotovu ako ih treba ponoviti više puta. Ali, ako bi imali
hiperlinkna polju autor koji bi vodo direktno na stranu za promene autora, to bi bilo znatno lakše za rad.

Django pruža opciju za pristup admin pogledima njegovim URL reverzirajućim sistemom. Naprimer,
možemo dobiti stranu za promene autorovog objekta u admin book modelu korišćenjem sintakse:

reverse(“admin:model_povezani_model_change”, args=id)

Primer:
from django.contrib import admin
from django.utils.safestring import mark_safe
class BookAdmin(admin.ModelAdmin):
list_display = ('name', 'author_link', )

def author_link(self, book):
url = reverse("admin:book_author_change",args=[book.author.id])
link = '<a href="%s">%s</a>' % (url, book.author.name)
return mark_safe(link)

author_link.short_description =
'Author'

Sada u BookAdmin pogledu, author polje će biti hiperlinkovano na changeview stranu AuthorAdmin modela,
možemo je posetiti jednostavnim klikom. Zavisno odzahteva, možemo linkovati bilo koje polje u django sa

drugim poljem ili dodati prilagodjeno polje.

Kako dobiti Django admin urls za specifične objekte?
Imate children kolonu koja prikazuje imena dece svakog od Hero objekata. Od vas se traži da linkujete svako
od dece na njegovu changeview stranu.

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

...
def children_display(self, obj):

display_text = ", ".join([
"<a
href={}>{}</a>".format( reverse('admin:{}_{}_change'.format(obj._meta.app_labe
l,

obj._meta.model_name), args=(child.pk,)), child.name)
for child in obj.children.all()

])
if display_text:

return mark_safe(display_text)return "-"

Ovde su opcije:

Delete: reverse('admin:{}_{}_delete' .format(obj._meta.app_label, obj._meta.model_name),
args=(child.pk,))
History: reverse('admin:{}_{}_history' .format(obj._meta.app_label, obj._meta.model_name),



P a g e | 55
args=(child.pk,))
Change: reverse('admin:{}_{}_change' .format(obj._meta.app_label, obj._meta.model_name),
args=(child.pk,))



P a g e | 56

10 Inlines
Kako editovati višestruke modele iz jednog Django admina?

Ukratko, treba da koristite inlines. Imate Category model, i potrebno je da dodajete i editujete Villain
modele unutar admina za Category:

class VillainInline(admin.StackedInline):
model = Villain

@admin.register(Category)
class CategoryAdmin(admin.ModelAdmin):

...
inlines = [VillainInline]

Vidite da je forma za dodavanje i editovanje Villain unutar Category admina. Ako Inline model ima više
polja, koristite StackedInline inače koristite TabularInline.

Kako dodati OneToOne vezu kao inline?
OneToOne veza može biti postavljena na isti način kao i ManyToOne. Ali, samo jedna strana OneToOne
veze može biti postavljena kao inline.

Možete imati HeroAcquaintance model sa OneToOne vezom na Hero korišćenjem:

class HeroAcquaintance(models.Model):
"""Non family contacts of a Hero"""
hero = models.OneToOneField(Hero, on_delete=models.CASCADE)
....

Možete dodati kao inline na Hero kao:

class HeroAcquaintanceInline(admin.TabularInline):
model = HeroAcquaintance

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

...
inlines = [HeroAcquaintanceInline]
inlines = [HeroAcquaintanceInline]

Kako dodati ugnježdene inlines u Django adminu?
Imate modele ovako definisane:

class Category(models.Model):
...

class Hero(models.Model):
category = models.ForeignKey(Catgeory)
...

class HeroAcquaintance(models.Model):
hero = models.OneToOneField(Hero, on_delete=models.CASCADE)

Želite jednu admin stranu za kreiranje Category, Hero i HeroAcquaintance objekata. Django ne podržava



P a g e | 57
umetnute inline sa ManyToOne ili OneToOne vezama koje povezuju više od jednog nivoa. Imate malo opcija:

- Možete promeniti HeroAcquaintance model, tako da imate direktnu FK vezu na Category:

class HeroAcquaintance(models.Model):
hero = models.OneToOneField(Hero, on_delete=models.CASCADE)
category = models.ForeignKey(Category)
def save(self, *args, **kwargs):

self.category =
self.hero.category
super().save(*args, **kwargs)

Potom možete povezati HeroAcquaintanceInline na CategoryAdmin, i dobiti neku
vrstu umetnutog inline.

- Alternativno, postoje gotovi Django add-on apps, koje pružaju umetnute inlines.
Pogledaj na Github ili DjangoPackages.

Kako kreirati jedan Django admin iz dva različita modela?
Hero ima FK vezu na Category, tako da možete izabrati Category iz Hero admina. Ako želite da kreirate
Category objekat iz Heroa dmina, možete promeniti formu Hero admina i prilagoditi ponašanje
save_model:

class HeroForm(forms.ModelForm):
category_name = forms.CharField()
class Meta:

model = Hero
exclude = ["category"]

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

form = HeroForm
....
def save_model(self, request, obj, form, change):

category_name = form.cleaned_data["category_name"]
category, _ = Category.objects.get_or_create(name=category_name)
obj.category = category
super().save_model(request, obj, form, change)

Na ovaj način Hero admin može da kreira i ažurira Category model.

Kako pristupiti parent instanci?
Moj cilj je da nadjačam has_add_permission funkciju bazirano na vrednosti polja statusparentinstance. Ja ne
želim da dozvolim dodavanje dečijih instanci ako je status parent instance različit od 1.

class ChildInline(admin.TabularInline):
model = Child
form = ChildForm
fields = (

...



P a g e | 58
)
extra = 0
def has_add_permission(self, request):

# Return True only if the parent has status == 1
# How to get to the parent instance?
#return True

class ParentAdmin(admin.ModelAdmin):
inlines = [ChildInline,]

10.5.1 Odgovor_1:
Za dobijanje parent objekta iskoristiti podatke iz reqeust. resolve funkcija iz django.urls modula vraća
podatke koji su nama interesantni. Funkcioniše kada su argumenti prisutni u zahtevu.

from django.contrib import admin
from django.urls import resolve
from app.models import YourParentModel, YourInlineModel
class YourInlineModelInline(admin.StackedInline):

model = YourInlineModel
def get_parent_object_from_request(self, request):

resolved = resolve(request.path_info)
if resolved.args:

return self.parent_model.objects.get(pk=resolved.args[0])
return None

def has_add_permission(self, request):
parent = self.get_parent_object_from_request(request)
# Validate that the parent status is active (1)
if parent:

return parent.status == 1
# No parent - return original has_add_permission() check
return super(YourInlineModelInline, self).has_add_permission(request)

@admin.register(YourParentModel)
class YourParentModelAdmin(admin.ModelAdmin):

inlines = [YourInlineModelInline]

10.5.2 Odgovor_2:
Da bi došli do parent_obj u child klasi, nadjačamo get_formset, postavljanjući parent_obj kao prosledjeni obj
datog metoda.
class ChildInline(admin.TabularInline):

model = Child
form = ChildForm
fields = (...)
extra = 0
def get_formset(self, request, obj=None, **kwargs):

self.parent_obj = obj
return super(ChildInline, self).get_formset(request, obj, **kwargs)



P a g e | 59
def has_add_permission(self, request):

return self.parent_obj.status == 1
class ParentAdmin(admin.ModelAdmin):

inlines = [ChildInline, ]

10.5.3 Odgovor_3:
Samo treba da dodate obj parametar i proverite parent model status:
class ChildInline(admin.TabularInline):

model = Child
form = ChildForm
fields = (

...
)
extra = 0
def has_add_permission(self, request, obj=None):

if obj.status == 1:
return True

else:
return False

class ParentAdmin(admin.ModelAdmin):
inlines = [ChildInline,]

10.5.4 Odgovor_4:
Pokušao sam predloženo ali ne radi za mene, morao sam da koristim varijantu iz Odgovor_1:
...
def get_parent_object_from_request(self, request):

resolved = resolve(request.path_info)
if resolved.kwargs:

return self.parent_model.objects.get(pk=resolved.kwargs["object_id"])
return None

...

Kako dobiti objekt iz formfield_for_foreignkey?
Pokušao sam da filtriram opcije prikazane u foreignkey polju, unutar django admin inlajna, zbog njihovog
editovanja.

class ProjectGroupMembershipInline(admin.StackedInline):
model = ProjectGroupMembership
extra = 1
formset = ProjectGroupMembershipInlineFormSet
form = ProjectGroupMembershipInlineForm
def formfield_for_foreignkey(self, db_field, request=None, **kwargs):

if db_field.name == 'group':
kwargs['queryset'] = Group.objects.filter(

some_filtering_here=object_being_edited)
return super(ProjectGroupMembershipInline, self).

formfield_for_foreignkey(db_field, request, **kwargs))

Verifikovao sam da je kwargs prazan kada se edituje objekat, tako da ne mogu dobiti objekat odatle. Pomoć?



P a g e | 60

10.6.1 Odgovor_1:
Da bi filtrirali izbore raspložive u polju stranog ključa admin inlajna, treba nadjačati formu tako da bude
moguć apdejt atributa queryset polja forme. Na taj način možete pristupiti self.instance što je objekat koji se
edituje u formi.
class ProjectGroupMembershipInlineForm(forms.ModelForm):

def init (self, *args, **kwargs):
super(ProjectGroupMembershipInlineForm, self). init (*args, **kwargs)
self.fields['group'].queryset = Group.objects.filter(

some_filtering_here=self.instance)

Nema potrebe da koristite formfield_for_foreignkey.

10.6.2 Odgovor_2:
Drugi način, malo čistiji, sličan gornjem odgovoru:
class ProjectGroupMembershipInline(admin.StackedInline):

# irrelevant bits....
def formfield_for_foreignkey(self, db_field, request, **kwargs):

if db_field.name == "group":
try:

parent_id = request.resolver_match.args[0]
kwargs["queryset"] = Group.objects.filter(some_column=parent_id)

except IndexError:
pass

return super().formfield_for_foreignkey(db_field, request, **kwargs)

10.6.3 Odgovor_3:
Moguće je to rešiti korišćenjem formfield_for_foreignkey i uklanjanjem object ID-a iz url-a. Nije najleši
način dobijnja ID-a ali Django ne obezbeđuje pristup object ID-u.
class ObjectAdmin(admin.ModelAdmin):

def formfield_for_foreignkey(self, db_field, request, **kwargs):
obj_id = request.META['PATH_INFO'].rstrip('/').split('/')[-1]
if db_field.name == 'my_field' and obj_id.isdigit():

obj = self.get_object(request, obj_id)
if obj:

kwargs['queryset'] = models.Object.objects.filter(field=obj)
return super().formfield_for_foreignkey(db_field, request, **kwargs)

10.6.4 Odgovor_4:
Django smešta parent id u kwargs a ne u args tako da korektan je sledeći način:

parent_id = request.resolver_match.kwargs.get('object_id')

10.6.5 Odgovor_5:
Ovo je rešenje do koga sam došao

def formfield_for_foreignkey(self, db_field, request, **kwargs):
if db_field.name == "group":

parent_id = request.resolver_match.kwargs['object_id']
kwargs["queryset"] = Group.objects.filter(some_column=parent_id)
return super().formfield_for_foreignkey(db_field, request, **kwargs)



P a g e | 61

Pristup parent model instanci iz modelforme admin inline-a
Koristim TabularInline u Django adminu, konfigurisan da prikaže jednu extra blank formu.

class MyChildInline(admin.TabularInline):
model = MyChildModel
form = MyChildInlineForm
extra = 1

Modeli su MyParentModel->MyChildModel->MyInlineForm. Koristim prilagođenu formu ako da dinamički
raim lookup i popunjavam lookup polja:

class MyChildInlineForm(ModelForm):
my_choice_field = forms.ChoiceField()
def init (self, *args, **kwargs):

super(MyChildInlineForm, self). init (*args, **kwargs)
# Lookup ID of parent model. parent_id = None
if "parent_id" in kwargs:

parent_id = kwargs.pop("parent_id")
elif

self.instance.parent_id:
parent_id = self.instance.parent_id

elif self.is_bound:
parent_id = self.data['%s-parent'% self.prefix]

if parent_id:
parent = MyParentModel.objects.get(id=parent_id)

if rev:
qs = parent.get_choices()
self.fields['my_choice_field'].choices = [(r.name,r.value) for r in qs]

Ovo radi dobro za inline zapise u edit modu, ali za ekstra blank formu ne prikazuje vrednosti u mome lookup
polju, pošto nema još zapisa i ne može lookup pridruženi MyParentModel zapis. Kako da pristupim parent
objektu u tom slučaju?

10.7.1 Odgovor_1:
Od Django 1.9, tu je get_form_kwargs(self, index)metod u BaseFormSet klasi.

class MyFormSet(BaseInlineFormSet):
def get_form_kwargs(self, index):

kwargs = super().get_form_kwargs(index)
kwargs['parent_object'] = self.instance
return kwargs

class MyForm(forms.ModelForm):
def init (self, *args, parent_object, **kwargs):

self.parent_object = parent_object
super(MyForm, self). init (*args, **kwargs)

class MyChildInline(admin.TabularInline):
formset = MyFormSet
form = MyForm



P a g e | 62

10.7.2 Odgovor_2:
Koristim Django 1.10 i ovo radi za mene:
Kreiraj formset i postavi parent objekat u kwargs:

class MyFormSet(BaseInlineFormSet):
def get_form_kwargs(self, index):

kwargs = super(MyFormSet, self).get_form_kwargs(index)
kwargs.update({'parent': self.instance})
return kwargs

Kreiraj objekat Form i pop atribut pre poziva super():
class MyForm(forms.ModelForm):

def init (self, *args, **kwargs):
parent = kwargs.pop('parent')
super(MyForm, self). init (*args, **kwargs)
# radi šta treba sa parent

Postavi u inline admin:

class MyModelInline(admin.StackedInline):
model = MyModel
fields = ('my_fields', )
form = MyFrom
formset = MyFormSet

10.7.3 Odgovor_3:
AdminModel ima metod get_formsets. On prima objekat i vraća formsetove. Mislim da možete dodati neki info
o parent objektu u formset klasu i kasnije koristiti formsetovu __init__ metodu.

10.7.4 Odgovor_4:
Ako je polje forme od intersa konstruisano iz polja modela, možete koristiti sledeći kod za primenu
prilagođenog ponašanja:

class MyChildInline(admin.TabularInline):
model = MyChildModel
extra = 1
def get_formset(self, request, parent=None, **kwargs):

def formfield_callback(db_field):
"""
Constructor of the formfield given the model field.
"""
formfield = self.formfield_for_dbfield(db_field, request=request)
if db_field.name == 'my_choice_field' and parent is not None:
formfield.choices = parent.get_choices()
return formfield
return super(MyChildInline, self).get_formset(request, obj=obj,

formfield_callback=formfield_callback, **kwargs)
return result



P a g e | 63

11 Filtriranje
U ovom vodiču naučićemo osnovno, prilagođeno i napredno filtriranje poput lančanih filtera pomoću Django
admina. Za ovaj vodič koristiće se modeli Shop i Product.

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

Zaista jednostavno, proizvod pripada radnji.

Kako dodati osnovno filtriranje u interfejs?
Recimo da bismo želeli da filtriramo našu prodavnicu u admin po korisniku. Da biste to postigli samo
dodajte list_filter za ShopAdmin klasu (u admin.py ) i odredite oblast na kojima može da filtrira.

list_filter = ('user', )

Pun kod:
class ShopAdmin(admin.ModelAdmin):

fieldsets = [
(None, {'fields': ['user','name','email','address']}),

]
list_display = ('user','name', 'email', )
list_filter = ('user',)
ordering = ('user',)

A sada će se u gornjem desnom uglu ekrana pojaviti opcija filtriranja. Još bolje, recimo da bismo želeli da
filtriramo samo za korisnike koji imaju prodavnicu. Ovaj put ćemo napisati:

list_filter = (('user', admin.RelatedOnlyFieldListFilter),)

Sada opcija filtriranja sadrži samo jednog korisnika, jer drugi korisnici nemaju prodavnice.

Kako dodati prilagođeni filter u django admin?



P a g e | 64
Sada zamislimo da želimo da dodamo filter zasnovan na poslovnim pravilima,a ne na određenom polju. Da
bismo to uradili, treba da

-Nasledimo klasu SimpleListFilter,
- zadamo ime filtera preko osobine name
-zadamo ime parametra filtera preko osobine parametar_name,
-nadjačamo metodu lookups,
-nadjačamo metodu queryset:

Lookups metod treba da vrati listu slogova.
-Prvi element sloga je kodirana vrednost koja će se pojaviti u url-u upita,
-Drugi element sloga je ime filtera koji će biti prikazan na admin interfejsu.

Queryset metod vraća filtrirani queryset. Pogledajmo:

class ProductPriceFilter(admin.SimpleListFilter):
title = 'price'
parameter_name = 'price'
def lookups(self, request, model_admin):

"""
Vraća listu slogova, prvi element svakog sloga je kodirana vrednostopcije koja
će se pojaviti u query URL-u, drugi element je ime opcije koja će se pojaviti
na filteru.
"""
return (("low_price","Filter by low price"), ("high_price","Filter by high

price"))
def queryset(self, request, queryset):

"""
Vraća filtrirani queryset baziran na vrednosti query stringa,
vraćenog sa `self.value()`.
"""
if self.value()=="low_price":

return queryset.filter(price lte=5)
elif self.value() == "high_price":

return queryset.filter(price gt=5)
else:

return queryset.all()

U ovom primeru ćemo dodati mogućnost filtriranja proizvoda po niskoj ili visokoj ceni. Smatramo da je
niska cena cena ispod ili jednaka 5, dok će visoka cena biti iznad 5.

Da biste koristili filter treba samo da ukažete na ime filterske klase u list_filter opciji:

list_filter = (ProductPriceFilter,)

Kao što vidimo, u interfejsu imamo novi filter "By price“. Ako kliknemo na jednu od vrednosti, lista
prikazanih rezultata će se promeniti i ako obratimo pažnju na url, možemo videti da izabrana vrednost filtera
u url-u sadrži naš parametar dodan u metodu pretraživanja:



P a g e | 65
http://127.0.0.1:8000/admin/backoffice/product/?price=low_price

Kako ulančati filtere u Django adminu?
Ok, idemo sada malo dalje. Recimo da bismo želeli da primenimo povezane filtere (tj. ulančane filtere). Na
Product možemo dodati filtriranje korisnika

list_filter=('user', ProductPriceFilter, 'refShop',)

Sada, ako filtriramo prema korisniku, i dalje ćemo videti sve vrednosti Shop (u Shop filteru). Moglo bi biti
bolje da prikažemo samo prodavnice koje pripadaju korisniku kojeg smo upravo filtrirali. To je ono što
predstavlja

ulančani filter. Spisak vrednosti prikazanih u filtru zavisiće od vrednosti izabrane u drugom filtru.

Da bismo to uradili, ispitaćemo URL upita i utvrditi da li je korisnik korišćen zafiltriranje rezultata, ako je
tako, URL će sadržati

? User id exact = <value>

Na osnovu toga možemo stvoriti prilagođeni filter koji će odabrati vrednosti samo zaizabranu vrednost
korisničkog ID-a.

class UserShopFilter(admin.SimpleListFilter):
title = 'shop'
parameter_name = 'refShop'
def lookups(self, request, model_admin):

if 'user id exact' in request.GET:
id = request.GET['user id exact']
shops = set([c.refShop for c in

model_admin.model.objects.all().filter(user=id)])
else:

shops = set([c.refShop for c in
model_admin.model.objects.all()])

return [(s.id, s.name) for s in shops]
def queryset(self, request, queryset):

if self.value():
return queryset.filter(refShop id exact = self.value())

Dakle, u metodu lookups, proveravamo da li imamo izabranog korisnika i da li ćemo odabrati samo
prodavnice koje pripadaju tom korisniku

shops = set([c.refShop for c in model_admin.model.objects.all().filter(user=id)])

U suprotnom možemo odabrati sve prodavnice

shops = set([c.refShop for c in model_admin.model.objects.all()])

Tada možemo vratiti rezultate koji će biti prikazani u interfejsu

return [(s.id, s.name) for s in shops]



P a g e | 66

Sada ako na administratorskom interfejsu izaberemo korisnika B, moći ćemo da vidimo samo prodavnice
koje pripadaju našem korisniku B. To je zato što smo primenili django filter koji zavisi od drugog filtera.

Kako dodati podrazumevani filter u Django adminu?
Recimo da za admin model naše pasmine želimo da korisnik može istovremeno da vidi sve pasmine iste
vrste, dakle pse u jednom trenutku, mačke ili bilo koju drugu, ali ne sve njih zajedno.

Da bismo to uradili, moramo da zamenimo dodatni metod klase SimpleListFilter, value metodu. Ovaj metod
je odgovoran da filtru kaže da li postoji izabrana stavka za filtriranje ili ne.

Da bismo to postigli, dodali smo value() metodu u naš filter:

from django.contrib import admin
from adminfilters.models import Species, Breed

class SpeciesListFilter(admin.SimpleListFilter):"""
Ovaj filter uvek vraća subset instanci modela, bilo filtriranjem pokorisničkom
izboru ili po default vrednosti.
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



P a g e | 67
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

Samo zamenjujući metodu value, postižemo da ako nije izabran nijedan filter, dajemo filteru zadanu
vrednost. Ostatak filtera već će vratiti filtrirani upit postavljen na ModelAdmin.

Napomena: Čak i pritiskom na filter „Sve“ vratiće se lista filtrirana prema podrazumevanoj vrednosti.

Podrazumevani filteri - II način
Postoji mnogo pristupa za primenu podrazumevanih filtera. Neki pristupi uključuju prilagođene filtere ili

ubrizgavanje posebnih parametara upita u zahtev. Želeli smo dato izbegnemo. Otkrili smo da je sledeći
pristup jednostavan i jasan:

from urllib.parse import urlencode from django.shortcuts import redirect
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

Ako je prikazu liste pristupljeno iz drugog prikaza, a nisu navedeni parametri upita, generišemo
podrazumevani upit i preusmeravamo.

Primenimo podrazumevani filter na našoj stranici proizvoda da bismo prikazali samo proizvode stvorene u
poslednjih mesec dana:

from django.utils import timezone



P a g e | 68

@admin.register(models.Product)
class ProductAdmin(admin.ModelAdmin, DefaultFilterMixin):

date_hierarchy = 'created'
def get_default_filters(self, request):

now = timezone.now()
return {'created year': now.year, 'created month': now.month,}

Ako idemo na stranicu detalja ili ako dođemo do stranice sa parametrima upita,podrazumevani filter se neće
primeniti.

Prilagodjeni tekst filteri
Ovde je izbor ograničen. Ali u nekim slučajevima, gde je jako puno filterskih mogućnosti, bolje je prikazati
tekst ulaz umesto liste izbora. Hajde da napišemo prilagodjeni filter za filtriranje knjiga po godini
publikovanja.

Napišimo ulazni filter.

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
#admin_input_filter.html
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



P a g e | 69
</ul>

Prilagodjena lista filtera nad datumskim poljem
Možemo pisati prilagodjene filtere tako da možemo postaviti izračunata polja i
dodati filtere nad njima.

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

published_date year lte=end)

Napredni filteri
Sve gornje metode biće korisne samo kao odredjeno proširenje. Preko toga, postoje paketi kao django-
advancedfilters koji imaju napredne mogućnosti filtriranja.

Za postavku paketa
- Instaliraj paket sa: pip install django-advanced-filters
-Dodaj advanced_filters u INSTALLED_APPS
-Dodaj u projektni urlconf.py: url(r’^advanced_filters/’,

include(‘advanced_filters.urls’))
-Pokreni python manage.py migrate.

Kada je setup završen možemo pisati:

from advanced_filters.admin import AdminAdvancedFiltersMixin
class BookAdAdminFilter(AdminAdvancedFiltersMixin, admin.ModelAdmin):

list_display = ('id', 'name', 'author', 'published_date', 'is_available')
advanced_filter_fields = ('name', 'published_date', 'author', 'is_available')

Jednostavno se može kreirati filter za sve knjige publikovane izmedju 1980 i 1990, koje imaju ocenu višu od
3.75 i broj strana veći od 100. Ovakav filter se može imenovati i sačuvati za kasnije korišćenje.

Za više detalja pogledati na: https://github.com/modlinltd/django-advanced-filters



P a g e | 70

12 Validacija Django prilagođene forme
Dobra je praksa ne prihvatati podatke dostavljene od korisnika takvi kakvi jesu. Podaci forme moraju biti
potvrđeni kako bi se osiguralo da su podaci validni i da ispunjavaju sve neophodne uslove. U Django-u se
ova provera vrši na dva nivoa - nivou polja i nivou forme, a oba omogućavaju dodavanje prilagođene
provere.

Django-ova ugrađena validacija forme
Django nam pomaže tako što automatski potvrđuje polja forme. To znači da možemo biti sigurni da su
dostavljeni podaci tipa koji očekuje polje forme a ako nisu doći će do greške.

Ako, na primer, očekujemo da korisnik pošalje datum, a umesto toga podnese ’hobotnicu’, pojaviće se
greška koja govori korisniku da ’hobotnica’ nije važeći datum. Django takođe može da obezbedi automatsku
proveru valjanosti u zavisnosti od polja forme. U Django dokumentima možete pronaći koje su validacije
dostupne za svaki tip polja forme.

Na nivou forme, Django može da vrši automatsku proveru jedinstvenosti između više polja. Ove provere
zavise od načina na koji smo podesili naše modele i pomažu u osiguravanju da korisnici ne šalju duplikate
podataka ako mi to ne želimo.

Ali šta ako želite da budete sigurni da ono što korisnik podnese ispunjava neki drugi uslov koji nije
automatski potvrđen? Na primer, ako smo očekivali da će korisnik predati palindrom (reč ili faza istu napred
i nazad), kako bismo mogli da proverimo dali je to tačno? Ili, ako bismo samo želeli da korisnici predaju
imena ljudi koji imaju imena i prezimena koja počinju istim slovom, kako bismo to mogli proveriti?
Istražimo oba ova scenarija.

12.1.1 Validacija pojedinačnog polja
Možemo da proverimo da li je reč koju je poslao korisnik palindrom tako što ćemo izvršiti dodatnu proveru
na nivou polja. Prvo, pogledajmo naš model palindroma koji ima samo jedno polje za našu reč.
#models.py
from django.db import models
class Palindrome(models.Model):

word = models.CharField(max_length=250, blank=False)
def _ str_ (self):

return self.word

Dalje, napravićemo formu da bi korisnik mogao da preda palindrom.

Nasleđujemo od ModelForm jer želimo da sačuvamo korisničke podatke na modelu Palindrome. U ovom
trenutku, forma ima samo Django-ovu automatsku proveru forme. Ako bismo formu povezali sa prikazom,
šta bi se dogodilo kada korisnik pošalje reč u formu koja nije palindrom?

Čuva se u bazi podataka. To nije ono što želimo, ali još uvek nismo učinili ništa da to sprečimo! Da bismo
proverili da li je poslata reč palindrom, moramo da modifikujemo našu PalindromeForm tako da uključuje
test koji proverava da li je reč ista napred i nazad.

#forms.py
from django.forms import ModelForm, ValidationError # Import ValidationError



P a g e | 71
from .models import Palindrome
class PalindromeForm(ModelForm):

def clean_word(self):
# Uzmi korisnički unešenu reč iz rečnika cleaned_data
data = self.cleaned_data["word"]
# Proveri da li je reč palindrom
if data != "".join(reversed(data)):

# Ako nije podigni grešku
raise ValidationError("The word is not a palindrome")

# Vrati podatak, uvek, čak iako nije menjan
return data

class Meta:
model = Palindrome
fields = ["word"]

Samo treba da izvršimo dodatnu proveru na nivou polja da bismo proverili da li je reč palindrom. Da bismo
dodali ovu dodatnu validaciju polja, definišemo metodu clean_word koja deluje samo na polju word. Ova
metoda će se pozvati nakon Djangove automatske provere valjanosti koja za nas stvara rečnik clean_data.
Word dobijemo iz clean_data i čuvamo je kao data.

Sada konačno moramo da proverimo da li je reč koju je korisnik poslao palindrom upoređujući je sa
obrnutom verzijom samog sebe. Ako je reč palindrom, provera prolazi i metoda vraća izvorni podatak. Ako
reč nije palindrom, podižemo ValidationError sa prilagođenom porukom o grešci. Ova poruka će se dodati
u formu i prikazati korisniku kada se ponovo prikaže sa greškama zbog neuspešnog potvrđivanja forme.

Ako je naša forma imala više polja koja smo želeli da potvrdimo, samo treba da dodamo metodu
clean_<fieldname> za svako polje za koje želimo da dodamo prilagođenu proveru.

Mogli smo da uključimo i više provera validacije u našu metodu clean_word ako smo, na primer, takođe
želeli da se uverimo da je svaki podneti palindrom duži od četiri slova.

12.1.2 Validacija više polja
Pogledajmo sada primer forme koji ima polja za first_name i last_name i želimo da oba počinju istim
slovom. U ovom slučaju, pošto moramo da izvršimo proveru validacije koja uključuje više od jednog polja,
naša validacija se vrši na nivou forme.

Naš model će imati dva CharField polja first_name i last_name.
#models.py
from django.db import models
class Person(models.Model):

first_name = models.CharField(max_length=250, blank=False)
last_name = models.CharField(max_length=250, blank=False)

Kao i prethodni primer, započećemo sa osnovnom formom koji nema prilagođenu proveru valjanosti.

#forms.py
from django.forms import ModelFormfrom .models import Person



P a g e | 72
class PersonForm(ModelForm):

class Meta:
model = Person
fields = ["first_name", "last_name"]

Kao i ranije, naša forma će omogućiti da se bilo koji niz znakova pošalje za first_name i last_name i kreiraće
i sačuvati instancu modela sa tim vrednostima bez grešaka.

Da bismo dodali proveru valjanosti, zamenimo metodu clean u formi.

#forms.py
from django.forms import ModelForm, ValidationError # Import ValidationError
from .models import Person
class PersonForm(ModelForm):

def clean(self):
# Uzmi korisnički unešena imena iz cleaned_data rečnika
cleaned_data = super().clean()
first_name = cleaned_data.get("first_name")
last_name = cleaned_data.get("last_name")
# Proveri da li je prvo slovo imena i prezimena isto
if first_name[0].lower() != last_name[0].lower()

# Ako nije podigni grešku
raise ValidationError("The first letters of the names do not match")

return cleaned_data
class Meta:

model = Person
fields = ["first_name", "last_name"]

Ovo se razlikuje od prethodnog primera gde smo kreirali metodu clean_<ime polja> jer sada gledamo više
polja umesto jednog polja. Kada zamenjujemo clean na ModelForm, prvo pozivamo metodu clean
roditeljske klase da bismo mogli imati pristup clean_data.

Ostali tipovi formi možda neće vratiti cleaned_data iz metode clean na roditeljskoj klasi. U tom slučaju
nazivamo super().clean() samostalno bez dodeljivanja i pristupimo očišćenim podacima putem
self.cleaned_data.

Jednom kada smo dobili clean_data možemo dobiti vrednosti first_name i last_name. Tada možemo izvršiti
stvarnu proveru valjanosti da li ime i prezime počinju istim slovom. Ako se ne podudaraju, pokrećemo
grešku validacije koja se dodaje u formu i prikazuje korisniku.

Zaključak
Validacija prilagođene forme vrši se na različite načine, u zavisnosti od toga da li je validacija potrebna na
nivou polja za jedno polje forme ili na nivou forme za više polja.

Da bi se dodala validacija za jedno polje, u formu se dodaje metoda clean_<ime_polja> za željeno ime
polja koje treba potvrditi. Ovom metodom treba dobiti podatke o polju izrečnika self.cleaned_data i vratiti
podatke o polju bez obzira da li su izmenjeni ili ne. Uslovni izrazi se dodaju metodi za izvođenje željene
provere valjanosti i podizanje ValidationErrors ako provera valjanosti ne uspe.



P a g e | 73
Za validaciju odnosa između više polja poziva se metoda clean na formi. Podaci polja postaju dostupni
pozivom metode roditeljske klase clean putem super().clean() koji vraća očišćene podatke polja za
ModelForm. Sada se može pristupiti svim podacima polja iz forme i izvršiti željena provera valjanosti da bi
se po potrebi pojavila ValidationError.



P a g e | 74

13 Djangove optimizacija QuerySet upita
Kada db ima strane ključeve, korišćenjem select_related() i prefetch_related() možemo smanjiti
broj db zahteva i poboljšati performanse.

Opis primera:

# models.py:
from django.db import models
class Province(models.Model):

name = models.CharField(max_length=10)
def unicode (self):
return self.name

class City(models.Model):
name = models.CharField(max_length=5)
province = models.ForeignKey(Province)
def unicode (self):
return self.name

class Person(models.Model):
firstname = models.CharField(max_length=10)
lastname = models.CharField(max_length=10)
visitation = models.ManyToManyField(City, related_name = "visitor")
hometown = models.ForeignKey(City, related_name = "birth")
living = models.ForeignKey(City, related_name = "citizen")

Note1: Kreirana aplikacija se zove “QSOptimize”
Note2: Zbog jednostavnosti, imamo dva podatka u tabeli qsoptimize_province:

- Hubei Province i Guangdong Province,
i tri podatka u tabeli qsoptimize_city.:
-Wuhan City, Shiyan City i Guangzhou City.

select_related()
Za jedan-na-jedan veze i polja stranih ključeva, možete koristiti select_related za optimizaciju QuerySet-a.

Posle upotrebe select_related() funkcije na QuerySet-u, Django vraća objekat koji odgovara stranom
ključu, zbog toga eliminiše potrebu za kasnijim upitima na db. Ako treba da štampamo sve gradove i
njihove provincije, na direktan način:

>> cities = City.objects.all()
>>> for c in cities:
... print c.province
Ovo vodi do linearnih SQL upita. Ako postoji n objekata sa k stranih ključeva za svaki objekat, to će
rezultovati sa n*k+1 SQL upita. U ovom slučaju postoji tri grada, i biće četiri upita:

SELECT
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`



P a g e | 75
FROM

`QSOptimize_city`
SELECT

`QSOptimize_province`.`id`,
`QSOptimize_province`.`name`

FROM
`QSOptimize_province`

WHERE
`QSOptimize_province`.`id` = 1 ;

SELECT
`QSOptimize_province`.`id`,
`QSOptimize_province`.`name`

FROM
`QSOptimize_province`

WHERE
`QSOptimize_province`.`id` = 2 ;

SELECT
`QSOptimize_province`.`id`,
`QSOptimize_province`.`name`

FROM
`QSOptimize_province`

WHERE
`QSOptimize_province`.`id` = 1 ;

Napomena: SQL izrazi su uzeti direktno sa Django logger-a. Ako koristimo select_related() funkciju:

>>> citys = City.objects.select_related().all()
>>> for c in citys:
... print c.province

Sada je samo jedan SQL upit:

SELECT
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`,
`QSOptimize_province`.`id`,
`QSOptimize_province`.`name`

FROM
`QSOptimize_city`

INNER JOIN
`QSOptimize_province` ON (`QSOptimize_city`.`province_id` =
QSOptimize_province`.`id`);

Django ovde koristi INNER JOIN da dobije informacije o provincijama. Rezultati upita su:

Id city_name province_id id province_name
1 Wuhan City 1 1 Hubei Province
2 Guangzhou City 2 2 Guangdong Province
3 Shiyan City 1 1 Hubei Province

Ovaj metod podržava sledeće srgumente:



P a g e | 76

13.1.1 *fields parametri
select_related() prihvata varijabilni broj parametara, svaki od njih je ime polja stranog ključa, (tj. sadržaj
glavne tabele) koji treba pokupiti, kao i ime stranog ključa itd.

Da bi izabrali polje iz parent tabele treba koristiti dve podcrte ’’__’’.

Na primer, da bi dobili Zhang San-ovu provinciju:

>>> zhangs = Person.objects
.select_related('living province')
.get(firstname = u "Zhang", lastname = u "San")

>>> zhangs.living.province

Odgovarajući SQL upit je:

SELECT
`QSOptimize_person`.`id`,
`QSOptimize_person`.`firstname`,
`QSOptimize_person`.`lastname`,
`QSOptimize_person`.`hometown_id`,
`QSOptimize_person`.`living_id`,
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`,
`QSOptimize_province`.`id`,
`QSOptimize_province`.`name`

FROM `QSOptimize_person`
INNER JOIN

`QSOptimize_city` ON (`QSOptimize_person`.`living_id` = `QSOptimize_city`.`id`)
INNER JOIN

`QSOptimize_province` ON(`QSOptimize_city`.`province_id`=
`QSOptimize_province`.`id`)
WHERE (

`QSOptimize_person`.`lastname` = 'San' AND `QSOptimize_person`.`first name` =
'Zhang'
);

Django koristi dva INNER JOIN-a za kompletiranje upita, dobija sadržaj tabela city i province i
dodaje ih na odgovarajuće vrste rezultujuće tabele, tako da ne treba više upita.

Id First name Last name Home Living City_Id City_Name Province_i Id Province_name
Town Town d

1 Zhang San 3 1 1 Wuhan 1 1 Hubei
Nespecificirani strani ključevi nisu dodati u rezultat. Treba dobiti Zhang San’s hometown, na
sledeći način:

>>>
zhangs.hometown.province
SELECT

`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`



P a g e | 77
FROM

`QSOptimize_city`
WHERE

`QSOptimize_city`.`id` = 3 ;
SELECT

`QSOptimize_province`.`id`,
`QSOptimize_province`.`name`

FROM
`QSOptimize_province`

WHERE
`QSOptimize_province`.`id` = 1

Pošto strani ključ nije specificiran, napravljena su dva upita. Ako je veća dubina,veći je i broj upita.
Ako želite i hometown i province gde živi u pre v1.7:

zhangs = Person.objects.select_related('town province', 'living province').
get(first name = u "Zhang", LastName = u "San")

>>> zhangs.hometown.province
>>> zhangs.living.province

Od ver. 1.7 možete ulančati operacije kao i druge queryset funkcije:

zangs = Person.objects.select_related('town province')
.select_related('living province')
.get(first name = u "Zhang", LastName = u "San")

>>> zhangs.hometown.province
>>> zhangs.living.province

Za verzije do 1.7 dobićete rezultate samo posledje operacije, u ovom slučaju nema rezultata za hometown.
Dva SQL upita su generisana kada štampate vašu provinciju.

13.1.2 depth parameter
select_related() prihvata depth parametar, koji definiše dubinu select_related. Django rekurzivno prolazi sve
OneToOneField i ForeignKey. Za ilustraciju:

>>> zhangs = Person.objects.select_related(depth = d)

d = 1 je ekvivalentno sa:

select_related(‘hometown’, ’living’)

d = 2 je ekvivalentno sa:

select_related(`citizen province’, `living province’)

13.1.3 *parameters
select_related() može biti parametrizovan, što znači da će Django ići sa select_related najdublje što može.

Na primer:

zangs = Person.objects.select_related().get(first name=u”Zhang“, LastName=u”San”)



P a g e | 78

Dve stvari treba primetiti:
- Django može u kompleksnim situacijama probiti granice rekurzije.
- Django ne zna koja polja koristitite, zbog toga uzima sve, što je gubitak performansi.

13.1.4 Zaključak
- select_related optimizuje jedan-na-jedan i više-na-jedan veze.
- select_related koristi JOIN naredbe da optimizuje i poboljša performanse smanjenjem broja upita.
- Možete spec. ime polja. Spec. rekurzivni upit može biti implementiran sa duplim podcrtama

povezanim na ime polja. Ovde nema keširanja.
- Može se spec. rekurzivna dubina, i Django automatski kešira sva polja unutar spec puta. Ako želite da

pristupite poljima van specificiranog puta, Django će raditi SQL upit ponovo.
- Podržani su pozvi bez parametara, i Django će uraditi rekurzivne upite što je dublje moguće. Ali

postoji granica u rekurzijama i gubitak na performansama.
- Django >= 1.7, select_related ulančava pozive što je ekvivalentno korišćenju varijabilne dužine

parametara, Django < 1.7. ulančavanje poziva ruši prednje select_related pozive i ostavlja samo
poslednje.

prefetch_related()
Za više-na-više i jedan-na-više vezu, prefetch_related() može biti korišćen za optimizaciju. Ali ovde
nema jedan-na-više veza, reći ćete vi. U stvari, ForeignKey je više-na-jedan veza, a sa druge strane
gledano to je jedan-na-više veza.

prefetch_related() i select_related() su dizajnirani da smanje broj SQL upita, ali su implementirane na različit
način. select_related() rešava problem sa JOIN naredbama.

Ali, za više-na-više veze, ako bi koristili JOIN vezu, dobijene tabele bile bi ogromne, n x m veličine.
Rešenje je da prefetch_related() odvojeno pravi upite i da zatim Python procesira njihove veze. Da bi
dobili gradove koje je Zhang San posetio, prefetch_related() bi uradio sledeće:

zhangs = Person.objects.prefetch_related('visitation')
.get(first name=u"Zhang", LastName=u"three")

>>> for city in zhangs.visitation.all():
... print city
...

SQL upiti su kao što sledi:

SELECT
`QSOptimize_person`.`id`,
`QSOptimize_person`.`firstname`,
`QSOptimize_person`.`lastname`,
`QSOptimize_person`.`hometown_id`,
`QSOptimize_person`.`living_id`

FROM
`QSOptimize_person`

WHERE
`QSOptimize_person'.`lastname`='three' AND
`QSOptimize_person'. `first name`='Zhang');

SELECT



P a g e | 79
`QSOptimize_person_visitation`.`person_id` AS `_prefetch_related_val`,
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`

FROM
`QSOptimize_city`

INNER JOIN
`QSOptimize_person_visitation` ON (

`QSOptimize_city`.`id` = `QSOptimize_person_visitation`.`city_id`
)

WHERE
`QSOptimize_person_visitation`.`person_id` IN (1);

Prvi SQL upit daje samo Zhang San Person objekat. Drugi upit je kritičan.

Id First Last Hometown_id Living
name name id

1 Zhang San 3 1

Prefetch_related Id Name Province_id
value
1 1 Wuhan City 1
1 2 Guangzhou City 2
1 3 Shiyan City 1

Ili ako želimo da dobijemo sve gradove provincije Hubei:

>>> hb = Province.objects.prefetch_related('city_set')
.get(name iexact = u "Hubei Province")

>>> for city in hb.city_set.all():
... city.name

Ovo će dovesti do sledećih SQL upita:

SELECT
`QSOptimize_province`.`id`,
`QSOptimize_province`.`name`

FROM
`QSOptimize_province`

WHERE
`QSOptimize_province`.`name` LIKE 'Hubei Province';

SELECT
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`

FROM
`QSOptimize_city`

WHERE
`QSOptimize_city`.`province_id` IN (1);

A rezultujuće tabele :

Id Name



P a g e | 80
1 Hubei

Id Name Province_id
1 Wuhan City 1
3 Shiyan City 1

Prefech je implementiran korišćenjem IN naredbe. Kada postoji više objekata u QuerySet-u, problem
performansi, u zavisnosti od vrste db se može pojaviti.

13.2.1 *lookups parameter
Prefetch_related() je korišćen u Django < 1.7. Kao select_related(), prefetch_related() takodje podržava
duboke upite, kao napr. provincije koje su posetili svi Zhang ljudi:

zhangs = Person.objects.prefetch_related('visitation province')
.filter(first name iexact=u'Zhang')

>>> for i in zhangs:
for city in i.visitation.all():

print city.province

Generisani SQL upiti su:

SELECT
`QSOptimize_person`.`id`,
`QSOptimize_person`.`firstname`,
`QSOptimize_person`.`lastname`,
`QSOptimize_person`.`hometown_id`,
`QSOptimize_person`.`living_i

d` FROM
`QSOptimize_person`

WHERE
`QSOptimize_person`.`first name` LIKE 'Zhang';

SELECT
`QSOptimize_person_visitation`.`person_id` AS `_prefetch_related_val`,
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_i

d` FROM
`QSOptimize_city`

INNER JOIN
`QSOptimize_person_visitation` ON (`QSOptimize_city`.`id` =

`QSOptimize_person_visitation`.`city_id`)
WHERE

`QSOptimize_person_visitation`.`person_id` IN (1, 4);
SELECT

`QSOptimize_province`.`id`,
`QSOptimize_province`.`nam

e` FROM
`QSOptimize_province`

WHERE
`QSOptimize_province`.`id` IN (1, 2);



P a g e | 81

A rezultujuće tabele su:

id first_name last_name hometown_id living_id
1 Zhang San 3 1
4 Zhang San 2 2

_prefetch_related_val Id Name province_id
1 1 Wuhan City 1
1 2 Guangzhou City 2
4 2 Guangzhou City 2
1 3 Skiyan City 1

Id Name
1 Hubei Province
2 Guangdong Province

Ulančavanje prefetch_related dodaje ove upite.

Primetite da kada koristimo QuerySet, pošto je db zahtev promenjen u ulančanoj operaciji, keširanje će biti
ignorisano. To dovodi do db re-request za dobijanje podataka, što može izazvati probleme sa
performansama.

Promena zahteva prema db može značiti potrebu za različitim filters(), exclude(), itd, tako da to veoma
menja SQL kod. all() ne menja krajnji db zahtev, tako da on ne vodi ka ponovnom db zahtevu.

Na primer, da bi dobili gradove sa rečju “city” koje je svako posetio, to će voditi do brojnih SQL upita:
plist = Person.objects.prefetch_related('visitation')
[p.visitation.filter(name icontains=u "city") for p in plist]

Pošto su četiri osobe u db, rezultat je 2 + 4 SQL upita:

SELECT
`QSOptimize_person`.`id`,
`QSOptimize_person`.`firstname`,
`QSOptimize_person`.`lastname`,
`QSOptimize_person`.`hometown_id`,
`QSOptimize_person`.`living_id

` FROM
`QSOptimize_person`;

SELECT
`QSOptimize_person_visitation`.`person_id` AS `_prefetch_related_val`,
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`

FROM
`QSOptimize_city`

INNER JOIN
`QSOptimize_person_visitation` ON (`QSOptimize_city`.`id` =

`QSOptimize_person_visitation`.`city_id`)



P a g e | 82
WHERE

`QSOptimize_person_visitation`.`person_id` IN (1, 2, 3, 4);
SELECT

`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`

FROM
`QSOptimize_city`

INNER JOIN
`QSOptimize_person_visitation` ON (`QSOptimize_city`.`id` =

`QSOptimize_person_visitation`.`city_id`)
WHERE

`QSOptimize_person_visitation'.`person_id'= 1 AND
`QSOptimize_city'.`name `LIKE'% city%';

SELECT
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`

FROM
`QSOptimize_city`

INNER JOIN
`QSOptimize_person_visitation` ON (`QSOptimize_city`.`id` =

`QSOptimize_person_visitation`.`city_id`)
WHERE

`QSOptimize_person_visitation'.`person_id'= 2 AND
`QSOptimize_city'. `name `LIKE'% market%';

SELECT
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`

FROM
`QSOptimize_city`

INNER JOIN
`QSOptimize_person_visitation` ON
(`QSOptimize_city`.`id` = `QSOptimize_person_visitation`.`city_id`)

WHERE
`QSOptimize_person_visitation'. `person_id'= 3 AND
`QSOptimize_city'. `name `LIKE'% city%';

SELECT
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`

FROM
`QSOptimize_city`

INNER JOIN
`QSOptimize_person_visitation` ON (`QSOptimize_city`.`id` =

`QSOptimize_person_visitation`.`city_id`)
WHERE

`QSOptimize_person_visitation'. `person_id'= 4 AND
`QSOptimize_city'. `name `LIKE'% city%';

QuerySet izračunavanje je lenjo, i rezultati nisu dostupni dok se ne koriste. Kada se pokrene druga linija
Python koda, for petlja tretira plist kao iterator, koji okida db upite. Prva dva SQL upita izazvana su sa



P a g e | 83
prefetch_related.

Iako su sve zahtevane city informacije bile uključene u rezultate upita, jer je Person.visitation fitriran u petlji,
ovo obično menja db zahtev. Zbog toga, ove operacije će zanemariti keširane podatke i ponovo uraditi SQL
upit.

Ali šta ako je takav zahtev? U Django >= 1.7, možete to uraditi kroz Prefetch objekat. Ako je Django < 1.7,
možete uraditi ovako:

plist = Person.objects.prefetch_related('visitation')
[[city for city in p.visitation.all() if u"city" in city.name] for p in plist]

13.2.2 Prefetch objekat
U Django >= 1.7, možete koristiti Prefetch objekat da kontrolišete ponašanje prefetch_related funkcije.

Karakteristike Prefetch objekta:
- Prefetch objekat može specifikovati samo jednu prefetch operaciju.
- Prefetch objekti specifikuju polja na isti način kao parametri u prefetch_related, lookup polja se prave

duplom podcrtom.
- Možete manuelno spec. QuerySet korišćen od prefetch kroz queryset parameter.
- Ime prefetch atributa može biti spec. sa to_attr parametrom.
- Prefetch objekti i lookup parametri spec. u formi stringa mogu se miksovati.

Nastavimo sa primerima da dobijemo gradove sa “Wu” i “Zhou” u svim gradovima koje je svako posetio:

Wus = City.objects.filter(name icontains = u"wu")
Zhous = City.objects.filter(name icontains = u"state")
plist = Person.objects.prefetch_related(

Prefetch('visitation', queryset = wus, to_attr = "wu_city"),
Prefetch('visitation', queryset = zhous, to_attr = "zhou_city"),

)
[p.wu_city for p in plist]
[p.zhou_city for p in plist]

13.2.2.1 None
Možete isprazniti prefetch_related prosledjenjem None:

>>> prefetch_cleared_qset = qset.prefetch_related(None)

13.2.3 Zaključak
- Prefetch_related optimizuje jedan-na-više i više-na-više veze.
- Prefetch_related optimizuje veze izmedju tabela vraćanjem njihovog sadržaja odvojeno i korišćenjem
Python-a za procesiranje njihovih veza.

- Možete spec. ime polja koje zahteva prefetch_related korišćenjem varijabilne dužine parametara, na isti
način kao kod select_related.

- U Django >= 1.7, kompleksni upiti mogu biti implementirani kroz Prefetch objekte, ali izgleda da u
nižim verzijama Django-a to isto može biti implementirano.

- Prefetch objekti i stringovi mogu se mešati kao parametri prefetch_related.



P a g e | 84
- Ulanačni pozivi prefetch_related dodaju prefetch umesto da ga menjaju.
- Prefetch_related može biti očišćen prosledjenjem None.

prefetch_related() vs selected related()
Ako želimo da dobijemo ko je sve rodjen u Hubei province, najlakše je da dobijemo Hubei province prvo,
potom sve gradove u Hubei, i na kraju njihove žitelje:

>>> hb = Province.objects.get(name iexact = u"Hubei Province")
>>> people = []
>>> for city in hb.city_set.all():
... people.extend(city.birth.all())

Ovo će dovest do 1 + (broj gradova u Hubei) SQL upita. Pokušajmo sa prefetch_related():

>>> hb = Province.objects.prefetch_related("city_set birth")
.objects.get(name iexact = u"Hubei Province")

>>> people = []
>>> for city in hb.city_set.all():
... people.extend(city.birth.all())

Ovo je prefetch dubine 2, biće tri SQL upita:

SELECT
`QSOptimize_province`.`id`,
`QSOptimize_province`.`name

` FROM
`QSOptimize_province`

WHERE
`QSOptimize_province`.`name` LIKE 'Hubei Province';

SELECT
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id

` FROM
`QSOptimize_city`

WHERE
`QSOptimize_city`.`province_id` IN (1);

SELECT
`QSOptimize_person`.`id`,
`QSOptimize_person`.`firstname`, `QSOptimize_person`.`lastname`,
`QSOptimize_person`.`hometown_id`,
`QSOptimize_person`.`living_id

` FROM
`QSOptimize_person`

WHERE
`QSOptimize_person`.`hometown_id` IN (1, 3);

Možda je reverzni upit jednostavniji?
People = list(Person.objects.select_related("town province")

.filter(town province name iexact = u"hubei province")
SELECT

`QSOptimize_person`.`id`,



P a g e | 85
`QSOptimize_person`.`firstname`,
`QSOptimize_person`.`lastname`,
`QSOptimize_person`.`hometown_id`,
`QSOptimize_person`.`living_id`,
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id`,
`QSOptimize_province`.`id`,

`QSOptimize_province`.`name`
FROM
`QSOptimize_person`

INNER JOIN
`QSOptimize_city` ON (`QSOptimize_person`.`hometown_id` =

`QSOptimize_city`.`id`)
INNER JOIN

`QSOptimize_province` ON (`QSOptimize_city`.`province_id` =
`QSOptimize_province`.`id`)

WHERE
`QSOptimize_province`.`name` LIKE` 'Hubei Province';

id firstname lastname hometown_id living_id id name province_id id name
1 Zhang San 3 3 1 Shiyan 1 1 Hubei

City Province
2 Li 4 1 3 Wuhan 1 1 Hubei

City Province
3 Wang Mazi 3 2 3 Shiyan 1 1 Hubei

City Province

Select_related() je efikasniji od prefetch_related(). Zbog toga najbolje je koristi ga gde je moguće, kao što je
mnogo-na-jedan veza.

13.3.1 Zaključak
- Pošto select_related() uvek rešava problem u jednom SQL upitu i prefetch_related() pravi upite na svakoj
povezanoj tabeli, efikasnost select_related() je obično veća.

- Razmišljajte o prefetch_related() samo ako select_related() ne može da reši problem.
- Možete koristiti i select_related() i prefetch_related() u jednom QuerySet-u da smanjite broj SQL upita.
- Samo select_related() pre prefetch_related() je validno. Ako ide posle biće ignorisano.

Kombinovano korišćenje
Za isti QuerySet, možete koristiti obe funkcije u isto vreme. Dodajmo model Order:

class Order(models.Model):
customer = models.ForeignKey(Person)
orderinfo = models.CharField(max_length=50)
time = models.DateTimeField(auto_now_add = True)
def unicode (self):

return
self.orderinfo

Ako imamo order_id, treba da saznamo provinciju gde je customer ordera bio. Ako pokušamo sa
prefetch_related():



P a g e | 86
>>> plist = Order.objects

.prefetch_related('customer visitation province')

.get(id=1)
>>> for city in plist.customer.visitation.all():
... print city.province.name

Pošto su četiri tabele uključene: Order, Person, City, Province, prefetch_related() će generisati četiri SQL
upita:
SELECT

`QSOptimize_order`.`id`,
`QSOptimize_order`.`customer_id`,
`QSOptimize_order`.`orderinfo`,
`QSOptimize_order`.`time`

FROM
`QSOptimize_order`

WHERE
`QSOptimize_order`.`id` = 1;

SELECT
`QSOptimize_person`.`id`,
`QSOptimize_person`.`firstname`,
`QSOptimize_person`.`lastname`,
`QSOptimize_person`.`hometown_id`, `QSOptimize_person`.`living_id`

FROM
`QSOptimize_person`

WHERE
`QSOptimize_person`.`id` IN (1);

SELECT
`QSOptimize_person_visitation`.`person_id` AS `_prefetch_related_val`,
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id

` FROM
`QSOptimize_city`

INNER JOIN
`QSOptimize_person_visitation` ON (`QSOptimize_city`.`id` =

`QSOptimize_person_visitation`.`city_id`)
WHERE `QSOptimize_person_visitation`.`person_id` IN (1);
SELECT

`QSOptimize_province`.`id`,
`QSOptimize_province`.`name

` FROM
`QSOptimize_province`

WHERE
`QSOptimize_province`.`id` IN (1, 2);

I rezultujuće tabele će biti:

id customer_id orderinfo time
1 1 Info of order 2014-08-10 17:05:48

id firstname lastname hometown_id living_id
1 Zhang San 3 1



P a g e | 87
prefetch_related_val id name province_id
1 1 Wuhan City 1
1 2 Guangzhou City 2
1 3 Shiyan City 1

id name
1 Hubei Province
2 Gunagding Province

Bolji način je pozvati select_related() jednom, potom prefetch_related():

>>> plist = Order.objects
.select_related('customer')
.prefetch_related('customer visitation province')
.get(id=1)

>>> for city in plist.customer.visitation.all():
... print city.province.name

Rezultat je tri upita. Django će prvo select_related, a potom prefetch_related koristeći keširane podatke:

SELECT
`QSOptimize_order`.`id`,
`QSOptimize_order`.`customer_id`, `QSOptimize_order`.`orderinfo`,
`QSOptimize_order`.`time`,
`QSOptimize_person`.`id`,
`QSOptimize_person`.`firstname`,
`QSOptimize_person`.`lastname`,
`QSOptimize_person`.`hometown_id`,
`QSOptimize_person`.`living_id

` FROM
`QSOptimize_order`

INNER JOIN
`QSOptimize_person` ON (`QSOptimize_order`.`customer_id` =

`QSOptimize_person`.`id`)
WHERE

`QSOptimize_order`.`id` = 1 ;
SELECT

`QSOptimize_person_visitation`.`person_id` AS `_prefetch_related_val`,
`QSOptimize_city`.`id`,
`QSOptimize_city`.`name`,
`QSOptimize_city`.`province_id

` FROM
`QSOptimize_city`

INNER JOIN
`QSOptimize_person_visitation` ON (`QSOptimize_city`.`id` =

`QSOptimize_person_visitation`.`city_id`)
WHERE

`QSOptimize_person_visitation`.`person_id` IN (1);
SELECT

`QSOptimize_province`.`id`,
`QSOptimize_province`.`name

` FROM
`QSOptimize_province`



P a g e | 88
WHERE

`QSOptimize_province`.`id` IN (1, 2);

Rezultujuće tabele su:

id customer_id orderinfo time id firstname lastname hometown_id living_id
1 1 Info of 2014-08- 1 Zhang San 3 1

order 10
17:05:48 1

prefetch_related_val id name province_id
1 1 Wuhan City 1
1 2 Guangzhou City 2
1 3 Shuiyan City 1

id name
1 Hubei Province
2 Guangdong Province

Sve što treba da znate o prefetching-u u Django
Skoro sam radio prijavni sistem za jednu konferenciju. Bilo im je veoma važno da vide tabelu prijava
uključujući kolonu sa listom imena programa. Ugrubo modeli izgledaju ovako:
class Program(models.Model):

name = models.CharField(max_length=20,)
class Price(models.Model):

program = models.ForeignKey(Program,)
from_date = models.DateTimeField()
to_date = models.DateTimeField()

class Order(models.Model):
state = models.CharField(max_length=20,)
items = models.ManyToManyField(Price)

Ovde je:
- Program – jedna sesija, lektura ili konferencijski dan.
- Price – cena koja se može menjati u vremenu. Model reprezentuje cenu programa u odredjenom

vremenskom trenutku.
- Order – prijava na jedan ili više programa. Svaka stavka u prijavi ima cenu u trenutku kada
je prijava napravljena.

Kroz log mi ćemo pratiti upite izvršene od strane Djanga. Da bi ih logovali dodaj sledeće u settings.py:

LOGGING = {
...
'loggers': {

'django.db.backends': {'level': 'DEBUG',},
},

}



P a g e | 89
13.5.1 Šta je problem?
Pokušajmo da dobijemo ime programa iz jedne prijave:
>>> o = Order.objects.filter(state='completed').first()
(0.002)SELECT ...

FROM "orders_order"
WHERE "orders_order"."state" =
'completed'ORDER BY "orders_order"."id"
ASC LIMIT 1;

>>> [p.program.name for p in
o.items.all()](0.002)SELECT ...

FROM "events_price"
INNER JOIN "orders_order_items" ON

("events_price"."id" = "orders_order_items"."price_id")
WHERE "orders_order_items"."order_id" = 29; args=(29,)

(0.01) SELECT ...
(0.02) FROM "events_program"

WHERE "events_program"."id" = 8;
args=(8,)['Day 1 Pass']

Za dobijanje kompletirane prijave potreban je jedan upit. Za dobijanje imena programa za svaku prijavu
potrebana su dva upita. Ako trebamo dva upita za svaku prijavu, ukupan broj upita za 100 prijava je 1 + 100 *
2 = 201! Koristimo Django da smanjimo broj upita:

>>> o.items.values_list('program name')
(0.03) SELECT "events_program"."name"

FROM "events_price"
INNER JOIN "orders_order_items" ON ("events_price"."id" =

"orders_order_items"."price_id")
INNER JOIN "events_program" ON ("events_price"."program_id" =

"events_program"."id")
WHERE "orders_order_items"."order_id" = 29 LIMIT 21;

['Day 1 Pass']

Django je uradio povezivanje izmedju Price i Program i smanjo broj upita na jedan po prijavi. Znači umesto
201 upita sada imamo 101 upit.

13.5.1.1 Zašto ne možemo povezati tabele?
Ako imamo strani ključ možemo koristiti select_related ili ’snake case’ da bi dobili povezana polja u jednom
upitu. Na primer mi smo dobijali ime programa za listu cena u jednom upitu korišćenjem:

values_list('program name')

Ovo smo mogli da uradimo jer je svaka cena bila povezana sa tačno jednim programom. Ako je veza
izmedju dva modela više na više, ne možemo to uraditi. Svaka prijava ima jednu ili više povezanih cena,
ako ih povežemo kao dosad dobićemo grešku duplih vrednosti.
SELECT o.id AS order_id, p.id AS

price_idFROM orders_order o
JOIN orders_order_items op ON (o.id = op.order_id)
JOIN events_price p ON (op.price_id = p.id)

ORDER BY 1, 2;
order_id | price_id



P a g e | 90
+

45 | 38
45 | 56
70 | 38
70 | 50
70 | 77
71 | 38

Prijave 70 i 45 imaju više stavki tako da se pojavljuju više puta u rezultatu. Django ne može
upravljati sa ovim problemom!

13.5.2 prefetch_related
Django ima lep, ugradjen način rukovanja ovim problemom prefetch_related:
>>> o = Order.objects.filter(state='completed',)

.prefetch_related('items program',).first()
(0.002)SELECT ...

FROM "orders_order"
WHERE "orders_order"."state" =
'completed'ORDER BY "orders_order"."id"
ASC LIMIT 1;

(0.01) SELECT (
"orders_order_items"."order_id") AS "_prefetch_related_val_order_id",
"events_price"...

FROM "events_price"
INNER JOIN "orders_order_items"

ON ("events_price"."id" = "orders_order_items"."price_id")
WHERE "orders_order_items"."order_id" IN (29);

(0.01) SELECT
"events_program"."id",
"events_program"."name"

FROM "events_program"
WHERE "events_program"."id" IN (8);

Mi smo rekli Djangu da nam je namera da dobijemo items program iz rezultujućeg seta. U drugom i trećem
upitu možemo videti da Django dobija rezultat kroz tabelu orders_order_items i iz events_program. Rezultat
prefetching-a su keširani na objektima.

Šta se dešava kada mi pokušamo da dobijemo imena programa iz rezultata?

>>> [p.program.name for p in
o.items.all()]['Day 1 Pass']

Nema dodatnih upita – tačno ono što smo želeli! Ovde je važno da kada koristimo prefetch, radimo sa
objektima a ne sa queryset-om! Pokušaj da dobijemo imena programa sa upitom će proizvesti isti izlaz,
queryset, ali će napraviti i dodatni upit:

>>> o.items.values_list('program name')
(0.02)

SELECT "events_program"."name"
FROM "events_price"



P a g e | 91
INNER JOIN "orders_order_items" ON

("events_price"."id" = "orders_order_items"."price_id")
INNER JOIN "events_program" ON

("events_price"."program_id" = "events_program"."id")
WHERE "orders_order_items"."order_id" = 29 LIMIT 21;
['Day 1 Pass']

Na ovaj način imamo, za 100 prijava 3 upita.

13.5.3 Uvod u Prefetch objekat
Od ver. 1.7 uveden je novi Prefetch objekat koji proširuje mogućnosti prefetch_related. Novi objekat pruža
programeru mogućnost da nadjača upit koji bi koristio Django za prefetch povezanih objekata.

U našem prethodnom primeru Django koristi dva upita za prefetch – jedan kroz tabelu i jedan kroz program.
Šta ako kažemo Djangu da ih spoji?

>>> prices_and_programs = Price.objects.select_related('program')
>>> o = Order.objects.filter(state='completed')

.prefetch_related(Prefetch('items', queryset=prices_and_programs))

.first()
(0.01) SELECT ...

FROM "orders_order"
WHERE "orders_order"."state" =
'completed'ORDER BY "orders_order"."id"
ASC LIMIT 1;

(0.01) SELECT (
"orders_order_items"."order_id") AS "_prefetch_related_val_order_id",
"events_price"..., "events_program"...

INNER JOIN "events_program"
ON ("events_price"."program_id" = "events_program"."id")

WHERE "orders_order_items"."order_id" IN (29);

Kreirali smo upit koji povezuje cene sa programima. Potom smo rekli Djangu da koristitaj upit za prefetch
vrednosti. To je kao da smo rekli Djangu da nam je namera da dobijemo i items i programs za svaku
prijavu.

Sada možemo da vratimo imena programa:
>>> [p.program.name for p in o.items.all()]['Day 1 Pass']
Nema dodatnih upita ! Idemo na sledeći nivo!

Kada smo govorili o modelima, rekli smo da su prices modelovane kao SCD tabela.
To znači da možemo da pravimo upit samo na aktivne cene.

Jedna cena je aktivna na odredjenom datumu ako je izmedju from_date i end_date:
>>> from django.utils import timezone
>>> now = timezone.now()
>>> active_prices = Price.objects.filter(from_date lte=now, to_date gt=now,)



P a g e | 92
Korišćenjem Prefetch objekta mi kažemo Djangu da smesti prefetch objekte u novi atributrezultujućeg seta:

>>> from django.utils import timezone
>>> now = timezone.now()
>>> active_prices_and_programs = (Price.objects

.filter(from_date lte=now,to_date gt=now,)

.select_related('program'))
>>> o = Order.objects.filter(state='completed').prefetch_related(Prefetch('items',

queryset=active_prices_and_programs, to_attr='active_prices',),).first()
(0.01) SELECT ...

FROM "orders_order"
WHERE "orders_order"."state" =
'completed'ORDER BY "orders_order"."id"
ASC LIMIT 1;

SELECT ...
FROM "events_price"
INNER JOIN "orders_order_items"

ON ("events_price"."id" = "orders_order_items"."price_id")
INNER JOIN "events_program"

ON ("events_price"."program_id" = "events_program"."id")
WHERE ("orders_order_items"."order_id" IN (29) AND
"events_price"."from_date"<='2017–04–29T07:53:00.210537+00:00'::timestamptz AND
"events_price"."to_date">'2017–04–29T07:53:00.210537+00:00'::timestamptz);

Vidimo da Django izvršava dva upita, i prefetch upit sada sadrži prilagodjeni filter koji smo definisali. Da bi
dobili aktivne cene možemo koristiti novi atribut to_attr:
>>> [p.program.name for p in o.active_prices]['Day 1 Pass']

Nema dodatnih upita !

Efikasna paginacija u Djangu i Postgresu
Moglo bi se reći da većina veb okvira koristi naivan pristup paginaciji. Koristeći PostgreSQL COUNT,
LIMIT i OFFSET radi dobro za većinu veb aplikacija, ali ako imate tabele sa milionima zapisa ili više,
degradira performanse brzo.

Django je odličan okvir za izradu veb aplikacija, ali njegova podrazumevana metoda paginacije u velikoj meri
pada u ovu zamku. U ovom članku ću vam pomoći da razumete ograničenja Django-ove stranice i ponuditi
tri alternativne metode koje će poboljšati performanse vaše aplikacije. Usput ćete videti kompromise i
slučajeve upotrebe za svaku metodu tako da možete odlučiti koja najbolje odgovara vašoj aplikaciji.

13.6.1 Razumevanje naivne paginacije
Pogledajmo primer vrste kontrole paginacije koju veb aplikacija može da koristi:



P a g e | 93
U ovoj kontroli korisnik može da ode na prethodnu i sledeću stranicu ili da pređe direktno na određenu
stranicu. Upit da biste dobili desetu stranicu koristeći Postgres-ov LIMIT i OFFSET pristup mogao bi
izgledati ovako:

SELECT *
FROM users
ORDER BY created_at DESC
LIMIT 10 OFFSET 100

Uočite da za dobijanje ispravnog OFFSET broja stranice morate pomnožiti broj stranice x LIMIT.

Potreban je još jedan upit za prikaz naše kontrole paginacije. Morate znati broj zapisa u tabeli. Bez tih
informacija nećete znati koliko stranica morate da pretražite.

SELECT count(*) FROM users

Možda ćete videti kako ovaj pristup vrlo brzo može postati pravi problem performansi.

13.6.2 Predstava naivne paginacije
Da biste bolje razumeli uska grla performansi LIMIT i OFFSET, možete uvesti neke testne podatke i
isprobati ih.

Prvo, napravite tabelu dovoljno veliku da naiđe na usporavanja:
CREATE TABLE USERS (

id serial,
name varchar(50)

);
INSERT INTO users SELECT
--- Ten million records generate_series(1,10000000) AS id,
--- Example: "e6f2c6842d146c518185e1e47add9532"
substr(md5(random()::text), 0, 50) AS name;

Kada pokrenete upit da biste dobili desetu stranicu rezultata, odgovor je gotovo trenutan. Na svom Macbook
Pro računaru iz 2018. sa najnovijom verzijom Postgresa vidim podatke u 89 ms.

Međutim, za upite dalje, vreme čekanja se povećava. Uz LIMIT od 10 i OFFSET od 5.000.000, odgovor traje
2,62 sekunde.

Na kraju, možete pogledati COUNT upit koji se izvodi u svih 10 miliona redova. Taj upit sada traje
letargičnih 4,45 sekundi.

Spora performansa u ovim primerima je izazvana na način na koji OFFSET i COUNT rade. Da biste sa
OFFSET došli do navedene stranice, potrebno je da baza podataka pređe svaki indeks do stranice koju želite.
Zbog toga se performanse degradiraju što dalje zavirite u tabelu.

Naivni pristup paginaciji koja koristi COUNT, LIMIT i OFFSET je održivo rešenje za tabele ispod milion
redova. U velikim tabelama možete očekivati spore upite i loše korisničko iskustvo.



P a g e | 94
13.6.3 Rukovanje paginacijom u Djangu
Sada kada imate osnovno znanje o performansama upita za paginaciju u Postgresu, možete početi da shvatate
zašto paginacija usporava u Djangu. Da bih pokazao kako Django upravlja paginacijom, napravio sam novu
aplikaciju sa modelom korisnika i ubacio 10 miliona zapisa prilagođavajući naš raniji upit.

# {project}/users/models.py
from django.db import models
class User(models.Model):

name = models.CharField(max_length=50)

Koristio sam administratorsku stranicu da testiram brzinu paginacije.

# {project}/users/admin.py
from django.contrib import admin from .models import User
admin.site.register(User)

Sada kada je User model predstavljen na admin panelu, mogu videti tabelu sa 10 miliona zapisa.

Koristeći django-debug-toolbar mogu zaviriti u SQL upite koje Django generiše u realnom vremenu. Za
generisanje ovog korisničkog interfejsa koriste se dva upita:
-- Count the total number of records - 2.43 seconds
SELECT COUNT(*) AS " count" FROM "users_user"
-- Get first page of items - 2ms
SELECT "users_user"."id","users_user"."name" FROM "users_user"
ORDER BY "users_user"."id" DESC LIMIT 100

Ovi upiti bi trebali izgledati poznato jer su gotovo identični gornjim upitima naivne paginacije. Čudno, upit
za brojanje se pokreće dva puta, što znači da kada učitate Django admin panel, morate čekati da baza
podataka dva puta prebroji svaki red tabele.

Kada kliknete na stranicu 99,999, Django će ponovo pokrenuti dva upita za brojanje i još jedan upit za
paginaciju koristeći LIMIT i OFFSET:

-- Get the 99,999 page (100 results per page) - 13.34 seconds
SELECT "users_user"."id", "users_user"."name" FROM "users_user"
ORDER BY "users_user"."id" DESC
LIMIT 100 OFFSET 9999900

Ovom upitu je potrebno 13 sekundi da se završi!

Jasno je da je naivan pristup paginaciji u Djangu spor za velike tabele. Vremenom će vaše tablice baze
podataka verovatno rasti, a kako dostignu desetine miliona zapisa, vi i vaši klijenti ćete početi da primećujete
ovo užasno vreme učitavanja. Šta možete učiniti u vezi s tim?



P a g e | 95
U sledećim odeljcima pokazaću vam tri opcije za poboljšanje performansi paginacije u aplikaciji Django.

13.6.3.1 Opcija 1: Uklanjanje COUNT upita
COUNT upit dominira u vremenu učitavanja za prvu stranicu rezultata. Prilikom preskakanja na kasnije
stranice, ofset upit je najsporiji, ali fokusiraću se na poboljšanje COUNT u ovoj prvoj opciji.

Ovo može iznenaditi, ali jedno rešenje je potpuno uklanjanje upita za brojanje. Zar to neće slomiti korisnički
interfejs !?

Nekako ... U ovom slučaju, moglo bi biti razumno ne znati koliko stranica ima u tabeli Korisnici. Ne dešava
se često da se korisnici nađu na pet milionitoj stranici tabele zapisa. Navigacija do sledeće i prethodne
stranice obično je dovoljna kontrola. Korišćenje okvira za pretragu ili filtera je verovatno bolji metod za
pronalaženje zapisa koji želite.

Pogledajte Django-ova polja za pretraživanje za informacije o omogućavanju pretraživanja i filtriranja na
admin tabli i slobodno pročitajte pganalize članak o potpunom pretraživanju teksta u Djangu.

Najpoznatiji primer ove vrste paginacije je stranica sa rezultatima Google pretrage. Pri dnu stranice, skraćena
kontrola paginacije prikazuje direktne veze samo na prvih 10 stranica:

To ne znači da vas Google sprečava da vidite ostale milijarde rezultata. Jednostavno vam govori da bi
precizniji termin za pretragu bio bolji način da dođete do tih rezultata od paginacije.

Da se oslobodite COUNT smisla u svojoj aplikaciji, Django olakšava skrivanje. Prvo prepišite svojstvo count
podrazumevanog Paginator -a :

# {project}/users/paginator.py
from django.core.paginator import Paginator
from django.utils.functional import cached_property
class UserPaginator(Paginator):

@cached_property
def count(self):

return 9999999999

Primetite da je vrednost čuvara mesta broj koji je mnogo veći nego što očekujete da ćete imati rezultate.

Django reaguje na ovo prilagođavanje tako što prikazuje prvih nekoliko stranica u komponenti paginacije,
kao što biste očekivali. Međutim, poslednjih nekoliko stranica će biti lažni. Kada kliknete na stranicu koja ne
postoji, Django će vas odvesti na poslednju stranicu bez obzira na to koliko zapisa imate.

Nakon što zamenite podrazumevani paginator, uvezite ga u svoj administratorski model:

# {project}/users/admin.py
from django.contrib import admin from .models import User
from .paginator import UserPaginator



P a g e | 96
@admin.register(User)
class UserTableAdmin(admin.ModelAdmin):

show_full_result_count = False
paginator = UserPaginator

Imajte na umu da sam takođe postavio show_full_result_count na False. Ovo će isključiti drugi upit za
brojanje koji sam ranije primetio.

Nakon ažuriranja aplikacije ovim promenama, smanjio sam vreme za prvu stranicu sa ~ 5 sekundi na 8ms.
Imajte na umu da ova tabela i dalje pati od sporih OFFSET upita. Prelazak na stranicu 50,0000 trajao je 18
sekundi. Pre nego što vam pokažem kako da rešite OFFSET problem, pokazaću vam još jedan način za
poboljšanje COUNT upita.

13.6.3.2 Opcija 2: Približno COUNT
Drugi način da se smanji vreme provedeno na COUNT upitu korišćenjem nekih ugrađenih Postgres funkcija
za procenu ukupnog broja zapisa kada brojanje traje predugo. Možete videti temeljnu implementaciju
pristupa koja preopterećuje count metodu u Djangoovoj Paginator klasi.

Prva stvar da se uradi je da se postavi statement_timeout na upit i rezervnu opciju procenjenih zapisa. Možete
koristiti atomske transakcije da postavite vremensko ograničenje na 150 ms.

try:
with transaction.atomic(), connection.cursor() as cursor:

# Limit to 150 ms
cursor.execute('SET LOCAL statement_timeout TO 150;')
return super().count

except OperationalError:
pass

...

Ako count metoda vraća podatke pre isteka vremenskog ograničenja, onda se koristi stvarna vrednost.
Međutim, ako upit potraje duže, Django će se vratiti na približnu vrednost uskladištenu u pg_class
metapodacima. Metapodaci se ažuriraju kada se izvršavaju komande kao VACUUM, ANALYZEi CREATE
INDEX, ili se radi autovacuum radi na tabeli.

...
with transaction.atomic(), connection.cursor() as cursor:

# Obtain estimated values (only valid with PostgreSQL)
cursor.execute("SELECT reltuples FROM pg_class WHERE relname = %s",

[self.object_list.query.model._meta.db_table])
estimate = int(cursor.fetchone()[0])
return estimate

...

Nakon primene ove metode, maksimalno vreme koje ćete potrošiti na učitavanje COUNT je 150 ms.

13.6.3.3 Opcija 3: Paginacija sa setovanjem ključa

Da biste rešili spor OFFSET problem, možete ga zameniti paginacijom skupa ključeva (ili pretrage). U
paginaciji skupa ključeva, svaku stranicu preuzima poređano polje poput id ili created_at datuma. Umesto
ponavljanja brojanja stranica, kao što se OFFSET to dešava, paginacija ključeva filtrira direktno poredano



P a g e | 97
polje.

13.6.3.3.1 Primer korišćenja paginacije skupova ključeva

SELECT *
FROM user
WHERE id < 60 -- The last item in the previous page
ORDER BY id DESC
LIMIT 10

Ovaj pristup se malo razlikuje od OFFSET zbog toga što morate znati vrednost na kojoj počinjete. Zamislite
da ste na četvrtoj strani tabele, a znate prvu id na petoj. Umesto da prebrojavate sve zapise, možete ići
direktno na to id i vratiti sledećih deset stavki.

Ovo funkcioniše jer indeksi mogu efikasno podržati ovakav upit. Traženje indeksa B-stabla da vrati 10 id
unosa pre određenog id zahteva samo učitavanje 10 unosa indeksa. Uporedite to sa upotrebom OFFSET, gde
se moraju učitati svi unosi do pomaka, a zatim i navedena granica, što čini velike pomake veoma skupim.

Kada koristite paginaciju ključeva zajedno sa pravim indeksom, videćete značajno povećanje performansi.
Kompleksnost vremena da nađe neki zapis u bazi podataka je konstantna. Na primer, traženje poslednje
stranice velike tabele generisane gore traje samo 78 ms.

Ova metoda takođe štiti od oskudnih podataka. Ako je korisnik izbrisan i redosled više nije uzastopan u tabeli,
to ne utiče na paginaciju skupova ključeva; bez problema će preskočiti nedostajuću vrednost.

13.6.3.3.2 Kompromisi paginacije skupova ključeva

Paginacija ključeva dolazi sa nekoliko kompromisa. Bez pomaka, ne znate tačno koliko stranica ima u tabeli
niti na kom se broju stranice trenutno nalazite. Osim toga, za paginaciju skupova ključeva potrebno je
sortirati polje na vašem modelu. Sekvencijalni ID-ovi i polja datuma dobro funkcionišu.

13.6.4 Korišćenje dodatka dj-pagination za Django

Nažalost, nisam uspeo da pronađem biblioteku koja proširuje Djangov osnovni Paginator za dodavanje
paginacije skupova ključeva. Admin tabela zahteva paginator tog tipa, pa ću iste generisane podatke
demonstrirati u novom prikazu aplikacije pomoću dodatka dj-pagination.

Dodajte aplikaciju u svoj INSTALLED_APPS i middleware - dokumenti dobro objašnjavaju. Zatim kreirajte
prikaz u aplikaciji Korisnici:

# {project}/users/views.py
from django.shortcuts import render from .models import User
def index(request):

context = {
'users': User.objects.order_by('id').all()

}
return render(request, 'users/index.html', context)

Ovaj kod omogućava korisnicima da budu QuerySet dostupni u kontekstu prikaza i kaže mu da prikaže
datoteku predloška. Zatim dodajte direktorijum predloška u svoj TEMPLATES objekat podešavanja i dodajte



P a g e | 98
novu datoteku šablona:

# {project}/templates/users/index.html
{% load pagination_tags %}
{% autopaginate users %}
{% for user in users %}
{{ user.name }}
{% endfor %}
{% paginate %}

U stvarnoj aplikaciji, dodali biste dodatno označavanje i oblikovanje kako biste prikazali listu korisnika, ali to
pokazuje oznake koje vam dj-pagination postaju dostupne.

Sada kada imate prikaz i predložak, napravite urls.py datoteku za usmeravanje zahteva do prikaza:

# {project}/users/urls.py from django.urls import path from . import views
app_name = 'users' urlpatterns = [

# ex: /users/
path('', views.index, name='index'),

]

Na kraju, dodajte ga u svoje URL adrese:

// various imports... urlpatterns = [
// routes...
path('users/', include('users.urls')),

]

Sada, kada usmerite pregledač na /users stranicu, videćete listu korisničkih imena sa jednostavnim
kontrolama paginacije. Generisani upit vraća rezultate za manje od 100 ms, bez obzira na koju stranicu
pređem.

Ako tražite način da imate veću kontrolu nad paginacijom skupova ključeva, django-infinite-scroll-pagination
bi možda bilo vredno pogledati.

13.6.5 Zaključak
U ovom članku ste saznali kako funkcioniše paginacija u Djangu. Iako naivna paginacija dobro funkcioniše
za male tabele, ova metoda brzo smanjuje performanse kako vaša tabela raste na milione redova. Štaviše,
skok do zapisa duboko u tabeli biće veoma spor u upitu koji koristi OFFSET.

Dobra vest je da možete ubrzati stvari promenom COUNT upita. Pored toga, prelazak na paginaciju skupova
tastera poboljšaće performanse pretraživanja stranica i učiniće ih da rade u stalnom vremenu. Django
olakšava promenu podrazumevane konfiguracije, dajući vam moć da napravite efikasno rešenje za paginaciju
u Djangu.



P a g e | 99

14 Kako skalirati Django admin
Django pruža snažan I fleksibilan admin interfejs. Medjutim kako tabele bivaju punije sa podacima, počinje
usporavanje odgovora pri pretrazi. Sa nekom jednostavnim prilagodjenjima moguće je nastaviti sa
korišćenjemDjango admina čak i sa velikim dataset-ovima.

Evidentiranje
Većina Djangovog rada obavlja SQL upite, tako da će naš glavni fokus biti na smanjenju količine upita.
Da biste pratili izvršavanje upita, možete da koristite jedno od sledećeg:

- django-debug-toolbar
- možete prijaviti SQL upite na konzolu dodavanjem sledećeg u settings.py.

# settings.py:
LOGGING = {

...
'loggers': {

'django.db.backends':
{'level': 'DEBUG',

},
},
...

}

N + 1 problem
N + 1 problem je dobro poznat problem u ORM-ima. Da bismo ilustrovali problem, recimo da imamo ovu
šemu:

class Category(models.Model):
name = models.CharField(max_length=50)
def str(self):

return self.name
class Product(models.Model):

name = models.CharField(max_length=50)
category = models.ForeignKey(Category)

Implementacijom __str__ kažemo Django-u da želimo da se naziv kategorije koristi kao opis objekta.
Kad god ispisujemo objekt kategorije, Django će dohvatiti ime kategorije.Kod za admin stranu:
class Category(models.Model):

name = models.CharField(max_length=50)
def str (self):

return
self.name

class Product(models.Model):
name = models.CharField(max_length=50)
category = models.ForeignKey(Category)

Ovo se čini nedužnim, ali SQL dnevnik otkriva užas:



P a g e | 100
(0.0) SELECT COUNT(*) AS " count" FROM "app_product"; args=()
(0.002) SELECT "app_product"."id", "app_product"."name", "app_product"."category_id

FROM "app_product"
ORDER BY "app_product"."id"
DESC LIMIT 100; args=()

(0.000) SELECT ... FROM "app_category" where "app_category"."id" = 1; args=(1)
(0.000) SELECT ... FROM "app_category" where "app_category"."id" = 2; args=(2)
(0.000) SELECT ... FROM "app_category" where "app_category"."id" = 1; args=(1)
(0.000) SELECT ... FROM "app_category" where "app_category"."id" = 4; args=(4)
(0.000) SELECT ... FROM "app_category" where "app_category"."id" = 3; args=(3)
...
(0.0) SELECT ... FROM "app_category" where "app_category"."id" = 2; args=(2)
(0.000) SELECT ... FROM "app_category" where "app_category"."id" = 99; args=(99)
(0.000) SELECT ... FROM "app_category" where "app_category"."id" = 104; args=(104)

Django prvo broji objekte (o tome više kasnije), zatim preuzima stvarne objekte (ograničavajući se na
podrazumevanu veličinu stranice od 100), a zatim prosleđuje podatke u obrazac za prikazivanje.

Ime kategorije smo koristili kao opis objekta category, tako da za svaki proizvod Django mora da preuzme
naziv kategorije. Rezultat je dodatnih 100 upita.

Da bismo rekli Djangu da želimo da izvršimo pridruživanje umesto da preuzmemo imena kategorija jednu
po jednu, možemo koristiti list_select_related :

@admin.register(models.Product)
class ProductAdmin(admin.ModelAdmin):

list_display = ( 'id', 'name', 'category', )
list_select_related = ('category',)

Sada SQL dnevnik izgleda mnogo lepše. Umesto 101 upita imamo samo 1:
(0.004) SELECT

"app_product"."id", "app_product"."name", "app_product"."category_id",
"app_category"."id", "app_category"."name"

FROM
"app_product"

INNER JOIN app_category" ON
("app_product"."category_id" = "app_category"."id")

ORDER BY
"app_product"."id"

DESC LIMIT 100;

Da biste razumeli stvarni uticaj ove postavke, uzmite u obzir sledeće. Podrazumevana veličina stranice za
Django je 100 objekata. Ako imate jedno povezano polje, imate ~ 101 upit. Ako su u prikazu liste prikazana
dva povezana objekta, imate ~ 201 upit i tako dalje. Preuzimanje povezanih polja u relaciji može raditi samo
za veze ForeignKey. Ako želite prikazati relacije ManyToMany, malo je složenije (i najčešće pogrešno), ali
nastavite čitati.

Nadjačavanje get_queryset
Za strane ključeve i izračunata polja nema potrebe za pokretanjem dodatnih upita po vrsti. Izvršavanje
višestrukih upita za svaki objekat može dovesti do značajnog usporavanja. Možemo prepisati get_queryset
metod u model admin klasi, i koristiti select_releated metod na poljima stranih ključeva:



P a g e | 101

class MyModelAdmin(ModelAdmin):
def get_queryset(self, request):

queryset = super().get_queryset(request)
return queryset.select_related('foreign_key_1', 'foreign_key_2')

ili koristiti annotacije da smanjimo broj upita:
class MyModelAdmin(ModelAdmin):

def get_queryset(self, request):
queryset = super().get_queryset(request)
return queryset.annotate(field_count=Count('field'))

Poboljšani search sa DjangoQL
Standardni search radi dobro ali posle ulaska u sferu miliona zapisa, search postaje problem. Kada je više od
jednog polja u search_fields, SQL upit ima višestruke JOIN komande ulančane jedna na drugu. Rešenje
može biti uvodjenje DjangoQL paketa.

DjangoQLSearchMixin menja standardni Django search sa DjangoQL search-om. DjangoQL pomaže
nam da radimo search kroz specifikovana polja, uključujući povezana. Sve što je potrebno je dodavanje
DjangoQLSearchMixin na model admin:

from djangoql.admin import DjangoQLSearchMixin
class MyModelAdmin(DjangoQLSearchMixin, ModelAdmin):

pass

DjangoQL autokompletira polja modela, podržava logičke operatore, zagrade i povezivanje tabela. Možete
dobiti višestruke JOIN komande sa samo nekoliko linija koda i fokus na ono što pretražujete bez
ograničenja kao u tvrdo kodovanom search_fields.

Elminisanje COUNT upita
Posle dostizanja odredjene veličine, cena COUNT(*) upita postaje značajna na change_list i izaziva
timeout greške. MySQL je naš go-to izbor i mi koristimo Django-MySQL paket da proširimo Djangovu
ugradjenu podršku za MySQL. Paket dolazi sa korisnom count_tries_approx metodom koja vraća
aproksimativni broj umesto tačnog broja zapisa. Na taj način eliminišemo puni sken na InnoDB i
postižemo značjno brži rezultat.

class MyModelAdmin(ModelAdmin):
def get_queryset(self, request):

qs = super().get_queryset(request)
return qs.count_tries_approx()

Pre:
SELECT COUNT(*) AS ` count` FROM `mytable`
Posle: count_tries_approx:

SELECT
TABLE_ROWS

FROM
INFORMATION_SCHEMA.TABLES



P a g e | 102
WHERE

TABLE_SCHEMA = DATABASE() AND TABLE_NAME = 'mytable'

show_full_result_count
Sprečite Django da prikazuje ukupan broj redova u prikazu liste. Postavljanjem show_full_result_count =
False uklanjate upit count(*) na queryset-u pri svakom učitavanju stranice.

deffer
Prilikom izvršavanja upita, čitav skup rezultata se stavlja u memoriju radi obrade. Ako u svom modelu
imate velike kolone kao što su JSON ili tekstualna polja, možda bi bilo dobro odložiti ih dok zaista ne
budete trebali da ih koristite. Da biste odložili polja, zamenite metodu get_queryset.

Paginator
Paginator svakog admin pogleda zahteva ukupan broj zapisa, što dovodi do prebrojavanja zapisa preko
cele tabele. Za tabelu sa milionima zapisa, prebrojavanje može potrajati. Za admin pogled za velike
tabele, možemo pokušati da sprečimo prebrojavanje. Možemo nadjačati paginator atribut admin objekta
sa sledećim blokom koda.

from django.contrib import admin
from django.core.paginator import Paginator
class BigTablePaginator(Paginator):

@property
def count(self):

return
9999999

class BigTableAdmin(admin.ModelAdmin):
paginator = BigTablePaginator
show_full_result_count = False

Za svaku veliku tabelu, možemo naslediti BigTableAdmin. Na taj način možemo izbeći prebrojavanje zapisa
u velikim tabelama.

readonly_fields
Na stranici sa detaljima, Django kreira element za uređivanje za svako polje. Tekstualna i numerička polja
prikazivaće se kao redovno polje za unos. Polja izbora i polja stranog ključa prikazivaće se kao element
<select>.

Da bi prikazao polje za potvrdu, Django mora učiniti sledeće:
1. Preuzeti ceo povezani model i njegove opise.
2. Prikazati celu listu opcija, jedna opcija za svaku povezanu instancu.

Uobičajeni scenario koji se često previdi je strani ključ za model User. Kada imate 100 korisnika, možda
nećete primetiti opterećenje, ali šta se dešava kada imate 100.000 korisnika? Stranica sa detaljima preuzima
celu tabelu korisnika, a lista opcija će rezultirajući HTML učiniti ogromnim. Plaćamo dva puta, prvo za
skeniranje kompletne tabele, a zatim za preuzimanje html datoteke. A da se i ne spominje memorija potrebna
za generisanje html datoteke.

Imati element za odabir sa opcijama od 100.000 nije baš upotrebljivo. Najlakši način da sprečite Django da



P a g e | 103
prikazuje polje kao element <select> je da ga označite kao readonly_fields:

@admin.register(SomeModel)
def SomeModelAdmin(admin.ModelAdmin):

readonly_fields = ( 'user', )

Ovo će prikazati opis povezanog modela, bez mogućnosti da ga promenite u adminu. Druga opcija koja
sprečava Django da prikazuje okvir za odabir je označavanje polja kao raw_id_fields.

@admin.register(SomeModel)
def SomeModelAdmin(admin.ModelAdmin):

raw_id_fields = ('user', )

Koristeći raw_id_fields, Django će prikazati poseban vidžet koji prikazuje ID vrednosti i opciju za
otvaranje liste svih vrednosti u iskačućem prozoru. Ova opcija je veoma korisna kada želite da izmenite
vrednost stranog ključa.

Skaliranje Django admin date_hierarchy
Ako niste familijarni sa Django Admin date_hierarchy opcijom - trebalo bi, sjajna je.Postavlja date_hierarchy
atribut ModelAdmin na neko DateField polje:

from django.contrib import
adminfrom .models import Sale

@admin.register(Sale)
class SaleAdmin(admin.ModelAdmin):

date_hierarchy = 'created'

Na ovaj način dobijete lep propadajuči meni na vrhu list view strane. Kada izaberete godinu, Django filtrira
podatke i prikaže listu meseci za datu godinu.

14.10.1 Paket
U nekoliko projekata smo koristili ovu mogćnost. Odlučili smo da objavimo kao paket.

Instalacija:

$ pip install django-admin-lightweight-date-hierarchy

Dodati u INSTALLED_APPS:

INSTALLED_APPS = (
'django_admin_lightweight_date_hierarchy',

)

Postavi date_hierarchy_drilldown na False na ModelAdmin-u da bi se sprečilo podrazumevano propadajuće
ponašanje:

@admin.register(MyModel)
class MyModelAdmin(admin.ModelAdmin):

date_hierarchy = 'created'



P a g e | 104
date_hierarchy_drilldown = False

14.10.2 date_hierarchy
Otkrili smo da se ovaj indeks može koristiti za poboljšanje generisanja upita sa predikatom hijerarhije datuma
u PostgresSQL 9.4:

CREATE INDEX yourmodel_date_hierarchy_ix ON yourmodel_table (
extract('day' from created at time zone 'America/New_York'),
extract('month' from created at time zone 'America/New_York'),
extract('year' from created at time zone 'America/New_York')

)



P a g e | 105

15 JavaScript u Django adminu
Događaji u inline formi

Možda ćete želeti da pokrenete neki JavaScript kada se umetnuti obrazac (zapis) doda ili ukloni u obrascu
promene admina. formset:added iformset:removed jQueri događaji to dozvoljavaju.

Rukovaocu događaja se prenose tri argumenta:

- Event je jQuery događaj.
- $row je novododani (ili uklonjeni) red (zapis).
- formsetName je skup obrazaca kome red pripada.

Događaj se pokreće pomoću prostora imena django.jQuery.

U prilagođenom change_form.html šablonu proširite admin_change_form_document_ready blok i dodajte
kod:

#change_form.html
{% extends 'admin/change_form.html' %}
{% load static %}
{% block admin_change_form_document_ready %}
{{ block.super }}
<script src="{% static 'app/formset_handlers.js' %}"></script>
{% endblock %}

# app/static/app/formset_handlers.js
(function($) {

$(document).on('formset:added',
function(event, $row, formsetName){

if (formsetName == 'author_set') {
// Do something
}

});
$(document).on('formset:removed',

function(event, $row, formsetName) {
// Row removed

});
})(django.jQuery);

Dve stvari koje treba imati na umu:
- JavaScript kod mora da se nalazi u bloku šablona ako nasleđujete admin/change_form.html ili se neće

prikazati u konačnom HTML -u.
- {{ block.super }} se dodaje jer Djangov admin_change_form_document_ready blok sadržiJavaScript

kod za rukovanje raznim operacijama u obrascu za promenu, pa nam je potrebno i to da se prikaže.

Ponekad ćete morati da radite sa jQuery dodacima koji nisu registrovani u prostoru django.jQuery imena. Da
biste to uradili, promenite način na koji kod sluša događaje. Umesto da omotate slušaoca udjango.jQuery
imenski prostor, poslušajte događaj koji se odatle pokrenuo. Na primer:

#change_form.html
{% extends 'admin/change_form.html' %}



P a g e | 106
{% load static %}
{% block admin_change_form_document_ready %}
{{ block.super }}
<script src="{% static 'app/unregistered_handlers.js' %}"></script>
{% endblock %}

# app/static/app/unregistered_handlers.js
django.jQuery(document).on('formset:added', function(event, $row, formsetName) {

// Row added
});
django.jQuery(document).on('formset:removed', function(event, $row, formsetName) {

// Row removed
});

Kako raditi sa AJAX Request u Django
AJAX je akronim za Asynchronous JavaScript And XML. AJAX tehnologije omogućavaju web stranama da
rade ažuriranje asinhronom razmenom podataka sa serverom. U suštini, web strana može biti ažurirana bez
učitavanja kompletne web strane. To se ostvaruje preko XMLHttpRequest objekta za slanje i primanje
podataka.
Kada se događaj kao: slanje podataka forme, učitavanje strane ili kada je neko dugme kliknuto, na web
strani dogodi, jedan XMLHttpRequest objekat je kreiran i poslan je zahtev na server. Server će odgovoriti na
zahtev.
Zahtevani odgovor je uhvaćen i web server odgovara sa zahtevanim podacima.

AJAX je koristan u scenarijima kada želite da napravite GET i POST zahtev za učitavanjem podataka sa
servera asinhrono. To omogućava aplikacijama da budu dinamičnije i redukuje vreme učitavanja.

15.2.1 Kao Ajax radi u Django

15.2.1.1 Javascript kod u Browser/client-strani
Brauzer podnosi zahtev kada se događaj dogodi na web stranici. JavaScript kod generiše XHR objekat koji
sadrži JavaScript objekt ili podatke i šalje se kao objekt zahteva na webserver. XHR objekt će u osnovi



P a g e | 107
imenovati funkciju povratnog poziva na serveru ili URL-u.

15.2.1.2 Zahtevom upravlja webserver
Kada je zahtev poslan, odgovarajuća funkcija povratnog poziva rukuje zahtjevom i ona će poslati uspeh ili
neuspeh odgovor. Zbog asinhrone prirode zahteva, kod će se izvršiti bez prekida, a server obrađuje zahtev.

15.2.1.3 Uspešan ili neuspešan odgovor
Uspešan odgovor će sadržati podatke u više formata poput teksta, JSON, XML.

15.2.1.4 17.2.1.4 Stvarni primer
AJAKS ima veoma mnogo aplikacija na veb stranama i aplikacijama. Primer je taster sličan na Facebooku ili
Linkedln. Kada se post lajkuje, nekoliko akcija se izvodi i na serveru i na klijentu. Na primer, boja tastera
može se promeniti ili će se broj lajkova na postu biti uvećan u bazi podataka.

15.2.1.5 Pojednostavljeni proces
Kada se klikne na lajk dugme, XHR objekat se šalje na server, server prima zahtev i vrši radnju kreiranu za
zahtev. U ovom slučaju se registruje lajk za post. Server zatim šalje odgovor uspeha, a taster lajk menja boju
ili je povećan broj lajkova ili oboje.

Pogled na Django
Django je opisan kao web okvir za perfekcioniste sa rokovima i prevladava kao Pithon web okvir. Django se
može okarakterisati kao MVC (Model Viev Controller) i MTV (Model Template View). Klase modela su u
orm-u i instance odgovaraju redovima u tabeli. Šablon sistem je dizajniran za lakoću upotrebe i ograničava
meru u kojoj se HTML koristi kroz Python izvorni kod. View je funkcija koja pravi odgovor uz pomoć
šablona.

Korišćenje jQuery za AJAX u Django
JQuery je jedna od najpopularnijih JavaScript biblioteka koje su korišćene za razvijanje drugih okvira
JavaScript-a poput React-a ili Angular-a. Omogućava funkcije sa DOM na web stranama.

15.4.1 Zašto koristiti jQuery
15.4.1.1 Ajax ima drugačiju implementaciju na više brouzera
Različiti pretraživači koriste razne XHR API-e koji analiziraju zahtev i odgovoraju neznatno različito.
JQuery vam pruža ugrađeno rešenje da ažurirate svoj kod iako je to što je učinio nezavisno. JQuery kod se
aktivno razvija i stoga se ažurira.

15.4.1.2 jQuery pruža različite metode korišćenja DOM-a
jQuery sadrži mnoge DOM objekte i srodne metode. Ovi se objekti mogu koristiti u zavisnosti od potreba
programera, a JQuery ga čini pogodnijim za upotrebu.

15.4.1.3 JQuery je osnova za mnoge popularne okvire
Okviri poput React JS-a, Angular JS zasnivaju se na jQuery-u. jQuery je praktično svuda u popularnim
okvirima JavaScript. jQuery je veoma popularan.

15.4.1.4 jQuery je viralna biblioteka među web programerima
JavaScript okviri pružaju gotove widgete, a okviri mogu vam dozvoliti da napišete JavaScript višeg nivoa i
malo više pythonic. Postoje i drugi načini za upotrebu Ajax u Django-u, ali ovi razlozi čine JQuery
popularnim izborom. Stoga ćemo ga koristiti da bismo demonstrirali kako raditi sa zahtevima u Django-u.



P a g e | 108
15.4.1.5 Scenario registracija korisnika
Kao web programer, želite da validirate polje User name u registraciju ili u prikazu registracije, čim korisnik
završi upisivanje korisničkog imena. Provera koju želite da napravite je jednostavna.

15.4.2 Inicijalni setup
Posle inicijalnog setup-a za Django, kreiraj jedan projekat i base.html fajl.

# base.html
{% load static %}<!doctype html>
<html>

<head>
<meta charset="utf-8">
<title>{% block title %}Default Title{% endblock %}</title>
<link rel="stylesheet" type="text/css" href="{% static 'css/app.css' %}">
{% block stylesheet %}{% endblock %}

</head>
<body>

<header>
...

</header>
<main>

{% block content %}
{% endblock %}

</main>
<footer>

...
</footer>
<script src="https://code.jquery.com/jquery-3.1.0.min.js"></script>
<script src="{% static 'js/app.js' %}"></script>
{% block javascript %}{% endblock %}

</body>
</html>

Primetite da jQuery biblioteka i JavaScript resursi trebaju biti smešteni na kraj HTML strane da garantuje da
DOM će biti učitan kada je skript izvršen i da izbegne inlajn skriptove koji koriste jQuery.

# views.py
from django.contrib.auth.forms import UserCreationForm
f​ rom django.views.generic.edit import CreateView
class SignUpView(CreateView):

template_name = 'core/signup.html'
form_class = UserCreationForm

# url.py
from django.conf.urls import url
f​ rom core import views
urlpatterns = [
>path(' signup/', views.SignUpView.as_view(), name='signup')
]

# signup.html
{% extends 'base.html' %}



P a g e | 109
​
{% block content %}

<form method="post">
{% csrf_token %}
{{ form.as_p }}
<button type="submit">Sign up</button>

</form>
{​ % endblock %}

15.4.3 AJAX Request implementacija
Ispod implementacije AJAX-a je asinhroni zahtev da potvrdi da li je korisnički ulaz za korisničko ime već
iskorišćen ili je dostupan za upotrebu. Prvi korak uključuje gledanje HTML-a generisan kao {{form.AS_P}}
da bi pregledao polje za korisničko ime.

Da biste potvrdili korisničko ime, kreiramo listener za događaj promene id_username polja.

# signup.html
{​ % extends 'base.html' %}
{​ % block javascript %}

<script>
$("#id_username").change(function () {

console.log( $(this).val() );
});

​ </script>

​ {% endblock %}
{% block content %}

<form method="post">
{% csrf_token %}
{{ form.as_p }}
<button type="submit">Sign up</button>

</form>
{% endblock %}

U implementaciji iznad, događaj promene će se pojaviti svaki put kada se menja vrednost polja za korisničko
ime. Zatim kreiramo pogled koji proverava da li je korisničko ime prihvaćeno i vraća JSON odgovor.

# views.py
from django.contrib.auth.models import User
f​ rom django.http import JsonResponse
def validate_username (request):

username = request.GET.get('username', None)
data = {

'is_taken': User.objects.filter(username__iexact=username).exists()
}
return JsonResponse(data)

Sledeći korak je dodavanje rute na prikaz (views.py).

# urls.py
from django.conf.urls import url
f​ rom core import views



P a g e | 110
urlpatterns = [

path(' signup/', views.SignUpView.as_view(), name='signup'),
path(' ajax/validate_username/', , views.validate_username,

name='validate_username'),
]

Na kraju, implementiramo AJAX:

# signup.html
{​ % extends 'base.html' %}
{​ % block javascript %}

<script>
$("#id_username").change(function () {

​ var username = $(this).val();
$.ajax({

url: '/ajax/validate_username/',
data: {

'username': username
​ },

dataType: 'json',
success: function (data) {

if (data.is_taken) {
alert("A user with this username already exists.");

}
}

​ });
});

​ </script>
{% endblock %}

​ {% block content %}
<form method="post">

{% csrf_token %}
{{ form.as_p }}
<button type="submit">Sign up</button>

</form>
{% endblock %}

Kod iznad je dodao događaj za rukovanje korisničkim unosom, koji potom naziva funkciju koja koristi
AJAX da proveri da li je dato ime iskorišćeno i vraća odgovor.

15.4.4 Najbolje prakse za AJAX JavaScript u Django-u
- Izbegavajte modifikovanje JavaScript koda sa Python kodom.
- Držite URL reference u HTML-u i upravljajte korisničkim porukama u Pithon kodu
- Na HTML polju, preferirajte dodavanje ime klase.

Nakon što dodate ime klase, možete da priključite događaj za promenu na klasu js-validite-username. Prefiks
JS na imenu klase sugerira na JavaScript kod koji komunicira sa ovim elementom.

Obrasci su u opasnosti od Cross-Site Request Forgeries (CSRF). Da biste sprečili napade CSRF-a, dodajte



P a g e | 111
{% csrf_token%} šablon oznaku u obrazac. To će dodati polje skrivenog unosa koji sadrži token poslat sa
svakim POST zahtevom.

Ajax request u Django adminu na edit strani
U ovom tutorijalu učimo kako kreirati Ajax request za validaciju nekih podataka dok editujemo podatke u
Django admin edit formi.

15.5.1 Izgradnja Ajax request-a od nule u Django admin-u
Koristimo sledeći model:

class Product(models.Model):
user = models.ForeignKey(User, null=True, blank=True, on_delete=models.CASCADE)
refShop = models.ForeignKey(Shop, db_index=True,on_delete=models.CASCADE)
title = models.TextField(max_length=65)
price = models.FloatField(default=0)
withEndDate = models.BooleanField(default=False)
endDate = models.DateField(blank=True,null=True)
description = models.CharField(max_length=65, default='', null=True, blank=True)
createdAt = models.DateTimeField(auto_now_add=True)
updatedAt = models.DateTimeField(auto_now=True)
available = models.BooleanField(default=True)
def clean(self):

from datetime import datetime, timedelta
from django.core.exceptions import ValidationError
today = datetime.now().date()
if self.endDate:

if self.endDate < today:
raise ValidationError("You cannot set an endDate in the past")

if self.withEndDate and self.endDate is None:
raise ValidationError("Please select an end date !")

def unicode (self):
return u'%s - %s' % (self.refShop,self.title)

Recimo da bi smo, kada dodajemo ili editujemo model Product, voleli da verifikujemo cenu unešenu od
strane korisnika u odredjenom rangu vrednosti. Da bi to uradili treba da osluškujemo (korišćenjem javascript-
a) ’price’ ulaznu liniju, i ako je vrednost unešena, uradimo jedan Ajax zahtev da proverimo da li je cena u
odgovarajućem rangu.



P a g e | 112

Prvo moramo da prepišemo podrazumevani change_form.html fajl admina, prvo kreiramo novi direktorijum
<application>/templates/admin/<application>/product/

I kopiramo unutra fajl podrazumevani change_form.html fajl iz našeg virtualenv



P a g e | 113
cp .../site-packages/django/contrib/admin/templates/admin/change_form.html

Pre modifikacije tog fajla, pogledajmo izvorni kod strane, da bi znali kako je ’price’ polje imenovano:

<div class="form-row field-price">
<div>

<label class="required" for="id_price">Price:</label>
<input type="number" name="price" value="2.0" step="any" required id="id_price">

</div>
</div>

Kao što smo očekivali ulazno polje je idetifikovano sa “id_price”. Sada modifikujemo dati fajl dodavanjem
javascript koda unutar pratećeg bloka:
{% block admin_change_form_document_ready %}
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script> <script>

django.jQuery(document).ready(function(){
django.jQuery("#id_price").change(function(){

console.log("Price change");
});

})
</script>

Pre implementacije Ajax request-a, mi samo logujemo u javascript konzolu promenu cene, da proverimo je
sve OK. Ako je sve OK. Sada možemo implementirati Ajax poziv:
<script>

django.jQuery(document).ready(function(){
django.jQuery("#id_price").change(function(){

console.log("Price change");
var price = $(this).val();
var url="http://127.0.0.1:8000/backoffice/checkPrice/"
$.ajax({ // initialize an AJAX request

url: url, // set the url of the request
data: {

'price': price
},
success: function(data) { // `data` is the return of the `checkPrice`

view
// function

console.log(data);
}

});
});

})
</script>

Dobili smo cenu kao $(this).val(); i potom smo konstruisali naš url na kome ćemo napraviti poziv. Sada, ako
reučitamo našu stranu i pokušamo da promenimo cenu, videćemo na javascript konzolnom logu:



P a g e | 114

Imamo 404 error page što je logično jer još nismo implementirali checkPrice prikaz. Uradimo to.

U views.py fajlu aplikacije, kreirajmo jednostavnu funkciju prikaza da proverimo vrednost cene:
from django.http import JsonResponse
def checkPrice(request):

price = request.GET['price']
if int(price) > 10:

data = {"status": "KO"}
else:

data = {"status":"OK"}
return JsonResponse(data)

Do vas je da u prikazu implementirate vaša biznis pravila. Za ovaj primer, proverićemo da li je cena veća od
10 i i vraćamo nazad JSON sa KO ako nije ili OK statusom ako jeste.

Potom u našem backoffice direktorijumu, kreirajmo urls.py fajl:
# coding=utf-8
from django.conf.urls import url
from backoffice.views import *
urlpatterns = [

url(r'^backoffice/checkPrice/$', checkPrice, name='checkPrice'),
]

Na kraju deklarišemo novu rutu unutar projektnog urls.py fajla:
from django.conf.urls import url,include
from django.contrib import admin
from backoffice.admin import
site admin.site = site admin.autodiscover()
urlpatterns = [

url('', include('backoffice.urls')),url(r'^admin/', admin.site.urls),
]

Sada, ako reučitate stranu i pokušate da promenite cenu, videćete u konzolnom logu Ajax odgovor sa
statusom. Na vama je šta ćete da uradite sa Ajax odgovorom. Na primer, proverićemo da li je status OK i



P a g e | 115
ako jeste postavićemo cenu na 10 u input boksu.
<script>

django.jQuery(document).ready(function(){
django.jQuery("#id_price").change(function(){

console.log("Price change");
var price = $(this).val();
var url="http://127.0.0.1:8000/backoffice/checkPrice/"
$.ajax({ // initialize an AJAX request

url: url, // set the url of the
request data: {

'price': price
},
success: function (data) {// `data` is the return of the `checkPrice`

view
// function

console.log(data);
if (data["status"]=="KO"){

$("input#id_price").val(10);
}

}
});

});
})

</script>

Sada, ako reučitate admin stranu i pokušate da unesete vrednost veću od 10 za cenu, vrednost će automatski
biti postavljena na 10, zahvaljujući Ajax pozivu.



P a g e | 116

16 CSV export-import
CSV Export iz Django admina?

Od vas je zatraženo da dodate mogućnost izvoza Hero i Villain iz admina. Postoji veliki broj nezavisnih
aplikacija koje to omogućavaju, ali to je prilično lako i bez dodavanja nove zavisnosti. Dodaćete admin
akciju u HeroAdmin i VillanAdmin. Svaka admin akcija uvek ima isti potpis:
def admin_action(ModelAdmin, request, queryset):
ili alternativno možete dodati direktno u ModelAdmin kao:

class SomeModelAdmin(admin.ModelAdmin):
def admin_action(self, request, queryset):

Da dodate izvoz HeroAdmin-u možete napraviti sledeće:

actions = ["export_as_csv"]
def export_as_csv(self, request, queryset):

pass
export_as_csv.short_description = "Export Selected"

To dodaje akciju export_as_csv. Sada definišemo export_as_csv:
import csv
from django.http import HttpResponse
...
def export_as_csv(self, request, queryset):

meta = self.model._meta
field_names = [field.name for field in meta.fields]
response = HttpResponse(content_type='text/csv')
response['Content-Disposition'] = 'attachment';
filename={}.csv'.format(meta)
writer = csv.writer(response)
writer.writerow(field_names)
for obj in queryset:

row = writer.writerow([getattr(obj, field) for field in field_names])
return response

Ovo izvozi sve izabrane zapise. Primetite, export_as_csv nema ništa specifično sa Hero modelom, tako da
možemo ekstrahovati metod u mixin. Sa tim promenama, kod izgleda ovako:

class ExportCsvMixin:
def export_as_csv(self, request, queryset):

meta = self.model._meta
field_names = [field.name for field in meta.fields]
response = HttpResponse(content_type='text/csv')
response['Content-Disposition'] = 'attachment;
filename={}.csv.format(meta)
writer = csv.writer(response)
writer.writerow(field_names)
for obj in queryset:

row = writer.writerow(



P a g e | 117
[getattr(obj, field) for field in field_names])
return response

export_as_csv.short_description = "Export Selected"
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

list_display = ("name", "is_immortal", "category", "origin",
"is_very_benevolent")

list_filter = ("is_immortal", "category", "origin",
IsVeryBenevolentFilter)

actions = ["export_as_csv"]
...

@admin.register(Villain)
class VillainAdmin(admin.ModelAdmin, ExportCsvMixin):

list_display = ("name", "category", "origin")
actions = ["export_as_csv"]

Možete dodati CSV izvoz i drugim modelima, nasledjujući ExportCsvMixin.

CSV Import u Django admin
Od vas je zatraženo da dozvolite uvoz CSV-a na Hero adminu. To ćete uraditi dodavanjem linka na
listview stranu Hero, koji vodi na stranu sa obrascem za otpremanje.

Napisaćete hendler za akciju POSTza kreiranje objekata iz CSV-a:

class CsvImportForm(forms.Form):
csv_file = forms.FileField()

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

...
change_list_template = "entities/heroes_changelist.html"
def get_urls(self)get_urls(self):

urls = super().get_urls()
my_urls = [
...
path('import-csv/', self.import_csv),

]
return my_urls + urls

def import_csv(self, request):
if request.method == "POST":

csv_file = request.FILES["csv_file"]
reader = csv.reader(csv_file)
Create Hero objects from passed in
data#...
self.message_user(request, "Your csv file has been imported")
return redirect("..")

form =
CsvImportForm()

payload =



P a g e | 118
{"form":form}

return render(request, "admin/csv_form.html", payload)
)

Zatim kreirate entities/heroes_changelist.html šablon, nadjačavajući admin/change_list.html:

{% extends 'admin/change_list.html' %}
{% block object-tools %}
<a href="import-csv/">Import CSV</a>
<br />
{{ block.super }}
{% endblock %}

Na kraju kreirate csv_form.html:

{% extends 'admin/base.html' %}
{% block content %}
<div>
<form action="." method="POST" enctype="multipart/form-data">
{{ form.as_p }}
{% csrf_token %}
<button type="submit">Upload CSV</button>
</form>
</div>
<br />
{% endblock %}

Sa ovim promenama, dobijate link na listview strani.

CSV Export preko komandne linije
Napravimo fajl:

# app_name/management/commands/dump_app_name_csv.py
import os
import csv
from django.conf import settings
from academy.models import Invite
from django.core.management.base import BaseCommand
class Command(BaseCommand):

def handle(self, *args, **options):
print "Dumping CSV"
csv_path = os.path.join(settings.BASE_DIR, "dump_invites.csv")
csv_file = open(csv_path, 'wb')
csv_writer = csv.writer(csv_file)
csv_writer.writerow(['name', 'branch', 'gender', 'date_of_birth',

'race', 'notes', 'reporter'])
for obj in Invite.objects.all():

row = [obj.name, obj.branch, obj.gender, obj.date_of_birth,
obj.race,obj.notes, obj.reporter]

csv_writer.writerow(row)



P a g e | 119
Pokrenimo gornji fajl:

$ python manage.py dump_app_name_csv

Podaci će biti izvezeni u fajl "dump_invites.csv".

CSV Import preko komandnde linije
Napravimo sledeću šemu direktorijuma aplikacije:

app_name/
init .py

admin.py
apps.py
models.py
management
/

init .py
commands/

init .py
migrations/
tests.py
views.py

Napravimo fajl app_name/management/commands/load_app_name_csv.py sa sadržajem:
import csv
from app_name.models import Invite
from django.core.management.base import BaseCommand
class Command(BaseCommand):

def handle(self, *args, **options):
print "Loading CSV"
csv_path = "./app_name_invites.csv" Iz ovog csv se učitavaju podaci
csv_file = open(csv_path, 'rb')
csv_reader = csv.DictReader(csv_file)
for row in csv_reader:

obj = Invite.objects.create(name=row['Name'], branch=row['Branch'])
print obj

Pokrenimo gornji fajl:

$ python manage.py load_app_name_csv
Podaci će biti učitani u Invite tabelu iz fajla "./app_name_invites.csv".



P a g e | 120

17 Dozvole
Django pruža dobar okvir za autentifikaciju uz usku integraciju sa Django adminom. Django admin ne
primenjuje posebna ograničenja na korisnika admina. To može dovesti doopasnih scenarija koji mogu
ugroziti vaš sistem.

Da li ste znali da korisnici koji pripadaju staff-u i koji upravljaju drugim korisnicima u admin-u mogu da
uređuju svoje dozvole? Da li ste znali da oni takođe moguda naprave superuser-e? U administratoru
Django ne postoji ništa što to sprečava, pa jena vama!

Na kraju ovog vodiča znaćete kako da zaštitite svoj sistem:
- Od eskalacije dozvola sprečavanjem korisnika da uređuju sopstvene dozvole.
- Održavajte dozvole urednim i održivim, prisiljavanjem korisnike da koriste samo grupe.
- Sprečite curenje dozvola kroz prilagođene akcije izričitim sprovođenjem potrebnih dozvola.

Dozvole za model
Dozvole su škakljive. Ako ne podesite dozvole, sistem izlažete riziku od uljeza,curenja podataka i
ljudskih grešaka. Ako zloupotrebite dozvole ili ih previše koristite, rizikujete da ometate
svakodnevne operacije.

Django dolazi sa ugrađenim sistemom za potvrdu identiteta. Sistem za potvrdu identitetauključuje korisnike,
grupe i dozvole.

Kada se kreira model, Django će automatski stvoriti četiri podrazumevane dozvole za
sledeće akcije:

- add: Korisnici sa ovom dozvolom mogu dodati instancu (objekat) modela.
- delete: Korisnici sa ovom dozvolom mogu da izbrišu instancu (objekat) modela.
- change:Korisnici sa ovom dozvolom mogu da ažuriraju instancu (objekat) modela.
- view: Korisnici sa ovom dozvolom mogu da vide instance (objekte) ovog modela. Ova dozvola je bila

dugo očekivana i konačno je dodata u Django 2.1.

Imena dozvola slede vrlo specifičnu konvenciju imenovanja:

<app>.<action>_<modelname>

Razdvojimo to:
- <app> je naziv aplikacije. Na primer, model User se uvozi iz aplikacije auth (django.contrib.auth).
-<action> je jedna od gore navedenih akcija (add, delete, change ili view).
-<modelname> je ime modela, sva mala slova.

Poznavanje ove konvencije imenovanja može vam pomoći da lakše upravljate dozvolama. Na primer, ime
dozvole za promenu korisnika je auth.change_user.

Kako proveriti dozvole
Dozvole na modelu se odobravaju korisnicima ili grupama. Da proverite da li odredjeni korisnik ima dozvolu,
možete uraditi sledeće:

>>> from django.contrib.auth.models import User
>>> u = User.objects.create_user(username='haki')



P a g e | 121
>>> u.has_perm('auth.change_user')
False

.has_perm() će uvek vratiti True za aktivnog superuser-a, čak iako dozola zaista ne postoji:

>>> from django.contrib.auth.models import User
>>> superuser = User.objects.create_superuser(
... username='superhaki',
... email='me@hakibenita.com',
... password='secret',
... )

superuser.has_perm('does.not.exist')
True

Kada proverate dozovole za superuser, one se u stvarnosti ne proveravaju, sistem uvek vraća True.

17.2.1 Kako primeniti dozvole
Django modeli sami po sebi ne primenjuju dozvole. Jedino mesto gde se dozvole primenjuju podrazumevano
je Django admin. Razlog zbog kojeg modeli ne primenjuju dozvole je taj što model obično nije svestan
korisnika koji izvršava akciju. U Django app, korisnik se obično dobija iz request-a. Zbog toga se, najčešće,
dozvole primenjuju na sloju prikaza.

Na primer, da biste sprečili korisnika bez dozvole da pristupi pogledu koji prikazujekorisničke informacije,
uradite sledeće:

from django.core.exceptions import PermissionDenied
def users_list_view(request):

if not request.user.has_perm('auth.view_user'):
raise PermissionDenied()

Ako se korisnik, koji je podneo zahtev, prijavio se i bio potvrđen identitet, tada će request.user biti
instanca User, ako se korisnik nije prijavio, tada će request.user biti instanca AnonimousUser. Ovo je
poseban objekat koji Django koristi za označavanje neovlašćenog korisnika. Korišćenje has_perm na
AnonimousUser uvek vraća False.

Ako korisnik koji pravi zahtev nema dozvolu view_user, tada pokrećete izuzetak PermissionDenied i klijentu
se vraća odgovor sa statusom 403.

Da bi olakšao primenu dozvola u prikazima, Django nudi dekorator nazvan permission_required koji radi istu
stvar:

from django.contrib.auth.decorators import permission_required
@permission_required('auth.view_user')
def users_list_view(request):

pass

Da biste primenili dozvole u šablonima, trenutnim korisničkim dozvolama možete pristupiti putem
posebne promenljive šablona, naziva perms. Na primer, ako želite da prikažete dugme za brisanje samo
korisnicima sa dozvolom za brisanje, učinite sledeće:



P a g e | 122
{% if perms.auth.delete_user %}

<button>Delete user!</button>
{% endif %}

Neke popularne nezavisne aplikacije, poput Django rest framework-a, takođe pružaju korisnu integraciju sa
dozvolama za Django model.

Django admin i dozvole za model
Django admin ima vrlo tesnu integraciju sa ugrađenim sistemom za potvrdu identiteta, aposebno dozvole
za model. Django admin primenjuje dozvole za model:

-Ako korisnik nema dozvole za model, neće moći da ga vidi ili da mu pristupi u adminu.
-Ako korisnik ima dozvolu za pregled i promenu na modelu, tada će moći da pregleda i ažurira instance,

ali neće moći da dodaje nove instance ili briše postojeće.

Sa odgovarajućim dozvolama, admin korisnici će manje grešiti, a uljezi će teženaneti štetu.

Primenite prilagođene poslovne uloge u Django Admin
Jedno od najranjivijih mesta u svakoj aplikaciji je sistem za potvrdu identiteta. U Django app ovo je
User model. Dakle, da biste bolje zaštitili svoju aplikaciju, krenućete sa korisničkim modelom.

Prvo treba da preuzmete kontrolu nad admin stranicom User modela. Django već ima vrlo lepu admin
stranicu za upravljanje korisnicima. Da biste iskoristili taj sjajni posao,proširićete ugrađeni User model za
admin korisnika.

17.4.1 Postavka prilagođenog User admina
Da biste obezbedili prilagođenog admina za User model, morate da opozovete registracijupostojećeg User
modela koji nudi Django i da registrujete svoj:

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
# Deregistrujte osnovni user model
adminadmin.site.unregister(User)
# Registrujte svoj prilagodjeni user model admin:
@admin.register(User)
class CustomUserAdmin(UserAdmin):

pass

Vaš CustomUserAdmin je prošireni Djangov UserAdmin. Ako se sada prijavite na Django admin
http://127.0.0.1:8000/admin/auth/user, videćete nepromenjen User admin. Proširenjem UserAdmin-a,
možete da koristite sve ugradjene mogućnosti koje pruža Django admin.

17.4.2 Sprečavanje ažuriranja polja User modela
Admin forme bez nadzora glavni su kandidat za užasne greške. Korisnik koji pripada staff-u može lako
ažurirati instancu User modela putem admina na način koji aplikacijane očekuje. U većini slučajeva
korisnik neće ni primetiti da nešto nije u redu. Takve greške je obično vrlo teško pronaći i otkloniti.



P a g e | 123
Da biste sprečili da se takve greške dogode, možete sprečiti admine da menjaju određena polja u User
modelu. Ako želite da sprečite bilo kog korisnika, uključujući superuser-a, da ažurira polje,možete ga
označiti samo za čitanje. Na primer, polje date_joined se postavlja kada se korisnik registruje. Korisnik ove
informacije nikada ne sme da menja, pa ćete ih označiti kao samo za čitanje:

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

readonly_fields = [
'date_joined',

]

Kada se polje doda u readonly_fields, ono se neće moći uređivati u podrazumevanoj admin formi. Kada je
polje označeno samo za čitanje, Django će prikazati ulazni element kao onemogućen.

Ali, šta ako želite da sprečite samo neke korisnike da ažuriraju polje User modela?

17.4.3 Uslovno sprečavanje ažuriranja polja
Ponekad je korisno ažurirati polja User modela direktno u adminu. Ali ne želite da dozvolite bilo kom
korisniku da to radi, želite da dozvolite samo superuser-ima da torade.

Recimo da želite da sprečite korisnike koji nisu superuser-i da promene username korisnika. Da biste to
uradili, potrebno je da izmenite formu za promenu koji je generisao Django i onemogućite polje za username
na osnovu trenutnog korisnika.

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

def get_form(self, request, obj=None, **kwargs):
form = super().get_form(request, obj, **kwargs)
is_superuser = request.user.is_superuser
if not is_superuser:

form.base_fields['username'].disabled = True
return form

Da biste izvršili prilagođavanje obrasca, nadjačajte get_form()metodu. Ovu metodukoristi Django za
generisanje podrazumevanog obrasca za promenu modela. Da biste uslovno onemogućili polje, prvo
dobavite podrazumevani obrazac koji je generisao Django, a zatim, ako korisnik nije superuser,
onemogućite username polje.

Sada, kada korisnik koji nije superuser pokuša da izmeni username korisnika, polje usernameće biti
onemogućeno. Svaki pokušaj izmene username polja putem Django admina neće uspeti. Kada superuser
pokuša da izmeni username korisnika, polje username će moćida se uređuje i ponašaće se onako kako se
očekuje.



P a g e | 124
17.4.4 Sprečavanje ne superuser-a da daju superuser prava
Superuser je vrlo jaka dozvola koja se ne sme davati olako. Međutim, bilo koji korisnik sa dozvolom
za promene na User modelu može od bilo kog korisnika napraviti superuser-a, uključujući i same
sebe. To se protivi celoj svrsi sistema dozvola, paželite da zatvorite ovu rupu.

Na osnovu prethodnog primera, da biste sprečili korisnike koji nisu superuser-i da daju sami sebi prava
superusrer-a, dodajete sledeće ograničenje:

from typing import Set
from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

def get_form(self, request, obj=None, **kwargs):
form = super().get_form(request, obj, **kwargs)
is_superuser = request.user.is_superuser
disabled_fields = set() type: Set[str]
if not is_superuser:

disabled_fields |= {'username', 'is_superuser',}

for f in disabled_fields:
if f in form.base_fields:

form.base_fields[f].disabled =
True

return form

U get_form metodi možete inicijalizovati prazan set disabled_fields koji će sadržati polja za
onemogućavanje. Set je struktura podataka koja sadrži jedinstvene vrednosti. U ovom slučaju ima smisla
koristiti set, jer polje možete samo jednom onemogućiti. Operator |= se koristi za izvođenje ažuriranja na
setu. Dalje, ako korisnik nije superuser, u set dodajte dva polja (username i is_superuser). Oni će sprečiti
osobe koje nisu superuser-i da se naprave superuser-ima.

Na kraju, prelistate polja u skupu, označite ih kao onemogućena i vratite formu.

17.4.5 Django admin User forma u dva koraka
Kada kreirate novog korisnika u Django adminu, prolazite kroz formu u dva koraka. U prvom koraku
popunjavate username i password. U drugom koraku ažurirate ostatak polja. Ovaj postupak u dva koraka
jedinstven je za User model. Da biste prilagodili ovaj jedinstveni proces, morate da potvrdite da polje postoji
pre nego što pokušate da ga onemogućite. U suprotnom, možete dobiti KeyError.

17.4.6 Dodelite dozvole samo koristeći grupe
Način upravljanja dozvolama vrlo je specifičan za svaki tim, proizvod i kompaniju. Otkrio sam da je lakše
upravljati dozvolama sa grupama. U svojim projektima stvaramgrupe za podršku, urednike sadržaja,
analitičare itd. Otkrio sam da upravljanje dozvolama na korisničkom nivou može biti prava gnjavaža. Kada
se dodaju novi modeli ili kada se promene poslovni zahtevi, zamorno je ažurirati svakog pojedinačnog
korisnika.

Da biste upravljali dozvolama samo pomoću grupa, morate da sprečite korisnike da dodeljuju dozvole



P a g e | 125
drugim korisnicima ili sami sebi. Umesto toga, želite da dozvolite samo pridruživanje korisnika grupama.
Da biste to uradili, onemogućite polje user_permissions za sve korisnike koji nisu superuser-i:

from typing import Set
from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

def get_form(self, request, obj=None, **kwargs):
form = uper().get_form(request, obj, **kwargs)
is_superuser = request.user.is_superuser
disabled_fields = set() type: Set[str]
if not is_superuser:

disabled_fields |= {'username', 'is_superuser', 'user_permissions',}
for f in disabled_fields:

if f in form.base_fields:
form.base_fields[f].disabled =
True

return form

Za primenu drugog poslovnog pravila koristite potpuno istu tehniku kao i prethodno. U sledećim odeljcima
ćete primeniti složenija poslovna pravila kako biste zaštitili svoj sistem.

17.4.7 Sprečavanje ne superuser-a od promena njihovih vlastitih dozvola
Jaki korisnici su slaba tačka. Oni imaju jake dozvole, i potencijalno oštećenje koje mogu izazvati može
biti značajno. Za sprečavanje eskalacije prava u slučaju upada, možete sprečiti korisnike da menjaju svoje
dozvole:

from typing import Set
from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

def get_form(self, request, obj=None, **kwargs):
form = super().get_form(request, obj, **kwargs)
is_superuser = request.user.is_superuser
disabled_fields = set() type: Set[str]
if not is_superuser:

disabled_fields |= {'username', 'is_superuser', 'user_permissions',}
# Prevent non-superusers from editing their own permissions
if (not is_superuser and obj is not None and obj == request.user)

disabled_fields |= {
'is_staff', 'is_superuser', 'groups', 'user_permissions',

}
for f in disabled_fields:

if f in form.base_fields:



P a g e | 126
form.base_fields[f].disabled =
True

return form

Argument obj je instanca na kojoj trenutno radite:
-Kada je obj is None, forma se koristi za kreiranje novog korisnika.
-Kada je obj is not None, forma se koristiti za ažuriranje postojećeg korisnika.

Da biste proverili da li korisnik koji podnosi zahtev radi na sebi, uporedite sa request.user sa obj. Kada je za
korisnika koji pravi zahtev request.user==obj, to znači da se korisnik ažurira. U ovom slučaju
onemogućavate sva osetljiva polja koja se mogu koristiti za dobijanje dozvola.

Sposobnost prilagođavanja forme na osnovu objekta je veoma korisna. Može se koristiti za sprovođenje
složenih poslovnih uloga.

17.4.8 Nadjačavanje dozvola na User modelu
Ponekad može biti korisno potpuno nadjačati dozvole na User modelu Django admina.Uobičajeni scenario je
kada koristite dozvole na drugim mestima i ne želite da korisnici osoblja unose promene u admin.

Django koristi hook metode za četiri ugrađene dozvole. Interno, kuke koriste trenutne dozvole korisnika za
donošenje odluke. Možete da izmenite ove kuke i donesete drugačiju odluku.

Da biste sprečili korisnike osoblja da izbrišu instancu modela, bez obzira na njihove dozvole, možete učiniti
sledeće:

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

def has_delete_permission(self, request, obj=None):
return False

Baš kao i kod get_form(), obj je instanca kojom se trenutno upravlja:
-Kada je obj is None, korisnik je zatražio prikaz liste.
-Kada obj is not None, korisnik je zahtevao prikaz strane za promenu određene instance.

Imati instancu modela u ovim hook metodama vrlo je korisno za primenu dozvola na nivo instance za
različite tipove akcija. Evo i drugih slučajeva upotrebe:

- Sprečavanje promena tokom radnog vremena
- Implementacija dozvola na nivou instance

17.4.9 Ograničite pristup prilagođenim akcijama na User modelu
Prilagođene admin akcije zahtevaju posebnu pažnju. Django njih ne poznaje, tako da impodrazumevano
ne može ograničiti pristup. Prilagođena akcija biće dostupna bilo kom adminu sa bilo kojom dozvolom na
modelu.

Da bismo ilustrovali, dodajte praktičnu admin akciju da biste više korisnika označili
kao aktivne:



P a g e | 127

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

actions = [
'activate_users',

]
def activate_users(self, request, queryset):

cnt = queryset.filter(is_active=False).update(is_active=True)
self.message_user(request, 'Activated {} users.'.format(cnt))

activate_users.short_description = 'Activate Users' type: ignore

Koristeći ovu akciju, staff korisnik može označiti jednog ili više korisnika i aktivirati ih sve odjednom.
Ovo je korisno u svim vrstama slučajeva, na primer ako imate grešku u procesu registracije i trebate da
aktivirate korisnike na veliko. Ova akcija ažurira korisničke informacije, tako da želite da ih mogu
koristiti samo korisnici sa dozvolama za promenu.

Django admin koristi get_action metodu za dobijanje instance akcije. Da biste sakrili metodu
activate_users() od korisnika bez dozvole za promenu User modela, nadjačajte metodu get_action():
from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.adminimport UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

actions = [
'activate_users',

]
def activate_users(self, request, queryset):

assert request.user.has_perm('auth.change_user')
cnt = queryset.filter(is_active=False).update(is_active=True)
self.message_user(request, 'Activated {} users.'.format(cnt))

activate_users.short_description = 'Activate Users' type: ignore
def get_actions(self, request):

actions = super().get_actions(request)
if not request.user.has_perm('auth.change_user'):

del actions['activate_users']
return actions

get_action() vraća OrderedDict. Ključ je ime akcije, a vrednost funkcija akcije. Da biste prilagodili
povratnu vrednost, nadjačajte funkciju activate_users, preuzmete originalnu vrednost i u zavisnosti od
korisničkih dozvola uklonite.

Za staff korisnike bez dozvole change_user(), akcija activate_users se neće pojaviti upadajućem meniju
akcija.



P a g e | 128

17.4.10 Kako dozvoliti kreiranje samo jednog objekta u adminu?
UMSRA traži da ograničite broj Category objekata na 1. Oni žele da svaki entitet bude iste kategorije:

MAX_OBJECTS = 1
def has_add_permission(self, request):

if self.model.objects.count() >= MAX_OBJECTS:
return False

return super().has_add_permission(request)

Ovo će sakriti add dugme sve dok je ispunjen uslov. Možete postaviti MAX_OBJECTS na
bilo koju vrednost.

17.4.11 Kako ukloniti ‘Add’/’Delete’ dugme iz admina?
UMSRA je dodala sve Category i Origin objekte i želi da onemogući dalje dodavanje ilibrisanje objekata. To
ćemo postići nadjačavanjem has_add_permission i has_delete_permissionu Django adminu:

def has_add_permission(self, request):
return False

def has_delete_permission(self, request, obj=None):
return False

Primetite uklanjanje add dugmeta sa listview strane. Add i delete dugmad će takodje biti uklonjena sa
changeview strane.

Kako napraviti Django admin sigurnijim?
17.5.1 Koristite SSL
Postavite svoju lokaciju iza HTTPS-a. Ako ne koristite HTTPS, moguće je da neko uhodi vašu (ili lozinku
korisnika) dok ste u kafiću, na aerodromu ili na drugom javnom mestu. Pročitajte više o omogućavanju
SSL-a i dodatnim koracima koje ćete možda morati preduzeti u Django dokumentima).

17.5.2 Promenite URL
Promenite podrazumevani admin URL iz /admin/ u nešto drugo. Uputstva su u Django dokumentaciji, ali
ukratko, zamenite admin/ u svojoj URL konfiguraciji nečim drugim:

urlpatterns = [
path('my-special-admin-login/', admin.site.urls),

]

Za još veću sigurnost, smestite admin u potpunosti na drugi domen. Ako vam je potrebnajoš veća
sigurnost, poslužite admin iza VPN-a ili nekog drugog mesta koje nije javno.

Obavezno promenite ime tabele, ime indeksa, kolonu hijerarhije datuma i vremensku zonu.

17.5.3 Koristite 'django-admin-honeypot'
Nakon što premestite svoju admin lokaciju na novi URL(ili čak odlučite da je hostujete na svom domenu),
instalirajte biblioteku django-admin-honeypot na vaš stari /admin/ URLda biste uhvatili pokušaje da
hakuju vašu stranu.



P a g e | 129

django-admin-honeypot generiše lažni ekran za prijavljivanje admina i e-poštom će poslati adminu vaših
strana kad god neko pokuša da se prijavi na vaš stari /admin/ URL.

E-mail koji je generisao django-admin-honeypot sadržaće IP adresu napadača, tako da za dodatnu sigurnost
ako primetite ponovljene pokušaje prijave sa iste IP adrese možete toj adresi blokirati upotrebu vaše veb
lokacije.

17.5.4 Zahtevajte jače lozinke
Većina vaših korisnika će odabrati loše lozinke. Omogućavanje validacije lozinkemože osigurati da vaši
korisnici odaberu jače lozinke, što će zauzvrat povećati sigurnost njihovih podataka i podataka kojima imaju
pristup u adminu. Zahtevajte jake lozinke omogućavanjem provere lozinke.

Django dokumentacija ima sjajan uvod o tome kako omogućiti validatore lozinki koji se isporučuju sa
Django-om.

Pogledajte validacije lozinki nezavisnih proizvođača poput

django-zkcvbn-password

da biste lozinke vaših korisnika učinili još sigurnijima. Scot Hacker ima sjajan post o tome šta čini jaku
lozinku i primeni biblioteke python-zkcvbn u projektu Python.

17.5.5 Koristite dvofaktorsku autentifikaciju
Dvofaktorska autentifikacija (2FA) je kada vam je potrebna lozinka i nešto drugo za potvrdu identiteta
korisnika za vašu veb lokaciju. Verovatno ste upoznati sa aplikacijama koje zahtevaju lozinku, a zatim
vam pošalju kod za prijavu pre nego što vam omoguće prijavu, te aplikacije koriste 2FA.

Postoje tri načina na koje možete da omogućite 2FA na svojoj veb lokaciji
1. 2FA sa SMS-om, gde sms-om šaljete kod za prijavu. Ovo je bolje od zahteva samo lozinke, ali

SMS poruke je iznenađujuće lako presresti.
2. 2FA sa aplikacijom kao što je Google Authenticator, koja generiše jedinstvene kodove za

prijavljivanje za bilo koju uslugu na koju se registrujete. Da bi postavili ove aplikacije, korisnici
će morati da skeniraju QR kod na vašoj veb lokaciji da bi se registrovali sa svojom aplikacijom.
Tada će aplikacija generisati prijavni kod koji mogu koristiti za prijavljivanje na vašu veb
lokaciju.

3. 2FA sa Yubi.ey je najsigurniji način da omogućite 2FA na svojoj veb lokaciji. Ova metoda
zahteva da vaši korisnici imaju fizički uređaj, Yubi.ey, da se priključe na USBport kada pokušaju
da se prijave.

Biblioteka django-two-factor-auth može pomoći da omogućite neku od gore napomenutih 2FA metoda.

17.5.6 Koristite najnoviju verziju Django-a
Uvek koristite najnoviju minor verziju Django-a da biste bili u toku sa bezbednosnim ispravkama i
ispravkama grešaka. Od ovog pisanja, to je Django 2.0.1. Nadogradite na najnovije dugoročno izdanje
(LTS) što je pre moguće, ali svakako uverite se da je vašprojekat nadograđen pre nego što padne iz
podrške.



P a g e | 130
17.5.7 Nikada nemojte pokretati `DEBUG` u produkciji
Kada je DEBUG postavljen na True u vašoj datoteci podešavanja, prikazaće se greške sa potpunim
povratnim podacima koji će verovatno sadržati informacije za koje ne želite daih vide krajnji korisnici.
Možda ćete imati i druga podešavanja ili metode koji su omogućeni samo u režimu otklanjanja grešaka
koji mogu predstavljati rizik za vaše korisnike i njihove podatke. Da biste to izbegli, koristite različite
datoteke postavkiza lokalni razvoj i za produkcijsku primenu. Pogledajte sjajni uvod Vitora Freitasa o
korišćenju više datoteka za podešavanja.

17.5.8 Zapamtite svoje okruženje
Admin treba izričito da navede u kom ste okruženju kako biste sprečili korisnike daslučajno izbrišu
proizvodne podatke. To možete lako postići pomoću biblioteke

django-admin-env-notice

koja će postaviti baner označen bojama na vrh vaše administratorske stranice.

17.5.9 Proverite da li postoje greške
Ovo nije specifično za Django admina, ali ipak je odlična praksa da zaštitite svojuaplikaciju. Pronađite
sigurnosne greške pomoću

python manage.py check --deploy.

Ako ovu komandu pokrenete kada lokalno izvodite projekat, verovatno ćete videti nekaupozorenja koja
neće biti relevantna u proizvodnji. Na primer, podešavanje DEBUG je verovatno Ttrue, ali već koristite
zasebnu datoteku za podešavanja da biste se pobrinuli za to u proizvodnji, zar ne?

Izlaz za ovu naredbu će izgledati otprilike ovako:

?: (securiti.V002)
U svom MIDDLEWARE-u nemate 'django.middleware.clickjacking.XFrameOptionsMiddleware',
tako da se na vašim stranama neće prikazivati zaglavlje 'x-frame-options'. Osim ako postoji dobar
razlog da se vaša stranica prikazuje u okviru, trebali biste razmotriti omogućavanje ovog zaglavlja
kako biste sprečili napade kliktanjem.

?: (securiti.V012)
SESSION_COOCKIE_SECURE nije postavljeno na True. Korišćenje sigurnog kolačića sesije otežava
snifers-ima mrežnog prometa da otmu korisničke sesije.
?: (securiti.V016)
U svom MIDDLEVARE imate 'django.middleware.csrf.CsrfViewMiddleware', ali
CSRF_COOCKIE_SECURE niste postavili na True. Korišćenje CSRF kolačića otežava snifersima
mrežnog saobraćaja krađu CSRF tokena.

Provera sistema je identifikovala 3 problema.

17.5.10 Proverite veb sajt
Ovo je još jedan savet koji je specifičan za admina, ali je i dalje dobra praksa. Jednom raspoređeni na
lokaciji, pokrenite svoju veb stranicu kroz Sasha's Poni Checkup.Ova stranica će vam dati sigurnosnu
ocenu i urednu listu stvari koje treba da uradite da biste je poboljšali. Testiraće vašu veb lokaciju na neke



P a g e | 131
stvari koje smo gore naveli, a takođe će preporučiti i druge načine za zaštitu vaše veb lokacije od
određenih ranjivosti i vrsta napada.



P a g e | 132

18 Prevodi
Django je fantastičan veb okvir Pithon-a, a jedna od njegovih sjajnih funkcija je internacionalizacija (ili
"i18n"). Prilično je lako dodati prevode na skoro bilo koji string u aplikaciji Django, ali šta je sa
prevođenjem Admin stranice sajta? Naslovi, imena i akcije - svima su potrebni prevodi. Te admin stranice
se automatski generišu, pa kako se njihove reči mogu prevesti?

Ovaj vodič vam pokazuje kako je to lako uraditi.

I18N Pregled
Ako ste novi za prevode u Django, prvo pročitajte zvaničnu Translation stranicu. Ukratko, svi stringovi
kojima je potreban prevod treba da se prenesu u funkciju prevođenja za Pithon kod ili blok prevod za
Django šablonski kod. Django Managementkomande generišu datoteke poruka za specifične za jezik, u
kojima prevodioci dodajuprevode za obeležene stringove i konačno ih sastavljaju za upotrebu u aplikaciji.
Imajte na umu da prevodi zahtevaju da se gettext alati postave na vašu mašinu. Djangotakođe pruža neku
naprednu logiku za rukovanje posebnim slučajevima kao i formatima datuma i pluralizacijom.

Početno podešavanje
Projektu Django je potrebno malo osnovne konfiguracije pre nego što je uradi prevode,
koji je potreban i za glavnu lokaciju i admin.

18.2.1 Omogućavanje internacionalizacije
Budite sigurni da su sledeće postavke date u settings.py:

# settings.py
LANGUAGE_CODE = 'en-us' or other appropriate
codeUSE_I18N = True
USE_L10N = True
One su dodate podrazumevano. Booleani trebaju postavljeni na False za apps koje ne žele
internacionalizaciju sa malim dobicima u brzini, mi ih trebamo tako da će biti True da bi se prevodi
dešavali.

18.2.2 Promena lokalnih puteva
Podrazumevano se datoteke sa porukama generišu u lokalne direktorijume za svaku aplikaciju sa stringovima
označenim za prevod. Možete opciono da postavite LOCALE_PATHS da biste promenili puteve. Na primer,
možda će biti najlakše staviti sve datoteke sa porukama ujedan takav direktorij, a ne da ih podelite
aplikacijom:

# settings.py
LOCALE_PATHS = [os.path.join(BASE_DIR, 'locale')]

Ovo će izbeći umnožavanje prevođenja između aplikacija. To je dobra strategija za male projekte, ali budite
upozoreni da to neće dobro skalirati za veće projekte.

18.2.3 Middleware softver za automatski prevod
Django nudi LocaleMiddleware da automatski prevodi stranice koristeći "kontekstne tragove" poput
prefiksa jezika u URL-u, vrednosti sesija i kolačića. Dakle, ako korisnik pristupa veb lokaciji iz Kine,
tada bi trebalo da automatski dobije kineske prevode! Da biste koristili middleware, dodajte:



P a g e | 133

django.middleware.locale.localemiddleware

na postavku MIDDLEWARE u settings.py Proverite da li dolazi posle

SessionMiddleware
CacheMiddleware

a pre

CommonMiddleware,

ako se koriste i ti middleware programi.

settings.py
MIDDLEWARE = [

#...
'django.middleware.locale.LocaleMiddleware',
#...

18.2.4 Prefiks jezika šablona URL-a
Dobijanje automatskih prevoda iz kontekstnih tragova je sjajno, ali je ipak korisno imati direktne URL adrese
na različite prevedene stranice. Funkcija

i18n_patterns

može lako dodati kod jezika kao prefiks u obrasce URL-a. Može se primeniti na sve URL adrese za veb
lokaciju ili samo podskup URL-ova (kao što je Admin veb lokacija).

Opciono, mogu se postaviti šabloni tako da URL-ovi bez prefiksa jezika će koristiti podrazumevani jezik.
Glavna upozorenja za korišćenje i18n_patterns-a je da se mora koristiti iz root URLconf-a, a ne iz
uključenih. Projektna urls.py datoteka treba daizgleda ovako:

# urls.py
from django.conf.urls.i18n import i18n_patterns
from django.contrib import admin
from django.urls import path
urlpatterns = i18n_patterns(

...
path('admin/', admin.site.urls),
#...
#If no prefix is given, use the default language
prefix_default_language=False

)

18.2.5 Ograničavanje izbor jezika
Kada dodate jezičke prefikse u URL adrese, toplo preporučujem ograničavanje dostupnih jezika. Django
obuhvata gotove datoteke sa gotovim porukama za nekoliko jezika. Sajt bi izgledalo loše ako je, na primer,
prefiks "/fr/" bio dostupsn bez ikakvih francuskih prevoda. Podesite dostupne jezike koristeći jezike u
settings.py:



P a g e | 134

# settings.py
from django.utils.translation import gettext_lazy as _
LANGUAGES = [

('en', _('English')),
('zh-hans', _('Simplified Chinese')),

]

Primetite da jezički kodovi prate ISO 639-1 standard.

Prevodjenje
Sa gore navedenim konfiguracijama, prevodi se sada mogu dodati na glavnu lokaciju! Koraci ispod
prikazuju kako da dodate prevode posebno za admina. Ako ne postoji posebna potreba, koristite lijeni
prevod za sve slučajeve.

18.3.1 Gotove fraze
Admin stranice sajta automatski se generišu pomoću šablona sa puno konzerviranih fraza za stvari poput
"login" i "save" i "delete". Kako da njih prevedete? Srećom, Django već ima prevode za mnoge glavne
jezike. Pogledajte listu na:

django/contrib/admin/locale
za dostupne jezike. Django će automatski koristiti prevode za ove jezike na veb lokaciji Admin - nema
šta drugo što treba da uradite! Ako vam je potreban jezik koji nije dostupan, snažno vas ohrabrujem da
doprinesete novim prevodima na projektu Djangotako da ih svi mogu deliti.

18.3.2 Prilagođeni admin naslovi
Postoji nekoliko načina za postavljanje prilagođenih naslova Admin stranice. Moja poželjna metoda je da
ih postavim u root urls.py datoteku. Gde god da su postavljeni, označite ih za lenji prevod. Lako ih je
prevideti!

from django.contrib import admin
from django.utils.translation import gettext_lazy as _
admin.site.index_title = _('My Index Title')
admin.site.site_header = _('My Site Administration')
admin.site.site_title = _('My Site Management')

18.3.3 Imena aplikacija
Imena aplikacija su još jedan skup fraza koji se mogu lako propustiti. Dodajtepolje verbose_name
sa prevodnim stringom na svaku klasu AppConfig u projektu. Nemojte jednostavno pokušati
prevesti string za polje name: Django će dati runtime izuzetak!

from django.apps import AppConfig
from django.utils.translation import gettext_lazy as _
class CustomersConfig(AppConfig):

name = 'customers'
verbose_name = _('Customers')

18.3.4 Imena modela



P a g e | 135
Modeli su puni stringova kojima su potrebni prevodi. Evo stvari koje treba potražiti:

-Dajte svakom polju verbose_name vrednost, jer se identifikatori ne mogu prevesti.
-Označite tekstove pomoći, opise izbora i dodatne poruke kao prevodne.
-Dodajte Meta class sa verbose_name, verbose_name_plural vrednostima.
- Pazite na bilo koje druge stringove koje bi mogle trebati prevod.

Evo primera modela:

from django.db import models
from django.core.validators import RegexValidator
from django.utils.translation import gettext_lazy as
_
class Customer(models.Model):

name = models.CharField(
max_length=100,
help_text=_('First and last name.'),
verbose_name=_('name')

)
address =

models.CharField( max_le
ngth=100,
verbose_name=_('address')

)
phone =

models.CharField(max
_length=10,
validators=[

RegexValidator('^\d{10}$', _('Phone must be exactly 10 digits.'))
],
verbose_name=_('phone number')

)
class Meta:

verbose_name = _('customer')
verbose_name_plural = _('customers')

18.3.5 Pokreni komande
Kada su svi stringovi markirani za prevodjenje, generiši datoteke poruka:

python manage.py makemessages -lzh_Hans

Zatim kompajliraj poruke

python manage.py compilemessages

Upozorenje:
Jezički kod i naziv lokala mogu biti različiti! Na primer, uzmite pojednostavljene kineske: Kod jezika je "ZH-
Hans", ali ime lokala je "zh_hans". Primjetite podvlaku i velika slova.

Imena lokala često uključuju šifru zemlje i različite jezičke nijanse, poput američkog engleskog vs.
britanskog engleskog. Pogledajte django/contrib/admin/local za listu primera.



P a g e | 136

Bonus: Dugmad za jezike
Sa LocaleMiddleware i i18n_patterns, stranice trebaju biti automatski prevedene na osnovu konteksta ili
prefiksa URL-a. Međutim, ipak bi bilo sjajno dozvoliti korisnikuručno da prebaci jezik iz admin interfejsa.
Klik na dugme je intuitivnije od rada sa prefiksima URL-a.

Postoji mnogo načina za dodavanje jezičnih preklopnika na veb lokaciju Admin. Za mene je najlakši
način da dodate ikone zastave na naslovnu traku. Iza scene, svaka ikona zazastave bila bi povezana sa
URL-om prefiks jezika za stranicu. Na taj način, kad god korisnik klikne na zastavu, tada je ista stranica
učitana za željeni jezik.

18.4.1 Prekidač prefiksa jezičkog koda
Pošto URL putevi koriste i18n_patterns, njihovi jezički kodovi mogu biti uniformni. Pomoćna funkcija
može lako da doda ili zameni željeni jezik koda kao prefiks URL-u. Na primer, da pretvorimo "/admin/" i
"/en/admin /" u "/zh-hans/admin/" za pojednostavljeni kineski. Ova funkcija takođe treba da potvrdi da su
put i jezik tačni. Može se staviti bilo gde u projektu.

from django.conf import settings
def switch_lang_code(path, language):

# Get the supported language codes
lang_codes = [c for (c, name) in settings.LANGUAGES]
#Validate the inputs
if path == '':

raise Exception('URL path for language switch is empty')
elif path[0] != '/':

raise Exception('URL path for language switch does not start with "/"')
elif language not in lang_codes:

raise Exception('%s is not a supported language code' % language)
# Split the parts of the path
parts = path.split('/')
# Add or substitute the new language prefix
if parts[1] in lang_codes:

parts[1] = language
else:

parts[0] = "/" + language
#Return the full new path
return '/'.join(parts)

18.4.2 Prekidač prefiksa šablonskog filtera
Konačno, ovu funkciju mora da zove Django šablon kako bi se pružile veze do stranica specifičnih za
jezik. Dakle, potreban nam je prilagođeni filter šablona. Modul za implementaciju filtera može se staviti
u bilo koju aplikaciju, ali mora biti u podpaketu po imenu templatetags - tako Django zna da traži oznake
prilagođenih šablonai filtere. Novi filteri će biti lako pisati jer već imamo funkciju switch_lang_code.
(Razdvajanje logike za rukovanje prefiksom iz samog filtra.)

Kod je ispod:

# [app]/templatetags/i18n_switcher.py
from django import template
from django.template.defaultfilters import stringfilter



P a g e | 137
register = template.Library()
@register.filter
@stringfilter
def switch_i18n_prefix(path, language):
"""takes in a string path"""

return switch_lang_code(path,language)
@register.filter
def switch_i18n(request, language):

"""takes in a request object and gets the path from it"""
return switch_lang_code(request.get_full_path(), language)

18.4.3 Nadjačavanje Admin šablona
Konačno, admin šabloni moraju biti nadjačane tako da možemo dodati nove elemente na Admin stranice.
Bilo koji šablon admina može se nadjačati stvaranjem novih šablona istog imena pod [project-
root]/templates/admin. Sadržaj roditelja koristiće se ako se izričito ne nadjača unutar datoteke dečijeg
šablona. Pošto želimo da promenimo naslovnutraku, kreiramo novu datoteku šablona za base_site.html sa
sledećim sadržajem:
{% extends "admin/base_site.html" %}
{% load static %}
{% load i18n %}
<!--custom filter module -->
{% load i18n_switcher %}
{% block extrahead %}
<link rel="shortcut icon" href="{% static 'images/favicon.ico' %}" />
<link rel="stylesheet" type="text/css

href="{% static 'css/custom_admin.css' %}"/>
{% endblock %}
{% block userlinks %}
<a href="{{ request|switch_i18n:'en' }}">
<img class="i18n_flag" src="{% static 'images/flag-usa-16.png' %}"/> </a> /

<a href="{{ request|switch_i18n:'zh-hans' }}">
<img class="i18n_flag" src="{% static 'images/flag-china-16.png' %}"/> </a> /
{% if user.is_active and user.is_staff %}
{% url 'django-admindocs-docroot' as docsroot %} {% if docsroot %}
<a href="{{ docsroot }}">{% trans 'Documentation' %}</a> / {% endif %}
{% endif %}
{% if user.has_usable_password %}
<a href="{% url 'admin:password_change' %}">{% trans 'Change password' %}</a> /
{% endif %}
<a href="{% url 'admin:logout' %}">{% trans 'Log out' %}</a> {% endblock %}

Statički CSS fajl sa imenom css/custom_admin.css trebalo bi da ima sledećisadržaj:

.i18n_flag img
{width:
16px;
vertical-align: text-top;

}



P a g e | 138

Primjetite da je ceo userlinks blok morao da se prepisuje kako bi se na mesto postavilazastava. Statičke
datoteke slika zastava su jednostavno besplatne emojis zastave. One su hiperlinkovane na odgovarajući
jezik URL za stranicu: switch_18n primenjen je na objekt aktivnog zahteva da biste dobili željeni jezik -
prefiksovan put. (Napomena: U mom primeru sam uklonio vezu "View site" jer je moj sajt nije trebao.)

18.4.4 Završni prikaz
U mom projektu, ja sam izabrao da postavim jezički prefiks prekidač kod u posebnu aplikaciju i18n_switcher.
Fajlovi u mom projektu potrebni za admin jezičku dugmad su oraganizaovani na sledeći način:

Fajlovi u mom projektu potrebni za admin jezičku dugmad su oraganizaovani na sledeći način:

[root]
|-i18n_switcher
| |-templatetags
| | |- init .py
| | `-i18n_switcher.py
| |- init .py
| `-apps.py
|-locale
| `-zh_Hans
| `-LC_MESSAGES
| |-django.mo
| ` -django.po
|-static
| |-css
| | `-custom_admin.css
| `-images
| |-flag-china-16.png
| `-flag -usa-16.png
`-templates
`-admin
`-base_site.html

Pošto sam kreirao novu aplikaciju za novi kod, takodje sam morao da dodam ime app u INSTALLED_APPS
u settings.py:

# settings.py
INSTALLED_APPS = [

...
'i18n_switcher.apps.I18NSwitcherConfig',
...

]
Kao što je već pomenuto, ikone zastava u naslovnoj traci su jedan od načina da se omoguće jednostavne
veze do prevedenih stranica. Dobro funkcioniše kada postoji samo nekoliko dostupnih jezičkih izbora.
Drugačiji pogled bio bi bolji za više jezika, poputpadajuće liste, druge linije u naslovnoj traci, ili čak i na
footer-u stranice.
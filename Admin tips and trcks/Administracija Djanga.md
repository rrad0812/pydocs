﻿
# Administracija Django-a

> [!Note]
>
> Admin interfejs Django-a nije namenjen krajnjim korisnicima.

Admin interfejs se sastoji od kontrolne tabla sa prikazom liste svih registrovanih aplikacija i modela koji se nalaze u tim aplikacijama.

Kada kliknete na model, otvara se `changelist` stranica sa listom objekata za taj model. Mi ćemo je zvati `listview` stranica. Ova stranica prikazuje sve objekte sadržane u modelu. Kada kliknete na objekat na `listview` stranici, otvara se `changeview` stranica za `Add/Change` koja prikazuje polja i sadržaje za taj objekat.

## Registracija modela

Da biste koristili admin interfejs, morate da kreirate bazu podataka i superkorisnika da biste se prijavili. Pre nego što se bilo koji od vaših modela pojavi u admin interfejsu, morate ga registrovati u admin.py datoteci unutar svake aplikacije.

**Napravite superuser korisnika**:

Napravite superuser korisnika za prijavljivanje na admin veb interfejs tako što ćete otići do direktorijuma projekta i otkucati sledeću komandu:

```shell
python manage.py createsuperuser
```

Biće vam zatraženo korisničko ime, imejl, lozinka i ponovo lozinka.

**Pokrenite Pajton server**:

  ```shell
  python manage.py runserver
  ```

Otvorite veb pregledač i idite na `localhost:8000/admin` da biste se
prijavili na admin interfejs sa superuser korisnikom koga ste upravo kreirali.

Kada se prijavite, možete videti kontrolnu tablu admin sajta.

Uredite `admin.py` datoteku u vašoj aplikaciji i dodajte kod koji će
registrovati svaki model.

`app admin.py`

```py
from django.contrib import admin
from .models import Item
admin.site.register(Item)
```

## Prilagodite način prikazivanja modela

Kada registrujete svoje modele, možete prilagoditi način na koji se prikazuju na admin sajtu koristeći `ModelAdmin` klasu u istoj `admin.py` datoteci u kojoj registrujete svoj(e) model(e).

U nastavku su navedene popularne opcije za prilagođavanje stranice sa listom promena za model.

**list_filter**:

Omogućava filtriranje liste objekata po poljima naznačenim na desnoj bočnoj traci na

```py
list_filter = ['is_staff', 'is_superuser']
```

**list_edit**:

Omogućava vam da uredite polje direktno u redu tabele na stranici Lista promena. Pogledajte primer liste able koja se može uređivati ispod.

**actions_on_bottom**:

Podrazumevano, administratorske radnje se nalaze na vrhu stranice. Međutim, ako imate mnogo objekata, može biti zgodno da se radnje prikažu i na dnu liste.

Ispod su popularne opcije za prilagođavanje stranice Dodaj/Izmeni za model.

**prepopulated_fields**:

Popuniće određena polja u skladu sa uputstvima datim u rečniku. Često se koristi za prepopulated slugs.

```py
prepopulated_fields = {'slug': ('category', 'product')}
```

**inlines**:
  
Omogućava ugnežđene modele uređivanja unutar matičnog modela.

```py
inlines = [OtherModel]
```

### Prikaz liste

Podrazumevano, Django prikazuje `str()` svakog objekta u `listview` listi. Ali možete prikazati više polja na ovoj stranici dodavanjem `list_display` opcije i određivanjem koja polja treba prikazati.

```py
class QuestionAdmin(admin.ModelAdmin):
  # ...
  list_display = ('question_text', 'pub_date', 'was_published_recently')
```

Filter liste
Dodaje bočnu traku filtera koja omogućava ljudima da filtriraju listu promena prema poljima definisanim list_filter opcijom.

```py
date_hierarchy = 'publication_date'
fieldsets = (
  ...
  (_('Illustration'), {
    fields': ('image', 'image_caption'),
    'classes': ('collapse', 'collapse-closed')
  }),
  (_('Publication'), {
    'fields': ('publication_date', 'sites',
    ('start_publication', 'end_publication')),
  }),
  (_('Discussions'), {
    'fields': ('comment_enabled', 'pingback_enabled', 'trackback_enabled'),
    'classes': ('collapse', 'collapse-closed'),
  }),
  (_('Privacy'), {
    'fields': ('login_required', 'password'),
    'classes': ('collapse', 'collapse-closed')}),
  (_('Metadatas'), {
    'fields': (('content_template', 'detail_template'), 'authors',
      'related'),
  }),
  (None, {
    'fields': ('featured', )
  })
)
  ...
```

Primer liste koji se može uređivati

## Ugrađeni modeli

Administratorski interfejs vam omogućava da ugnezdite model za uređivanje unutar nadređenog modela. To se naziva `inlines`.

Postoje dve vrste ugrađenih polja:

- TabularInLine, koji postavlja svako polje jedno pored drugog.
- StackedInLine, koji postavlja svako polje preko drugog.

Primer StackedInline-a možete videti ispod:

```py
  ...
  ('Date information', {
    'fields': ['pub_date'], 
    'classes': ['collapse']
  }),
    
  inlines = [ChoiceInline]

admin.site.register(Question, QuestionAdmin)
```

Kod govori Djangu da se objekti Choice uređuju na admin stranici Question i da bi trebalo da bude dovoljno polja za tri izbora.

Ako umesto toga promenite ChoiceInline klasu u TabularInline model, prikaz se menja i izgleda ovako, što je u ovom slučaju mnogo preglednije:

**save_on_top**:

Obično se dugmad za čuvanje pojavljuju samo na dnu obrazaca. Ako vaš model ima mnogo polja, možda ćete želeti dugmad za čuvanje i na vrhu stranice. Da biste to uradili, podesite save_on_top=True i dugmad će se pojaviti i na vrhu i na dnu.

Promena načina prikazivanja objekata

Jedan od načina da prilagodite prikaz objekata jeste korišćenje __str__() metode prilikom prve konfiguracije modela. Podrazumevano, objekat će se prikazivati kao njegov ID broj kada se na njega pozivate. Ovo možete promeniti tako što ćete odrediti kako želite da se objekat modela prikazuje.

`models.py`

```py
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=100)
    body = models.TextField()

def __str__(self):
return self.title
```

### Callable

Možete koristiti pozivajuće metode i funkcije da biste dodali funkcionalnost Django administrativnom panelu. Ovo vam omogućava da
zaista izmenite listu i ekrane za prikaz kako bi odgovarali potrebama vašeg projekta.

Uobičajeni primer je definisanje funkcije koja prikazuje ciljni URL objekta.

```py
    url = reverse('ice_cream_bar_detail', kwargs={'pk': instance.pk}) 
    response = format_html("""<a href="{0}">{0}</a>""", url)
    return response

    show_url.short_description = 'Ice Cream Bar URL'

    # Displays HTML tags
    # Never set allow_tags to True against user submitted data!!!

    show_url.allow_tags = True
```

### Akcije administratora Django-a

Obično u Administratorskom delu birate stavku i menjate je. Međutim, Django Admin interfejs omogućava administratorske akcije koje vam omogućavaju da menjate više objekata odjednom. Akcija Delete selected <objects> dolazi podrazumevano, ali možete pisati i prilagođene funkcije.

Primer bi bio da vam omogućite da promenite status objavljivanja na više članaka na blogu odjednom.

Bezbednost administratora Django-a

Postoje načini da se dodatno obezbedi bezbednost administratorskom sajtu Django-a. U nastavku je nekoliko predloga.

Promenite podrazumevani URL

Podrazumevano, administratorska URL adresa je /admin/ . Trebalo bi da je promenite u nešto što je teško pogoditi kako biste izbegli
neželjeni pristup.

""" config/settings/base.py file """
...
# ADMIN
# -----------------------------------------------------------------------------
ADMIN_URL = "app-name-admin/"
...

Sakrij linkove do administratora

Ako koristite linkove menija ka administratorskom delu, koristite ih {% if user_staff %} da biste sakrili link od korisnika koji nisu ovlašćeni da koriste administratorski interfejs.

{% if request.user.is_staff %}
  <li class="nav-item"><a class="nav-link" href="{% url 'admin:index' % "target="_blank">
    Django Admin</a></li>
{% endif %}

Vizuelno razlikujte okruženja

Kada radite u više okruženja, lepo je imati način da ih vizuelno prikažete na različite načine.
Ovaj hak od Haki Benite vam omogućava da postavite obojenu traku preko vrha sa imenom okruženja tako da odmah znate u kom
administratorskom interfejsu radite.

1. Dodajte sledeća dva podešavanja u datoteku podešavanja, dajući im odgovarajuće nazive za okruženje koje datoteka podešavanja predstavlja: ENVIRONMENT_NAME i ENVIRONMENT_COLOR . Za proizvodna podešavanja, dodajte ove promenljive u .env datoteku (ili gde god da čuvate svoje promenljive okruženja).

# ------------------------------------------------------------------------------
ENVIRONMENT_NAME = env('DJANGO_ENVIRONMENT_NAME', default='Environmental Settings not working!')
ENVIRONMENT_COLOR = '#B20000'
...

2. Zamenite osnovni administratorski šablon.
U glavnoj /templates fascikli dodajte novu fasciklu pod nazivom /admin .
U /admin folderu kreirajte datoteku pod nazivom base_site.html . (Uverite se da je ovo ime tačno jer prepisujete postojeću datoteku koja ovo obrađuje.)

<!-- templates/admin/base_site.html -->
{% extends "admin/base_site.html" %}

{% block extrastyle %}

<style type="text/css">
body:before {

display: block;
line-height: 35px;
text-align: center;
font-weight: bold;
text-transform: uppercase;
color: white;
content: "{{ ENVIRONMENT_NAME }}";
background-color: {{ ENVIRONMENT_COLOR }};

    }
</style>

{% endblock extrastyle %}

Prilagođeni stil Django administratora

Postoji veoma brz i jednostavan način da prilagodite celokupni izgled i osećaj Django administracije izmenom base_site.html šablona i dodavanjem posebne admin.css datoteke.

Datoteka šablona izgleda ovako:

CSS datoteka izgleda ovako:

/* Django Admin specific styling */
#header {

height: 50px;
background: #333;
color: #fff; }

#branding h1 {
color: #fff; }

#content h1 {
font-size: 28px; }

a:link, a:visited {
color: #333; }

div.breadcrumbs {
background: #8bd9ef;
color: #25737f; }

div.breadcrumbs a {
color: #25737f; }

div.breadcrumbs a:hover {
color: #333; }

.module h2, .module caption, .inline-group h2 {
background: #3ec1d5; }

.button, input[type="submit"], input[type="button"], .submit-row input, a.button {
background: #333;
color: #fff; }

.button.default, input[type="submit"].default, .submit-row input.default {
background-color: #319aaa; }

Rezultat je administratorska kontrolna tabla koja odgovara stilu vaše veb stranice.

Dodatni paketi

Za dodatno prilagođavanje, možete koristiti dodatne pakete posebno dizajnirane za prilagođavanje Django administratorskog panela.

django-object-actions vam omogućava da kreirate prilagođena dugmad na administratorskoj stranici kao što je ova.

django-grappelli je praroditelj svih prilagođenih Django skinova. Stabilan je, robustan i sa jedinstvenim, ali prijateljskim stilom.

django-suit je napravljen korišćenjem poznatog Twitter Bootstrap front-end frejmvorka.
django-admin-tools je kolekcija ekstenzija/alata za podrazumevani django administratorski interfejs.

Kompletniju listu možete pronaći na https://www.djangopackages.org/grids/g/admin-styling/.

Django administratorska dokumentacija

Django ima aplikaciju koja preuzima dokumentaciju iz docstring-ova modela, prikaza, oznaka šablona i filtera šablona za bilo koju aplikaciju i čini tu dokumentaciju dostupnom Django administratoru. Veoma je korisno prilikom pregleda konfiguracije modela i provere da li je sve ispravno konfigurisano.

Modul admindocs se ne instalira automatski, pa ga morate dodati u podešavanja i URL datoteke koristeći sledeće korake.

1. Instalirajte docutils Pajton modul (http://docutils.sf.net/).
2. Dodajte django.contrib.admindocs svom INSTALLED_APPS .
3. Dodajte path('admin/doc/', include('django.contrib.admindocs.urls')) u svoj urlpatterns . Uverite se da je uključen pre unosa „admin/“, kako zahtevi ka /admin/doc/ ne bi bili obrađivani ovim drugim unosom.

Kako dokumentovati svoje aplikacije

Sledeće konvencije se koriste za kreiranje dokumentacije, zato se pobrinite da ih koristite u dokumentacionim stringovima komponenti vaše aplikacije.

Django komponenta Uloge restrukturiranog teksta

Pregledi :view:`app_label.view_name`

Filteri šablona :filter:`filtername`

Modeli
Odeljak o modelima na admindocs stranici opisuje svaki model u sistemu zajedno sa svim poljima, svojstvima i metodama koje su dostupne na njemu. help_text Atribut na poljima modela se koristi za formiranje opisa za polja modela u dokumentaciji.

    blog = models.ForeignKey(Blog, models.CASCADE)
    ...

def publish(self):
"""Makes the blog entry live on the site."""

        ...

Pregledi
Svaki URL na vašem sajtu ima poseban unos na stranici administratorske dokumentacije, a klikom na dati URL prikazaćete odgovarajući
prikaz.
Evo dobrog primera dokumentacije za prikaz:

from django.shortcuts import render

from myapp.models import MyModel

def my_view(request, slug):
"""

    Display an individual :model:`myapp.MyModel`.

    **Context**

    ``mymodel``
        An instance of :model:`myapp.MyModel`.

    **Template:**

    :template:`myapp/my_template.html`
    """
    context = {'mymodel': MyModel.objects.get(slug=slug)}

return render(request, 'myapp/my_template.html', context)

Oznake i filteri šablona
Odeljci za oznake i filtere u administratorskoj dokumentaciji opisuju sve oznake i filtere koji dolaze sa Django-om. Sve oznake ili filteri
koje kreirate ili koje doda aplikacija treće strane takođe će se pojaviti u ovim odeljcima.

Referenca šablona
Iako admindocs ne uključuje mesto za dokumentovanje samih šablona, korišćenje sintakse prikazane gore u dokumentacionom stringu
će proveriti putanju tog šablona pomoću Django-ovih učitavača šablona. Ovo može biti zgodan način da se proveri da li navedeni
šablon postoji i da se prikaže gde je taj šablon sačuvan na fajl sistemu.

Uslužni program komandne linije Django
I django-admin su manage.py pomoćne komande koje vam omogućavaju da obavljate administrativne zadatke van Django projekta.
Najčešće korišćene komande su startproject , startapp i runserver .

Upotreba:

shell – Pokreće interaktivni interpreter Pajtona. Ovo je veoma dobro jer vam omogućava da pokrećete testne naredbe na vašem Django kodu bez pokretanja servera.
dumpdata – Izvodi podatke iz baze podataka na standardni izlaz. Ovo se može uraditi za SVE podatke ili samo za podatke iz određene aplikacije.
loaddata – Pretražuje i učitava sadržaj imenovanog uređaja (kolekcije datoteka) u bazu podataka. Ovo je posebno korisno za potrebe testiranja ili za učitavanje sirovih podataka u druga okruženja.
Za kompletnu listu, pogledajte django-admin i manage.py .

Migracije

Migracije u Django-u se odnose na promene vaših modela koje utiču na vašu bazu podataka i ponekad mogu biti komplikovane. Pošto neki modeli imaju veze sa drugim modelima, često postoji zavisnost i ako migrirate jedan model pre drugog, možete na kraju imati problema u bazi podataka.

Opcija 1: Obriši bazu podataka

Često sam tokom razvoja morao da obrišem bazu podataka i počnem ispočetka. Kada se to desilo, ručno sam obrisao sve datoteke za migraciju zajedno sa .pyc datotekama za čist početak.
U svom članku „Kako resetovati migracije“ , Vitor Freitas nam daje neke brze i jednostavne komande za automatizaciju ovoga na operativnim sistemima sličnim Juniksu. Pokrenite komande ispod iz komandne linije u direktorijumu u kojem manage.py se nalazi vaša datoteka:

$ find . -path "*/migrations/*.py" -not -name "__init__.py" -delete
$ find . -path "*/migrations/*.pyc"  -delete

Nakon što ste obrisali staru bazu podataka i kreirali novu, možete pokrenuti svoje uobičajene komande za migraciju podataka:

$ python manage.py makemigrations
$ python manage.py migrate

Opcija 2: Obriši istoriju migracije, zadrži bazu podataka

Ponekad želite da obrišete istoriju migracije, ali da zadržite postojeću bazu podataka.

1. Uverite se da su sve migracije na čekanju završene pokretanjem komandi makemigrations i migrate .
2. Pregledajte koje su migracije izvršene koristeći showmigrations komandu poput ove:

$ python manage.py showmigrations

Rezultat će prikazati sve završene migracije sa „X“ u polju.

Terminal će prikazati nešto ovako:

Operations to perform:
Unapply all migrations: core

Running migrations:
Rendering model states... DONE
Unapplying core.0003_mymodel_bio... FAKED
Unapplying core.0002_remove_mymodel_i... FAKED
Unapplying core.0001_initial... FAKED

Ponovo pokrenite komandu python manage.py showmigrations i rezultati će pokazati ovo:

contenttypes
[X] 0001_initial
[X] 0002_remove_content_type_name
core
[ ] 0001_initial
[ ] 0002_remove_mymodel_i
[ ] 0003_mymodel_bio
sessions
[X] 0001_initial

Uradite ovo za svaku aplikaciju za koju želite da resetujete istoriju migracije.
4. Uklonite stvarne datoteke migracije.

Iz fascikle za migraciju morate ukloniti sve osim __init__.py datoteke. To možete uraditi ručno ili koristiti sledeće komande u
operativnom sistemu sličnom Juniksu:

$ find . -path "*/migrations/*.py" -not -name "__init__.py" -delete
$ find . -path "*/migrations/*.pyc"  -delete

Napomena: Obavezno navedite tačnu putanju do aplikacije. Gornji kod uklanja SVE datoteke za migraciju za sve aplikacije unutar
projekta.
Ako pokrenemo kod za core aplikaciju, a zatim pokrenemo showmigrations , dobijamo ove rezultate:

contenttypes
[X] 0001_initial
[X] 0002_remove_content_type_name
core
(no migrations)
sessions
[X] 0001_initial

5. Kreirajte početne migracije za novu bazu podataka.

$ python manage.py makemigrations



Operations to perform:
Apply all migrations: admin, core, contenttypes, auth, sessions

Running migrations:
Rendering model states... DONE
Applying core.0001_initial... FAKED

Kada showmigrations ponovo vidite navedenu jednu migraciju.

contenttypes
[X] 0001_initial
[X] 0002_remove_content_type_name core
[X] 0001_initial sessions
[X] 0001_initial

I sve bi trebalo da funkcioniše kako se očekuje.
Ako ne, možda ćete želeti da izdvojite svoje podatke, a zatim uradite opciju 1 gde smo obrisali i ponovo kreirali bazu podataka.
Komande za izdvojenje i uvoz podataka u Django-u su obrađene u sledećem odeljku.

Uvoz i izvoz podataka

Možete eksportovati i uvoziti podatke u bazu podataka pomoću uslužnog programa komandne linije. Međutim, upravljanje podacima zahteva malo više od onoga što vam govore uobičajeni Django dokumenti, posebno ako je vaša datoteka settings.py podeljena na više datoteka.

Izvoz podataka

Da biste izvezli podatke u JSON datoteku, iz komandne linije otkucajte:

$ python manage.py dumpdata <app.model> --output=path/<filename>.json --settings=<settings.py file to use>

Primer:

$ python manage.py dumpdata har.proptype –-output=fixtures/proptype.json --settings=config.settings.local

Ako ne navedete --output atribut, vaši podaci će biti poslati na konzolu i možda ćete videti nešto poput ovoga:

[{"model": "har.proptype", "pk": "PID1", "fields": {"name": "Single Family Homes"}}, {"model": "har.proptype", 
"pk": "PID2", "fields": {"name": "Townhouse / Condo"}}, {"model": "har.proptype", "pk": "PID3", 
"fields": {"name": "Residential Lots / Land"}}, {"model": "har.proptype", "pk": "PID4", "fields": 
{"name": "Multi-Family"}}, {"model": "har.proptype", "pk": "PID5", "fields": {"name": "Country Homes/Acreage"}}, 
{"model": "har.proptype", "pk": "PID6", "fields": {"name": "Mid/Hi-Rise Condominium"}}]

Uvoz podataka

Da biste uvezli podatke iz JSON datoteke u /fixtures direktorijumu, iz komandne linije otkucajte:

$ python manage.py loaddata <filename.json> --settings=<settings.py file to use>

Ovo je odlična lista od Danijela i Odri Roj Grinfeld za mesta za smanjenje uskih grla i ubrzanje vaših aplikacija.
Odlučite da li vam je potrebno ili ne. Mali sajtovi koji se dobro učitavaju možda nisu vredni truda.
Ubrzajte stranice sa puno upita:

Koristite django-debug-toolbar da biste utvrdili duplirane i spore upite.

Smanjite broj upita tako što ćete često korišćene upite premestiti u prikaz i dodati ih u kontekst kao promenljivu.
Implementirajte keširanje.
Koristite indeksiranje da biste ubrzali najčešće spore upite

Izvucite maksimum iz svoje baze podataka
Držite logove i efemerne podatke kao što su sesije, poruke i metrike van baze podataka. NJih treba čuvati u nerelacionim
skladištima.

Keširanje upita pomoću Memcached-a ili Redis-a
Identifikujte određena mesta za keširanje

Koji prikazi/šabloni sadrže najviše upita?
Koji URL-ovi se najviše traže?
Kada keš memorija za stranicu treba da bude nevažeća?

Razmotrite pakete za keširanje trećih strana
Kompresija i minimifikacija HTML-a, CSS-a i JavaScript-a

Imajte na umu da teranje Django-a i Python-a da obavljaju posao koristi sistemske resurse koji sami po sebi stvaraju usko
grlo. Uobičajeno je koristiti alat izvan Django-a za minimiziranje ovih elemenata.

Koristite uzvodno keširanje ili mrežu za isporuku sadržaja (CDN)

Keš
Kada nešto keširate, čuvate rezultat proračuna tako da ga ne morate ponovo izvršavati. Postoji nekoliko opcija u okviru Django-a za
keširanje:

Memkeš – baziran na memoriji (RAM) sa mogućnošću deljenja keša na više servera.
Baza podataka – čuva keširane podatke u bazi podataka. (Morate kreirati keš tabelu unutar baze podataka pre korišćenja ove
metode.)
Sistem datoteka – čuva svaku vrednost keša kao zasebnu datoteku
Lokalna memorija (podrazumevano) – metod keširanja bezbedan za svaki proces i niz niti
Fiktivno keširanje (za razvoj) – implementira interfejs keša bez ikakve radnje

Pogledajte Django-ov keš frejmvork za više detalja.

Asinhroni redovi zadataka
Asinhroni red zadataka je onaj u kome se zadaci izvršavaju u različito vreme od vremena kada su kreirani, a moguće i ne istim
redosledom kojim su kreirani.
Da li vam je potreban red zadataka zavisi od toga da li kod izaziva usko grlo i može se odložiti za kasnije kada procesor nije toliko
zauzet.
Pravilo iz knjige „Dve merice DŽanga“ je sledeće:

Obrada rezultata zahteva vreme: Verovatno bi trebalo koristiti red čekanja zadataka.
Korisnici mogu i treba odmah da vide rezultate: Red čekanja zadataka ne treba koristiti.



treba da vidite.
Naučite karakteristike vašeg softvera za redove čekanja zadataka i koristite njegovo rukovanje greškama.

Bezbednost
Postoji mnogo načina da dodate bezbednost svojoj Django veb stranici. Neke od ovih metoda su obezbeđene od strane Django-a, a
neke su stvari koje treba da uradite van same aplikacije.

Van DŽanga
Odvojite konfiguraciju od koda.
Ojačajte svoje servere podešavanjem zaštitnih zidova (fajervola), promenom SSH porta i onemogućavanjem/uklanjanjem
nepotrebnih servisa.
Budite u toku sa opštim bezbednosnim praksama.
Nikada ne čuvajte podatke o kreditnim karticama. Koristite usluge treće strane kao što su Stripe, Braintree i druge koje se bave
čuvanjem ovih informacija i razumeju sve PCI bezbednosne standarde.
Pratite svoje sajtove, redovno proveravajući evidencije pristupa i grešaka vaših veb servera u potrazi za sumnjivim aktivnostima.
Redovno ažurirajte svoje zavisnosti kako biste mogli da rešite sve bezbednosne ispravke koje postanu poznate.

Unutar DŽanga
Bezbednosna podešavanja Django-a

Koristite Django-ove ugrađene bezbednosne funkcije:
Zaštita od međusajtskog skriptovanja (XSS).
Zaštita od falsifikovanja zahteva na više sajtova (CSRF).
Zaštita od SQL injekcija.
Zaštita od klikdžeka pomoću DŽangoovog X_FRAME_OPTIONS podešavanja.
Podrška za TLS/HTTPS/HSTS, uključujući bezbedne kolačiće.
Bezbedno skladištenje lozinki, koristeći PBKDF2 algoritam sa SHA256 hešem podrazumevano.
Automatsko izbegavanje HTML-a.
Parser za iseljenike otporan na XML bombaške napade.
Ojačani alati za serijalizaciju/deserijalizaciju JSON, YAML i XML.

Koristite HTTPS svuda.
Koristite SSL sertifikat.

Dodajte django.middleware.security.SecurityMiddleware definiciji settings.MIDDLEWARE_CLASSES .
Postavi settings.SECURE_SSL_HOST=True .

Koristite bezbedne kolačiće sa SESSION_COOKIE_SECURE = True i CSRF_COOKIE_SECURE = True podešavanjima.
Koristite alate za konfiguraciju HTTPS-a

Ostala podešavanja Django-a
Isključite DEBUG režim u produkciji.
Držite svoje tajne ključeve skrivenim. To uključuje Django SECRET_KEY kao i vaše API ključeve za razne aplikacije.

Uverite se da nijedna .env datoteka nije otpremljena u vaš git repozitorijum tako što ćete je/ih dodati u
.gitignore datoteku.

Koristite validaciju dozvoljenih hostova u produkciji tako što ćete podesiti ALLOWED_HOSTS listu dozvoljenih imena hostova/
domena.
Uvek koristite CSRF zaštitu sa obrascima koji menjaju podatke.

koje šalju samo ključ klijentu. Međutim, sesije zasnovane na kolačićima smeštaju podatke sesije u potpunosti na klijentsku
mašinu i stvaraju bezbednosne probleme jer ključ i vrednosti nisu odvojeni.

Pazite na SQL injekcije napada.
Zaštitite se od XML bombardovanja pomoću defusedxml , Python biblioteke dizajnirane da zakrpi osnovne XML biblioteke Python-
a.
Istražite dvofaktorsku autentifikaciju.
Primorajte upotrebu jakih lozinki. Tipična pravila nalažu najmanje 8 znakova, kombinovanu upotrebu velikih i malih slova + brojeva
+ specijalnih znakova. Najbolja praksa sugeriše korišćenje iste kombinacije, ali sa najmanje 30 znakova.
Proverite bezbednost vašeg sajta koristeći spoljne servise.

Bezbednosna biblioteka pyup.io proverava vaše instalirane zavisnosti za poznate bezbednosne ranjivosti.
ponycheckup.com je automatizovani alat za bezbednosnu proveru Django veb lokacija.
observatory.mozilla.org pruža sličnu uslugu koja nije specifična za Django.

Nikada ne prikazujte sekvencijalne primarne ključeve jer oni obaveštavaju potencijalne rivale ili hakere o vašem tom-u, pružaju
mete za XSS napade i olakšavaju iskorišćavanje nebezbednih direktnih referenci objekata.

Vršite pretrage po poljima za slag.
Koristite UUID-ove

Obrasci
Validirajte sve dolazne podatke pomoću Django obrazaca.
Onemogućite automatsko dovršavanje u poljima za plaćanje jer mnogi ljudi koriste javne računare ili računare na javnim mestima.

Modeli
Ne koristite ModelForms.Meta.exclude jer omogućava promenu svih polja modela osim onih koja su navedena.
Ne koristite ModelForms.Meta.fields = "__all__" . Deluje kao prečica, ali otvara ranjivost sličnu funkciji exclude .
Pažljivo rukujte datotekama koje su otpremili korisnici korišćenjem mreža za isporuku sadržaja ili njihovim skladištenjem na potpuno odvojenom domenu.

Za datoteke koje su otpremili korisnici ( FileField i ImageField ), koristite paket treće strane (kao što je python-magic ili Pillow) da biste validirali tipove otpremljenih datoteka.

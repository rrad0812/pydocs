# Django Osnove

U ovom odeljku ćete naučiti osnovne Django koncepte tako što ćete napraviti blog koji omogućava korisnicima da se registruju, prijavljuju i objavljuju postove na blogu.

## Sadržaj

- [Početak rada](#početak-rada) </br>
  Pomoći će vam da počnete sa Django-om objašnjavajući šta je Django frejmvork, kako da:
  - instalirate Django paket,
  - podesite projekat i
  - pokrenete Django aplikaciju koristeći razvojni veb server.</br></br>

- [Kreiranje aplikacije](#kreiranje-aplikacije) </br>
  Pokazaćemo vam kako da:
  - kreirate blog aplikaciju u Django-u i
  - mapirate URL-ove sa prikazima (views).</br></br>

- [Šabloni](#šabloni) </br>
  Naučite kako da:
  - kreirate šablone i
  - prosleđujete promenljive iz funkcija prikaza u šablone.</br></br>

- [Modeli](#modeli) </br>
  Pokazaćemo vam kako da:
  - kreirate jednostavan Django model. </br></br>

- [Migracije](#migracije) </br>
  Naučite kako da:
  - napravite i primenite migracije. </br></br>

- [Administratorska strana](#administratorska-strana) </br>
  Pokazuje vam kako da:
  - koristite Django administratorsku stranicu. </br></br>

- [Forme](#forme) </br>
  Naučite kako da:
  - definišite ModelForm koja kreira novu objavu i
  - čuva je u bazi podataka. </br></br>

- [Fleš poruke](#fleš-poruke) </br>
  Pokazaće vam kako da
  - kreirate i prikazujete fleš poruke. </br></br>

- [Edit forme](#edit-forme)</br>
  Naučite kako da
  - kreirate obrazac za uređivanje koji ažurira objavu.</br></br>

- [Forme za brisanje](#forma-za-brisanje)</br>
  Pokazaćemo vam kako da
  - kreirate obrazac za brisanje koji briše objavu.</br></br>

- [Forma za prijavu](#forma-za-prijavu)</br>
  Kreirajte sistem za prijavu/odjavu za Django aplikaciju.</br></br>

- [Registraciona forma](#registraciona-forma)</br>
  Pokazaćemo vam kako da
  - kreirate registracioni formular koji omogućava korisnicima da kreiraju nalog.

## Početak rada

U ovom tutorijalu ćete naučiti kako da:

- kreirate novi Django projekat,
- razumete strukturu projekta i
- pokrenete Django veb aplikaciju iz veb pregledača.

### Pregled

Django je Python veb frejmvork koji uključuje skup komponenti za rešavanje uobičajenih problema veb razvoja.

Django vam omogućava brz razvoj veb aplikacija sa manje koda koristeći prednosti njegovog frejmvorka.

Django prati `DRY (don't repeat yourself)` princip, što vam omogućava da maksimizirate mogućnost ponovne upotrebe koda.

Django koristi `MVT (Model-View-Template)` obrazac, koji je malo sličan `MVC (Model-View-Controller)` obrascu.

`MVT` obrazac se sastoji od tri glavne komponente:

- `Model ( Model )` – definiše podatke ili sadrži logiku koja interaguje sa podacima u bazi podataka.
- `Prikaz ( View )` – komunicira sa bazom podataka preko modela i prenosi podatke u šablon za predstavljanje podataka.
- `Šablon ( Template )` – definiše šablon za prikazivanje podataka u veb pregledaču.

Sam Django frejmvork deluje kao kontroler. Django frejmvork koristi URL obrasce koji šalju zahtev odgovarajućem prikazu ( View ).

Ako ste upoznati sa MVC-om, sledeće je ekvivalentno:

- `Šablon (T)` je ekvivalentan prikazu ( View ) u MVC-u.
- `View (V)` je ekvivalentan kontroleru ( Controller ) u MVC-u.
- `Model (M)` je ekvivalentan modelu ( Model ) u MVC-u

U praksi ćete često raditi sa modelima, prikazima, šablonima i URL-ovima u Django aplikaciji.

### Django arhitektura

Sledi prikaz kako Django upravlja HTTP ciklusom zahteva/odgovora koristeći svoje komponente:

- Veb pregledač zahteva stranicu određenu URL-om od veb servera.
- Veb server prosleđuje HTTP zahtev Django-u.
- Django upoređuje URL sa URL obrascima kako bi pronašao prvo podudaranje.
- Django poziva prikaz ( View ) koji odgovara podudarnom URL-u.
- Prikaz koristi model za preuzimanje podataka iz baze podataka.
- Model vraća podatke u prikaz.
- Konačno, prikaz uključuje podatke u šablon ( Template ) i vraća ga kao HTTP odgovor.

### Kreiranje virtuelnog okruženja

Virtuelno okruženje kreira izolovano okruženje koje se sastoji od nezavisnog skupa Pajton paketa.

Korišćenjem virtuelnih okruženja možete imati više projekata koji koriste različite verzije Django-a. Takođe, kada premeštate projekat na drugi server, možete instalirati sve zavisne pakete projekta pomoću jedne `pip` komande.

Kako da kreirate virtuelno okruženje za Django projekat koristeći ugrađeni `venv` modul prikazano je ispod:

Kreirajte novi direktorijum `django-playground`:

```shell
mkdir django-playground
```

Idite do `django-playground` direktorijuma:

```shell
cd django-playground
```

Kreirajte novo virtuelno okruženje koristeći `venv` modul:

```shell
python -m venv venv
```

Aktivirajte virtuelno okruženje:

```shell
venv\scripts\activate
```

Terminal će prikazati sledeće:

```shell
(venv) D:\django-playground>
```

Imajte na umu da možete deaktivirati virtuelno okruženje pomoću `deactivate` komande:

```shell
deactivate
```

### Instalirajte Django paket

Pošto je Django paket treće strane, potrebno ga je instalirati prateći:

Sa sledećom `pip` komandom instalirate Django paket:

```shell
pip install django
```

Proverite verziju Django-a:

```shell
python -m django --version
```

Biće prikazano nešto ovako:

```shell
4.1.1
```

Obratite pažnju da verovatno vidite noviju Django verziju.

### Istraživanje django-admin komande

Django dolazi sa uslužnim programom komandne linije `django-admin` koji upravlja administrativnim zadacima kao što su:

- kreiranje novog projekta i
- pokretanje Django razvojnog servera.

Da biste naveli sve dostupne django-admin komande, izvršite sledeću komandu ovako:

```shell
django-admin
```

Izlaz:

```shell
Type 'django-admin help <subcommand>' for help on a specific subcommand.
Available subcommands:
[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    optimizemigration
    runserver
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    test
    testserver
```

Za sada nas zanima `startproject` komanda koja kreira novi Django projekat. Sledeća `startproject` komanda kreira novi projekat pod nazivom `django_project`:

```shell
django-admin startproject django_project
```

Ova komanda kreira `django_project` direktorijum sa standardnim subdirektorijumima.

Hajde da istražimo strukturu projekta:

```shell
cd django_project
```

Sledeća `django_project` struktura je prikazana:

```shell
├── django_project
| ├── asgi.py
| ├── settings.py
| ├── urls.py
| ├── wsgi.py
| └── __init__.py
└── manage.py
```

Evo kratkog pregleda svake datoteke u Django projektu:

- `manage.py` je program komandne linije koji se koristi za interakciju sa projektom, kao što je pokretanje razvojnog servera i pravljenje izmena u bazi podataka.

- `django_project` Pajton paket koji se sastoji od sledećih datoteka:
  - `__init__.py` – je prazna datoteka što ukazuje da django_projectje direktorijum paket.
  - `settings.py` – sadrži podešavanja projekta kao što su instalirane aplikacije, veze sa bazom podataka i direktorijumi šablona.
  - `urls.py` – čuva listu ruta koje mapiraju URL-ove na prikaze.
  - `wsgi.py` – sadrži konfiguracije koje pokreću projekat kao aplikaciju veb servera gateway interface (WSGI) sa WSGI-kompatibilnim veb serverima.
  - `asgi.py` – sadrži konfiguracije koje pokreću projekat kao asinhronu aplikaciju za interfejs veb servera (AWSGI) sa AWSGI-kompatibilnim veb serverima.

### Pokretanje razvojnog servera

Django dolazi sa ugrađenim veb serverom koji vam omogućava da brzo pokrenete svoj Django projekat u svrhu razvoja.

Veb server za razvoj Django-a će kontinuirano proveravati izmene koda i automatski ponovo učitavati projekat. Međutim, i dalje je potrebno ručno ponovo pokrenuti veb server u nekim slučajevima, kao što je:

- dodavanje novih datoteka u projekat.

Da biste pokrenuli Django razvojni server, koristite `runserver` komandu:

```shell
python manage.py runserver
```

Izlaz:

```shell
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
...
Django version 4.1.1, using settings 'django_project.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-C.
```

Kada server bude pokrenut i radi, možete otvoriti veb aplikaciju koristeći URL adresu navedenu u izlazu. Tipično, URL je nešto poput ovog:

```shell
http://127.0.0.1:8000/
```

Sada možete kopirati i nalepiti URL adresu u veb pregledač. Trebalo bi da se prikaže osnovna veb stranica servera.

`urls.py` sadrži podrazumevanu rutu koja mapira `/admin` putanju sa `admin.site.urls` prikazom:

```py
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

Da biste otvorili administratorsku stranicu, koristite sledeći URL:

```shell
http://127.0.0.1:8000/admin/
```

Prikazaće se stranica za prijavu:

```html
Početak rada sa Django-om - Prijava
```

### Zaustavljanje razvojnog servera

Da biste zaustavili razvojni server Django, otvorite terminal i dva puta pritisnite `Ctrl-C` ( ili `Command-C` na Mac OS).

### Napravite datoteku requirements.txt

Datoteka `requirements.txt` sadrži sve zavisnosti za određeni Django projekat. Takođe sadrži zavisnosti nad zavisnostima.

Da biste kreirali `requirements.txt` datoteku, pokrećete sledeću `pip` komandu:

```shell
pip freeze > requirements.txt
```

Kada premestite projekat na novi server, npr. testni ili produkcijski server, možete instalirati sve zavisnosti koje koristi trenutni Django projekat koristeći sledeću `pip` komandu:

```shell
pip install -r requirements.txt
```

### Rezime početka rada sa Django-om

- Django je Python veb frejmvork koji vam omogućava brz razvoj veb aplikacija.
- Django koristi `MVT (Model-View-Template)` obrazac, koji je sličan `MVC (Model-View-Controller)` obrazcu.
- Koristite `django-admin startproject new_project` komandu da biste kreirali novi projekat.
- Koristite `python manage.py runserver` komandu za pokretanje projekta koristeći Django razvojni veb server.
- Pritisnite dva puta `Ctrl-C` ( ili `Cmd-C` ) da biste zaustavili Django razvojni veb server.

[Sadržaj](#sadržaj)

## Kreiranje aplikacije  

U ovom tutorijalu ćete naučiti kako da:

- kreirate novu aplikaciju u Django projektu,
- kako da definišete prikaze (views) i
- kako da mapirate prikaze pomoću URL-ova.

### Django projekti i aplikacije

U okviru Django-a:

- Projekat je Django instalacija sa nekim podešavanjima.
- Aplikacija je grupa modela, prikaza, šablona i URL-ova.

Django projekat može imati jednu ili više aplikacija. Na primer, projekat je poput veb stranice koja može da se sastoji od nekoliko aplikacija kao što su blogovi, korisnici i vikiji.

Obično se dizajnira Django aplikacija koja se može ponovo koristiti u drugim Django projektima.

### Kreiranje blog aplikacije

Da biste kreirali aplikaciju, koristite `startapp` komandu:

```shell
python manage.py startapp app_name
```

Na primer, možete kreirati aplikaciju koja se zove `blog` pomoću `startapp` komande poput ove:

```shell
python manage.py startapp blog
```

Komanda kreira `blog` direktorijum sa nekim datotekama:

```shell
├── blog
|  ├── admin.py
|  ├── apps.py
|  ├── migrations
|  ├── models.py
|  ├── tests.py
|  ├── views.py
|  └── __init__.py
├── db.sqlite3
├── django_project
|  ├── asgi.py
|  ├── settings.py
|  ├── urls.py
|  ├── wsgi.py
|  ├── __init__.py
|  └── __pycache__
└── manage.py
```

### Registracija aplikacije

Nakon kreiranja aplikacije, potrebno je da je registrujete u projektu, posebno kada aplikacija koristi šablone i komunicira sa bazom podataka.

Aplikacija `blog` ima `apps.py` modul koji sadrži `BlogConfig` klasu poput ove:

```py
from django.apps import AppConfig

class BlogConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'blog'
```

Da biste registrovali `blog` aplikaciju, dodajete `blog.apps.BlogConfig` klasu na `INSTALLED_APPS` listu u `settings.py` projekta:

```py
INSTALLED_APPS = [
     # ...
    'blog.apps.BlogConfig',
]
```

Alternativno, možete koristiti ime aplikacije kao `blog` na `INSTALLED_APPS` listi ovako:

```py
INSTALLED_APPS = [
     # ...
    'blog',
]
```

### Kreiranje prikaza

Datoteka `views.py` u `blog` direktorijumu dolazi sa sledećim podrazumevanim kodom:

```py
from django.shortcuts import render

# Create your views here.
```

Datoteka `views.py` će sadržati sve prikaze aplikacije. Prikaz je funkcija koja prihvata `HttpRequest` objekat i vraća `HttpResponse`. Ekvivalentna je kontroleru u `MVC` arhitekturi.

Da biste kreirali novi prikaz, uvezite `HttpResponse` iz `django.http` u datoteku `views.py` i definišite novu funkciju koja prihvata instancu klase `HttpRequest`:

```py
from django.shortcuts import render
from django.http import HttpResponse

def home(request):
    return HttpResponse('<h1>Blog Home</h1>')
```

U ovom primeru, `home()` funkcija vraća `HttpResponse` objekat koji sadrži deo HTML koda. HTML kod sadrži `h1` oznaku.

Funkcija `home()` prihvata instancu objekta `HttpRequest` i vraća `HttpResponse` objekat. To se naziva prikaz zasnovan na funkciji. Kasnije ćete naučiti kako da kreirate prikaze zasnovane na klasi.

Da biste mapirali URL sa `home()` funkcijom, kreirate novu datoteku `urls.py` unutar `blog` direktorijuma i dodate sledeći kod u `urls.py` datoteku:

```py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='posts'),
]
```

Kako to funkcioniše?

- Uvezite `path` iz `django.urls` modula:

    ```py
    from django.urls import path
    ```

- Uvezite `views.py` modul iz trenutnog direktorijuma.

    ```py
    from . import views
    ```

    Imajte na umu da je ovo relativni uvoz koji uvozi views modul iz trenutnog direktorijuma.

- Definišite rutu koja mapira URL bloga sa `home()` funkcijom koristeći `path()` funkciju.

    ```py
    urlpatterns = [
        path('', views.home, name='posts'),
    ]
    ```

    Argument ključne reči `name` definiše ime rute. Kasnije, možete referencirati URL koristeći ime rute umesto čvrsto kodiranog URL-a kao što je `blog/`.

    </br>Korišćenjem imena za putanju, možete promeniti URL putanje u nešto drugo, kao `my-blog/` u `urls.py` umesto da menjate čvrsto kodirani URL svuda.

Imajte na umu da poslednji argument putanje mora biti argument ključne reči kao što je `name='posts'`. Ako koristite pozicioni argument kao što je ovaj:

```py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, 'posts'), # Error
]
```

dobićete sledeću grešku:

```shell
TypeError: kwargs argument must be a dict, but got str.
```

Da bi rute `blog` radile, potrebno je da uključite `urls.py` aplikacije `blog` u `urls.py` datoteku Django projekta:

```py
from django.contrib import admin
from django.urls import path, include # new

urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog/', include('blog.urls')), # new
]
```

U `urls.py` projekta, uvozimo `include` funkciju iz `django.urls` i mapiramo putanju `/blog` do `blog.urls`.

Na ovaj način, kada pređete na <http://127.0.0.1:8000/blog/>, Django će pokrenuti `home()` funkciju iz `views.py` modula i vratiti veb stranicu koja prikazuje `h1` oznaku.

Pre otvaranja URL adrese, potrebno je da pokrenete Django razvojni veb server:

```shell
python manage.py runserver
```

Kada odete na <http://127.0.0.1:8000/blog/>, videćete veb stranicu koja prikazuje `Blog Home` naslov.

Evo toka:

- Veb pregledač šalje HTTP zahtev na URL adresu <http://127.0.0.1:8000/blog/>

- Django izvršava `urls.py` u `django_project` direktorijumu. Upoređuje `blog/` sa URL-om u `urlpatterns` listi u `urls.py`. Kao rezultat toga, šalje  `''` u `urls.py` `blog` aplikacije.

- Django pokreće `urls.py` datoteku u `blog` aplikaciji. Uparuje URL sa `views`.`home` funkcijom i izvršava je, što vraća HTTP odgovor koji izbacuje `h1` oznaku.

- Konačno, Django vraća veb stranicu veb pregledaču.

### Dodavanje još ruta

- Definišite `about()` funkciju u `views.py` aplikaciji `blog`:

    ```py
    from django.shortcuts import render
    from django.http import HttpResponse

    def home(request):
        return HttpResponse('<h1>Blog Home</h1>')

    def about(request):
        return HttpResponse('<h1>About</h1>')
    ```

- Dodajte rutu u `urls.py` datoteku:

    ```py
    from django.urls import path
    from . import views

    urlpatterns = [
        path('', views.home, name='posts'),
        path('about/', views.about, name='about'),
    ]
    ```

- Otvorite URL adresu <http://127.0.0.1:8000/blog/about/> i videćete stranicu koja prikazuje stranicu "O nama".

- Sada, ako otvorite početnu adresu, videćete stranicu koja prikazuje da stranica nije pronađena sa HTTP kodom statusa 404.

    Razlog je taj što `urls.py` u `django_project` nema rutu koja mapira početnu URL adresu sa prikazom.

Da biste aplikaciju za `blog` postavili kao početnu stranicu, možete promeniti rutu sa `blog/` na `''` na sledeći način:

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```

Ako otvorite URL adresu <http://127.0.0.1:8000>, videćete početnu stranicu bloga. A navigacija do URL adrese <http://127.0.0.1:8000/about/> će vas odvesti na stranicu "O nama".

### Rezime kreiranja aplikacije

- Django projekat sadrži jednu ili više aplikacija.
- Django aplikacija je grupa `modela`, `prikaza`, `šablona` i `URL`-ova.
- Koristite python `manage.py` `startapp` `app_name` komandu za kreiranje nove Django aplikacije.
- Definišite funkciju u `views.py` datoteci da biste kreirali prikaz zasnovan na funkcijama.
- Definišite rutu u `urls.py` datoteci aplikacije da biste mapirali URL obrazac sa funkcijom prikaza.
- Koristite `include()` funkciju da biste uključili `urls.py` aplikaciju u `urls.py` projekta.

[Sadržaj](#sadržaj)

## Šabloni

U ovom tutorijalu ćete naučiti kako da:

- kreirate Django šablone,
- prenosite podatke iz funkcija prikaza u šablone i
- prikazujete podatke u šablonima.

### Uvod u Django šablone

U prethodnom tutorijalu ste naučili kako da vratite `HttpResponse` oznaku sa `h1` iz prikaza. Da biste vratili celu HTML stranicu, moraćete da koristite šablon.

Imajte na umu da je moguće vratiti celu HTML stranicu kombinovanjem HTML-a sa Python kodom. Međutim, to nije praktično i ne skalira se dobro.

`Šablon` je datoteka koja sadrži `statičke` i `dinamičke` delove veb stranice. Da bi generisao dinamičke delove veb stranice, Django koristi svoj specifičan jezik za šablone koji se naziva `Django jezik šablona` ili `DTL`.

Django endžin šablona prikazuje šablone koji sadrže `promenljive`, `konstrukcije`, `tagove` i `filtere`.

### Promenljive

Promenljiva je okružena sa `{{` i `}}`. Na primer:

```py
Hi {{name}}, welcome back!
```

U ovom šablonu, je `name` promenljiva. Ako je 'John'  vrednost  `name` promenljive, Django šablonski mehanizam će prikazati gornji šablon ovako:

```py
Hi John, welcome back!
```

Ako je promenljiva rečnik, stavkama rečnika možete pristupiti koristeći tačka notaciju ( `dict_name.key` ).

Pretpostavimo da imate `person` rečnik sa dva ključa `name` i `email`:

```py
person = {'name': 'John', 'email': 'john@pythontutorial.net'}
```

Vrednostima ključeva `name` i `email` rečnika `person` u šablonu možete pristupiti na ovaj način:

```py
{{ person.name }}
{{ person.email }}
```

### Tagovi

`Tagovi` su odgovorni za prikazivanje sadržaja, opsluživanje kontrolne strukture `if-else`, `for-loop` i dobijanje podataka iz baze podataka.

Tagovi su okruženi sa `{%` i `%}`. Na primer:

```py
{% csrf_token %}
```

U ovom primeru, `csrf_token` tag generiše token za sprečavanje `CSRF` napada.

Neki tagovi poput `if-else` i `for-loop` zahtevaju početne i završne oznake. Na primer:

```py
{% if user.is_authenticated %}
Hi {{user.username}} 
{% endif %}
```

### Filteri

Filteri transformišu sadržaj promenljivih i oznaka argumenta. Na primer, da biste svaku reč u stringu napisali velikim slovom, koristite `title` filter ovako:

```py
{{ name | title }}
```

Ako je vrednost promenljive `name` "john doe", onda će je `title` filter transformisati u sledeće:

```py
John Doe
```

Neki filteri prihvataju argument. Na primer, da biste formatirali datum promenljive `joined_date` u `Y-m-d` formatu, koristite sledeći filter:

```py
{{ joined_date | date: "Y-m-d" }}
```

Evo [kompletnih ugrađenih tagova i filtera šablona][01].

### Komentari

Komentari će izgledati ovako:

```py
{# This is a comment in the template #}
```

Django šablonski mehanizam neće prikazivati tekst unutar blokova komentara.

### Primeri Django šablona

- Kreirajte novi direktorijum pod nazivom `templates` unutar `blog` direktorijuma:

    ```shell
    mkdir templates
    ```

- Kreirajte `blog` direktorijum unutar `templates`direktorijuma:

    ```shell
    cd templates
    mkdir blog
    ```

    Imajte na umu da direktorijum unutar `templates` direktorijuma mora imati isto ime kao i ime aplikacije. U ovom primeru, `blog` direktorijum ima isto ime kao i `blog` aplikacija Django projekta.

- Unutar `templates/blog` direktorijuma kreirajte dve datoteke šablona `home.html` i `about.html` sa sledećim sadržajem.

    Datoteka `home.html`:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Home</title>
    </head>
    <body>
        <h1>Home</h1>
    </body>
    </html>
    ```

    Datoteka `about.html`:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>About</title>
    </head>
    <body>
        <h1>About</h1>
    </body>
    </html>
    ```

    Važno je napomenuti da bi trebalo da dodate blog aplikaciju na `INSTALLED_APPS` listu u `settings.py` datoteci da bi šabloni radili. Obično to radite odmah nakon kreiranja nove Django aplikacije.

    ```py
    INSTALLED_APPS = [
        # ...
        'blog.apps.BlogConfig',
    ]
    ```

- Otvorite `views.py` datoteku i promenite funkcije `home()` i `about()` prikaza na sledeće:

    ```py
    from django.shortcuts import render

    def home(request):
        return render(request, 'blog/home.html')

    def about(request):
        return render(request, 'blog/about.html')
    ```

    U ovoj `views.py` datoteci uvozimo `render()` funkciju iz `django.shortcuts`.

    Funkcija `render()` prihvata `HttpRequest` objekat i putanju do šablona. Ona prikazuje šablon i vraća `HttpResponse` objekat.

- Pokrenite Django razvojni server:

    ```shell
    python manage.py runserver
    ```

- Konačno, otvorite URL adresu <http://127.0.0.1:8000/> i URL adresu <http://127.0.0.1:8000/about/>, videćete kompletne HTML stranice koje dolaze iz `home.html` i `about.html` šablona.

### Prenošenje promenljivih u šablon

Napravićemo probne podatke o objavama na blogu i proslediti ih `home.html` šablonu. Kasnije ćete naučiti kako da dobijete podatke o objavama iz baze podataka.

Prikaz `views.py` izgleda ovako:

```py
from django.shortcuts import render

posts = [
    {
        'title': 'Beautiful is better than ugly',
        'author': 'John Doe',
        'content': 'Beautiful is better than ugly',
        'published_at': 'October 1, 2022'
    },
    {
        'title': 'Explicit is better than implicit',
        'author': 'Jane Doe',
        'content': 'Explicit is better than implicit',
        'published_at': 'October 1, 2022'
    }
]

def home(request):
    context = {
        'posts': posts
    }
    return render(request, 'blog/home.html', context)

def about(request):
    return render(request, 'blog/about.html')
```

Kako to funkcioniše.

- Kreirate novu listu `posts` koja čuva podatke o probnim objavama.
- Definišete novi rečnik `context` unutar `home()` funkcije sa ključem `posts` i prosledite ga funkciji `render()` kao treći argument.

Unutar `home.html` šablona, možete pristupiti podacima objave preko `posts` promenljive.

Sledi `home.html` šablon koji prikazuje objave:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blog</title>
</head>
<body>  
    {% for post in posts %}
        <h2>{{ post.title }}</h2>
        <small>Published on {{ post.published_at }} by {{ post.author}}</small>
        <p>{{ post.content }}</p>
    {% endfor %}
</body>
</html>
```

Kako to funkcioniše.

- Koristite `for` petlju za iteraciju kroz `posts` promenljivu. `for` petlja se završava sa `endfor`. I `for` i `endfor` su okruženi sa `{%` i `%}`.
- Upišete vrednost svake stavke u rečnik koristeći tačka notaciju ( `.` ).

Ako sačuvate `home.html` i otvorite URL adresu <http://127.0.0.1:8000/>, videćete podatke objave prikazane na stranici.

Pored `for` petlje, možete koristiti i druge kao uslovnu izjavu `if-else`. Na primer:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{% if title %} {{title}} {% else %} Blog {% endif %}</title>
</head>
<body>
    {% for post in posts %}
        <h2>{{ post.title }}</h2>
        <small>Published on {{ post.published_at }} by {{ post.author}}</small>
        <p>{{ post.content }}</p>
    {% endfor %}
</body>
</html>
```

Ovaj primer koristi `if-else` izjavu da bi pokazao da li je promenljiva `title` dostupna u `Blog` ili nije.

Da biste prosledili `title` promenljivu šablonu `home.html`, dodajete novi unos u `context` rečnik sa ključem `title` u `home()` funkciji kao što je ovaj:

```py
def home(request):
    context = {
        'posts': posts,
        'title': 'Zen of Python'
    }
    return render(request, 'blog/home.html', context)
```

Ako osvežite početnu URL adresu <http://127.0.0.1:8000/>, videćete novi naslov.

Tipično, veb sajt ima neke uobičajene delove kao što su `zaglavlje`, `podnožje` i bočna traka. Da biste izbegli njihovo ponavljanje u svakom šablonu, možete koristiti `osnovni` šablon.

### Kreiranje osnovnog šablona

- Kreirajte novi `templates` direktorijum u direktorijumu projekta (ne u `blog` aplikaciji):

    ```shell
    ├── blog
    ├── db.sqlite3
    ├── django_project
    ├── manage.py
    ├── templates
    └── users
    ```

- Dodajte direktorijum šablona opciji TEMPLATES u `settings.py` datoteci projekta:

```py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates' ],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

Imajte na umu da je `BASE_DIR` `Path` objekat koji dolazi iz `pathlib` ugrađenog modula. Kosa crta / je operator koji spaja `BASE_DIR` objekat sa `'templates'` stringom. Ova funkcija se u Pajtonu naziva preopterećenje operatora.

Zatim, kreirajte `base.html` u `templates` direktorijumu sa sledećim kodom:

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>{% if title %} {{title}} {% else %} Blog {% endif %}</title>
    </head>
    <body>
        {% block content %}
        {% endblock %}
    </body>
</html>
```

`base.html` služi kao osnovni šablon za druge šablone. Naziv osnovnog šablona može biti bilo šta poput `main.html`.

Nakon toga, promenite `home.html` šablon unutar `templates/blog` direktorijuma na sledeći način:

```py
{% extends 'base.html' %}

{% block content %}
        <h1>My Posts</h1>
    {% for post in posts%}
        <h2>{{ post.title }}</h2>
        <small>Published on {{ post.published_at }} by {{ post.author}}</small>
        <p>{{ post.content }}</p>
    {% endfor%}
{% endblock %}
```

Ovde se proširuje `home.html` šablon sa `base.html` koristeći `extends` tag.  `home.html` šablon ima svoj odeljak za `content` blok.

Takođe, promenite `about.html` šablon koji proširuje iz `base.html` šablona:

```py
{% extends 'base.html' %}

{% block content %}
<h1>About</h1>
{% endblock content %}
```

Na kraju, ponovo pokrenite Django razvojni server i otvorite URL <http://127.0.0.1:8000/>, i videćete promene.

### Konfigurišite statičke datoteke

Statičke datoteke su CSS, JavaScript i datoteke slika koje koristite u šablonima. Da biste koristili statičke datoteke u šablonima, pratite ove korake:

- Kreirajte `static` direktorijum unutar direktorijuma projekta:

    ```shell
    mkdir static
    ```

    Direktorijum projekta će izgledati ovako:

    ```shell
    ├── blog
    ├── db.sqlite3
    ├── manage.py
    ├── mysite
    ├── static
    └── templates
    ```

- Postavite `STATICFILES_DIRS` nakon `settings.py` datoteke `STATIC_URL` tako da Django može da pronađe statičke datoteke u `static` direktorijumu:

    ```py
    STATIC_URL = 'static/'
    STATICFILES_DIRS = [BASE_DIR / 'static']
    ```

- Kreirajte tri direktorijuma `js`, `css` i `images` direktorijum unutar `static` direktorijuma:

    ```shell
    ├── static
    |  ├── css
    |  ├── images
    |  └── js
    ```

- Kreirajte `style.css` unutar `CSS` direktorijuma sa sledećim sadržajem.

    ```css
    h1{
        color:#0052EA
    }

    form {
        max-width: 400px;
    }

    label, input, textarea, select{
        display:block;
        width:100%;
    }

    input[type="submit"]{
        display:inline-block;
        width:auto;
    }

    .errorlist {
        padding:0;
        margin:0;
    }
    .errorlist li{
        color:red;
        list-style:none;
    }

    .alert{
        padding:0.5rem;
    }

    .alert-success{
        background-color: #dfd
    }

    .alert-error{
        background-color:#ba2121;
        color:#fff;
    }
    ```

    Imajte na umu da koristimo samo neka jednostavna CSS pravila kako bi tutorijali bili lakši za praćenje. Naš primarni fokus je Django, a ne CSS ili JavaScript.

- Peto, kreirajte app.js unutar js direktorijuma pomoću sledećeg koda:

    ```js
    setTimeout(() => {
    alert('Welcome to my site!');
    }, 3000);
    ```

    Ovaj kod prikazuje upozorenje nakon što se stranica učita 3 sekunde .

- Uredite base.html šablon da biste učitali datoteke `style.css` i `app.js`:

    ```html
    {%load static %}
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <link rel="stylesheet" href="{% static 'css/style.css' %}" />
        <script src="{% static 'js/app.js' %}" defer></script>
        <title>My Site</title>
    </head>
    <body>
        {%block content%} 
        {%endblock content%}
    </body>
    </html>
    ```

- Ponovo pokrenite Django razvojni server, otvorite URL adresu <http://127.0.0.1:8000/> i videćete da se boja naslova menja u skladu sa CSS pravilom.

    Takođe, videćete upozorenje nakon oko 3 sekunde jer se JavaScript kod u `app.js` pokreće:

    ```shell
    django runserver blog - js
    ```

    Pošto se ne fokusiramo na deo koji se bavi JavaSkriptom, možete ukloniti kod iz `app.js` datoteke da biste nastavili sa sledećim tutorijalom.

### Rezime šablona

- Django šablon sadrži i statičke i dinamičke delove veb stranice.
- Django podrazumevano koristi Django Template Language (DTL) za kreiranje šablona.
- Koristi se `{{ variable_name }}` za prikaz vrednosti `variable_name` u šablonu.
- Koristi se `{% control_tag %}` za uključivanje kontrolne oznake u šablon.
- Koristi se `static` oznaku za učitavanje statičkih datoteka, uključujući CSS, JavaScript i slike.

[01]: <https://docs.djangoproject.com/en/4.2/ref/templates/builtins/>

[Sadržaj](#sadržaj)

## Modeli

U ovom tutorijalu ćete naučiti o Django modelima i kako da

- kreirate modele za vašu Django aplikaciju.

### Uvod u Django modele

U Django-u, model je podklasa klase `django.db.models.Model`. Model sadrži jedno ili više polja i metoda koje manipulišu tim poljima.

U suštini, Django model se mapira na jednu tabelu u bazi podataka u kojoj svako polje modela predstavlja kolonu u tabeli.

Aplikacija može imati nula ili više modela sačuvanih u `models.py` modulu. Na primer, sledeće definiše `Post` model za `blog` aplikaciju:

```py
from django.db import models
from django.utils import timezone

class Post(models.Model):
    title = models.CharField(max_length=120)
    content = models.TextField()
    published_at = models.DateTimeField(default=timezone.now)
```

Model `Post` ima polja `title`, `content`, i `published_at`. Na osnovu `Post` modela, Django će kreirati tabelu u bazi podataka sa sledećim SQL kodom:

```sql
CREATE TABLE "blog_post" (
  "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
  "title" varchar(120) NOT NULL, 
  "content" text NOT NULL, 
  "published_at" datetime NOT NULL, 
);
```

Imajte na umu da je gore generisani SQL za `SQLite` batu podataka. Ako koristite drugu bazu podataka, videćete da je SQL kod malo drugačiji.

Ime tabele `blog_post` se automatski izvodi iz imena aplikacije i modela:

```py
application.model
```

U ovom primeru, Django će kreirati tabelu `blog_post` za `Post` model.

Da biste naveli ime tabele umesto korišćenja podrazumevanog imena koje je generisao Django, možete koristiti `db_table` atribut klase `Meta` ovako:

```py
from django.db import models
from django.utils import timezone

class Post(models.Model):
    title = models.CharField(max_length=120)
    content = models.TextField()
    published_at = models.DateTimeField(default=timezone.now)
    
    class Meta:
        db_table = 'posts'
```

U ovom slučaju, `Post` model će se mapirati na `posts` tabelu umesto na generisanu `blog_post` tabelu. U ovom tutorijalu, koristićemo podrazumevano generisano ime tabele `blog_post`.

Prilikom kreiranja tabele, Django automatski dodaje `id` polje kao primarni ključ tabele. Polje `id` je polje sa automatskim povećanjem tipa navedenog u `settings.py` datoteci projekta:

```py
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```

Ako želite da odredite sopstveno polje primarnog ključa, potrebno je da ga eksplicitno definišete u modelu ovako:

```py
post_id = models.BigAutoField(primary_key=True)
```

U ovom primeru, `primary_key=True` označava da `post_id` je primarni ključ . Kada Django vidi polje u modelu sa `primary_key=True`, neće dodati automatsku `id` kolonu.

Django zahteva da svaki model ima tačno jedno polje sa `primary_key=True`.

### Korišćenje modela

Kada definišete modele, potrebno je da kažete Django-u da ćete ih koristiti tako što ćete registrovati ime aplikacije u `INSTALLED_APPS` listi u settings.pyprojektu:

```py
INSTALLED_APPS = [
    # ...
    'blog.apps.BlogConfig',
]
```

### Ugrađeni modeli

Django dolazi sa nekim ugrađenim modelima kao što je `User` iz `django.contrib.auth.models` modula. Da biste koristili `User` model, potrebno je da ga uvezete u `models.py` datoteku:

```py
from django.contrib.auth.models import User
```

### Strani ključevi

Svaku objavu u `blog` aplikaciji kreira korisnik, a korisnik može kreirati nula ili više objava. Ovo se naziva relacija `jedan-prema-više`.

Da biste modelirali odnos `jedan-prema-više`, koristite `ForeignKey` polje:

```py
from django.db import models
from django.utils import timezone
from django.contrib.auth.models import User


class Post(models.Model):
    title = models.CharField(max_length=120)
    content = models.TextField()
    published_at = models.DateTimeField(default=timezone.now)
    author = models.ForeignKey(User, on_delete=models.CASCADE)
```

Na osnovu ovog modela, Django će kreirati `blog_post` tabelu sa sledećom strukturom:

```sql
CREATE TABLE "blog_post" (
  "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
  "title" varchar(120) NOT NULL, 
  "content" text NOT NULL, 
  "published_at" datetime NOT NULL, 
  "author_id" integer NOT NULL 
      REFERENCES "auth_user" ("id") DEFERRABLE INITIALLY DEFERRED
);
```

U ovom primeru, `auth_id` je strani ključ koji kreira vezu između `blog_post` tabele i `auth_user` tabele. Imajte na umu da `auth_user` je tabela koju je obezbedio Django.

### Metoda __str()**

Da biste definisali string reprezentaciju modela, možete prepisati `__str__()` metod. Na primer:

```py
from django.db import models
from django.utils import timezone
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=120)
    content = models.TextField()
    published_at = models.DateTimeField(default=timezone.now)
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    
    def __str__(self):
        return self.title
```

Kada koristite instancu `Post` modela kao string, Django poziva `__str__()` metodu i prikazuje njen rezultat.

### Dodavanje klase Meta u klasu Model

Klasa `Meta` vam omogućava da konfigurišete model. Na primer, sledeće definiše `Meta` klasu unutar `Post` klase modela koja sortira objave po `published_at` opadajućem redosledu ( -published_at ), tj. prvo novije objave, a zatim starije.

```py
from django.db import models
from django.utils import timezone
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=120)
    content = models.TextField()
    published_at = models.DateTimeField(default=timezone.now)
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    
    def __str__(self):
        return self.title
    
    class Meta:
        ordering = ['-published_at']
```

Nakon definisanja modela, možete kreirati i primeniti migracije da biste kreirali tabele u bazi podataka, što ćemo obraditi u sledećem tutorijalu.

### Rezime modela

- Definišite sve modele u `models.py` datoteci Django aplikacije.
- Definišite klasu koja nasleđuje od `django.db.models.Model` da biste kreirali model.
- Model se mapira na tabelu u bazi podataka, u kojoj se svako polje mapira na kolonu u tabeli baze podataka.
- Metoda za nadjačavanje `__str__()` koja vraća string reprezentaciju modela.
- Koristite `Meta` klasu za konfigurisanje modela.

[Sadržaj](#sadržaj)

## Migracije

U ovom tutorijalu ćete naučiti kako da

- kreirate modele i
- koristite `Django migracije` za kreiranje tabela baze podataka.

### Uvod u Django komande za migraciju

Kada radite sa Django-om, ne morate da pišete SQL da biste kreirali nove tabele ili menjali postojeće tabele. Umesto toga, koristite `Django migracije`.

`Django migracije` vam omogućavaju da promene koje napravite na modelima prenesete u bazu podataka putem komandne linije.

Django vam pruža neke komande za kreiranje novih migracija na osnovu promena koje ste napravili na modelu i primenu migracija na bazu podataka.

Proces za unošenje izmena u modele, kreiranje migracija i primenu izmena u bazu podataka je sledeći:

1. Definišite nove modele ili napravite izmene na postojećim modelima.
2. Napravite nove migracije pokretanjem `makemigrations` komande.
3. Primenite promene iz modela na bazu podataka izvršavanjem `migrate` komande.

Pretpostavimo da definišete `Post` modele u `blog` aplikaciji ovako:

```py
from django.db import models
from django.utils import timezone
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=120)
    content = models.TextField()
    published_at = models.DateTimeField(default=timezone.now)
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    
    def __str__(self):
        return self.title
```

i možete kreirati novu migraciju pomoću `makemigrations` komande:

```shell
python manage.py makemigrations
```

Komanda `makemigrations` skenira `models.py` datoteku, detektuje promene i vrši odgovarajuće migracije. Prikazaće sledeći izlaz:

```shell
Migrations for 'blog':
  blog\migrations\0001_initial.py
    - Create model Post
```

Iza scene, komanda kreira datoteku `migrations\0001_initial.py` file.

Da biste pregledali SQL kod koji će Django pokrenuti za kreiranje `blog_post` tabele u bazi podataka, koristite `sqlmigrate` komandu:

```shell
python manage.py sqlmigrate blog 0001
```

U ovoj `sqlmigrate komandi`, `blog` je naziv aplikacije, a `0001` je broj migracije.

Odštampaće sledeće:

```sql
BEGIN;
--
-- Create model Post
--
CREATE TABLE "blog_post" (
  "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
  "title" varchar(120) NOT NULL, 
  "content" text NOT NULL, 
  "published_at" datetime NOT NULL, 
  "author_id" integer NOT NULL REFERENCES "auth_user" ("id") 
   DEFERRABLE INITIALLY DEFERRED
);

CREATE INDEX "blog_post_author_id_dd7a8485" 
ON "blog_post" ("author_id");

COMMIT;
```

Da biste primenili izmene na bazu podataka, izvršite `migrate` komandu:

```shell
python manage.py migrate
```

Prikazaće sledeći izlaz:

```shell
Operations to perform:
  Apply all migrations: admin, auth, blog, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying blog.0001_initial... OK
  Applying sessions.0001_initial... OK
```

Imajte na umu da je, pored primene migracije za `Post` model, Django takođe primenio migracije za ugrađene modele koji se koriste u autentifikaciji, autorizaciji, sesijama itd.

Ako ponovo izvršite `migrate` komandu i nema neprimenjenih migracija, komanda će ispisati sledeće:

```shell
Operations to perform:
  Apply all migrations: admin, auth, blog, contenttypes, sessions, users
Running migrations:
  No migrations to apply.
```

Da biste naveli migracije projekta i njihov status, koristite `showmigrations` komandu:

```shell
python manage.py showmigrations
```

Izlaz:

```shell
admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
 [X] 0003_logentry_add_action_flag_choices
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
 [X] 0008_alter_user_username_max_length
 [X] 0009_alter_user_last_name_max_length
 [X] 0010_alter_group_name_max_length
 [X] 0011_update_proxy_permissions
 [X] 0012_alter_user_first_name_max_length
blog
 [X] 0001_initial
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
sessions
 [X] 0001_initial
```

### Rezime migracija

- Koristite `makemigrations` komandu da biste izvršili migracije na osnovu promena koje ste napravili na modelima.
- Koristite `migrate` komandu da primenite promene iz modela na bazu podataka.
- Koristite `sqlmigrate` komandu da biste videli generisani SQL na osnovu modela.
- Koristite `showmigrations` komandu da biste naveli sve migracije i njihov status u projektu.

[Sadržaj](#sadržaj)

## Administratorska strana

U ovom tutorijalu ćete naučiti kako da

- kreirate superkorisnika i
- koristite ga za prijavu na administratorsku stranicu Django-a.

### Uvod u Django administratorsku stranicu

Kada kreirate novi projekat pomoću `startproject` komande, Django automatski generiše administratorsku stranicu za upravljanje modelima, uključujući kreiranje, čitanje, ažuriranje i brisanje, što je često poznato kao `CRUD`.

Da biste pristupili administratorskoj stranici, idite na URL adresu <http://127.0.0.1/admin/>. Otvoriće se stranica za prijavu.

Imajte na umu da Django navodi admin/ u `urls.py` projekta:

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('blog.urls'))
]
```

Django administrator zahteva nalog za prijavu. Stoga, potrebno je da kreirate korisnika pomoću `createsuperuser` komande.

### Kreiranje superkorisničkog naloga

Da biste kreirali nalog superkorisnika, koristite createsuperuserovu komandu:

```shell
python manage.py createsuperuser
```

Tražiće se

- korisničko ime,
- adresa e-pošte i
- lozinka:

```shell
Username: john          
Email address: john@pythontutorial.net
Password: 
Password (again): 
Superuser created successfully.
```

Pokrenite Django razvojni server:

```shell
python manage.py runserver
```

I prijavite se koristeći kreiranog korisnika, videćete podrazumevanu administratorsku stranicu koja upravlja korisnicima i grupama.

Da biste prikazali `Post` model na administratorskoj stranici, potrebno je da ga registrujete u `admin.py` datoteci aplikacije `blog`:

```py
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

U ovom kodu:

1. Uvezite `Post` iz `models.py` datoteke.
2. Registrujte ga koristeći `admin.site.register(Post)`.

Kada registrujete model, videćete da se pojavljuje na administratorskoj stranici.

Odavde možete upravljati objavama, uključujući kreiranje, ažuriranje, brisanje i pregled objava. Na primer, možete kreirati objavu klikom na dugme `Add`

Hajde da napravimo tri posta!

### Prikaži podatke iz baze podataka

Da biste prikazali objave iz baze podataka, potrebno je da promenite `home()` funkciju u `views.py` datoteci aplikacije `blog`:

```py
from django.shortcuts import render
from .models import Post

def home(request):
    posts = Post.objects.all()
    context = {'posts': posts}
    return render(request, 'blog/home.html', context)

def about(request):
    return render(request, 'blog/about.html')
```

Kako to funkcioniše.

- Uvezite `Post` model iz `models.py` modula:

    ```py
    from .models import Post
    ```

- Preuzmite sve objave iz baze podataka koristeći `Post` model:

    ```py
    posts = Post.objects.all()
    ```

    Metod `all()` vraća objekat `QuerySet` koji sadrži sve `Post` objekte iz baze podataka. Imajte na umu da ćete saznati više o tome kako interagovati sa bazom podataka u odeljku o Django `ORM`-u.

- Kreirajte `context` rečnik sa ključem `'posts'` i vrednošću `posts` kao `QuerySet`:

    ```py
    context = {'posts': posts}
    ```

- Nakon toga, prosledite `context` funkciji `render()`:

    ```py
    return render(request,'blog/home.html', context)
    ```

- Prikažite posts u `home.html` šablonu:

    ```html
    {% extends 'base.html' %}

    {% block content %}
    <h1>My Postsc</h1>
    {% for post in posts %}
        <h2>{{ post.title }}</h2>
        <small>Published on {{ post.published_at | date:"M d, Y" }} by {{ post.author | title}}</small>
        <p>{{ post.content }}</p>
    {% endfor %}
    {% endblock content %}
    ```

Ako otvorite URL adresu <http://127.0.0.1/>, videćete tri objave iz baze podataka.

### Rezime administratorske strane

- Django dolazi sa podrazumevanom administratorskom stranom koji vam omogućava da upravljate korisnicima, grupama i modelima.
- Koristite `createsuperuser` da biste kreirali `superkorisnika` za prijavljivanje na administratorski sajt Django-a.
- Koristite `admin.site.register` metodu za registraciju modela u administratorskom panelu.
- Koristite `all()` metod `Model.objects` da biste dobili sve modele kao `QuerySet` iz baze podataka.

[Sadržaj](#sadržaj)

## Forme

U ovom tutorijalu ćete naučiti kako da

- kreirate Django obrazac za
  - kreiranje,
  - ažuriranje i
  - brisanje objava u blog aplikaciji.

Django admin je dovoljno dobar za administratore da upravljaju sadržajem na osnovu modela. Međutim, kada želite da korisnici veb stranice upravljaju svojim sadržajem, potrebno je da kreirate odvojene formulare za njih.

### Uvod u Form-u

Rukovanje formama uključuje veoma složenu logiku:

- Pripremanje HTML šablona.
- Validiranje polja u pregledaču koristeći Javaskript ili ugrađenu HTML5 validaciju.
- Primanje vrednosti na serveru.
- Proveravanje vrednosti polja na serveru.
- Obrada vrednosti forme, kao što je čuvanje u bazi podataka ako su podaci forme ispravni.
- Ponovo renderovanje obrazac sa starim vrednostima i porukom o grešci ako podaci forme nisu ispravni.

Django forme pojednostavljuju i automatizuju skoro sve gore navedene korake.

Imajte na umu da možete napisati kod da biste sve gore navedene korake obavili ručno ako želite više prilagođavanja forme.

Sve forme u Django-u nasleđuju od `django.forms.Form` klase. `ModelForm` klasa vam omogućava da kreirate formu koja se povezuje sa modelom.

### Definisanje forme

- Kreirajte novu datoteku `forms.py` u `blog` direktorijumu aplikacije.

- Definišite novu formu pod nazivom `PostForm` koja nasleđuje klasu `ModelForm`:

    ```py
    from django.forms import ModelForm
    from .models import Post

    class PostForm(ModelForm):
        class Meta:
            model = Post
            fields = ['title','content', 'author', 'author']       
    ```

- U `post/url.py` definišite rutu koja prikazuje PostForm:

    ```py
    from django.urls import path
    from . import views

    urlpatterns = [
        path('', views.home, name='posts'),
        path('post/create', views.create_post, name='post-create'),
        path('about/', views.about, name='about'),
    ]
    ```

- Definišite `create_post()` funkciju koja prikazuje formu:

    ```py
    from django.shortcuts import render
    from .models import Post
    from .forms import PostForm

    def create_post(request):
        if request.method == 'GET':
            context = {'form': PostForm()}
            return render(request, 'blog/post_form.html', context)

    def home(request):
        posts = Post.objects.all()
        context = {'posts': posts}
        return render(request, 'blog/home.html', context)

    def about(request):
        return render(request, 'blog/about.html')
    ```

- U `create_post()`, ako je HTTP zahtev GET, onda kreirajte novu instancu klase `PostForm` i prosledite je funkciji `render()`.

- Kreirajte `post_form.html` šablon:

    ```html
    {% extends 'base.html' %}

    {% block content %}

    <h2>New Post</h2>
    <form method="post" novalidate>
        {% csrf_token %}
        {{ form.as_p }}
        <input type="submit" value="Save" />
    </form>

    {% endblock content %}
    ```

- U `post_form.html`, dodajte `csrf_token` tag i prikažite obrazac koristeći `form.as_p` svojstvo. Prikazaće sledeći HTML:

```html
<p>
    <label for="id_title"> Title:</label>
    <input type="text" name="title" maxlength="120" required id="id_title">
</p>
<p>
    <label for="id_content">Content:</label>
    <textarea name="content" cols="40" rows="10" required id="id_content"></textarea>
</p>
```

Ako otvorite URL adresu <http://127.0.0.1:8000/post/create>, videćete obrazac za kreiranje nove objave.

Ako kliknete na dugme `Sačuvaj`, videćete poruku o grešci. Pošto su polja za `naslov`, `sadržaj` i `autor` modela `Post` podrazumevano obavezna polja `PostForm`  objekat koji koristi `Post` model takođe prikazuje HTML obrazac koji zahteva ova polja.

Da biste testirali validaciju servera, možete onemogućiti validaciju klijenta dodavanjem `novalidate` svojstva u obrazac ovako:

```html
{% extends 'base.html' %}

{% block content %}

<h2>New Post</h2>
<form method="post" novalidate>
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit" value="Save" />
</form>

{% endblock content %}
```

Da biste obradili HTTP POST metodu, potrebno je da izmenite `create_post` funkciju u `views.py` aplikaciji `blog`:

```py
from django.shortcuts import render, redirect
from .models import Post
from .forms import PostForm

def create_post(request):
    if request.method == 'GET':
        context = {'form': PostForm()}
        return render(request, 'blog/post_form.html', context)
    elif request.method == 'POST':
        form = PostForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('posts')
        else:
            return render(request, 'blog/post_form.html', {'form': form})
# ...
```

Ako je HTTP zahtev POST ( request.method=='POST' ):

- Napravite novu instancu klase `PostForm` sa podacima iz POST-a.
- Proverite da li je obrazac važeći.
- Ako je obrazac važeći, sačuvajte vrednosti obrasca u bazu podataka i preusmerite veb pregledač na `'posts'` putanju.
- U suprotnom, ponovo prikažite obrazac sa starim vrednostima i greškama.

Ako pošaljete obrazac bez unosa bilo kakvih vrednosti, dobićete poruke o grešci.

Međutim, kada unesete vrednosti za neka obavezna polja, Django prikazuje obrazac sa starim vrednostima i prikazuje poruke o grešci za nevažeća polja.

Ako unesete validne vrednosti za sva polja, Django čuva vrednosti u bazi podataka, i preusmerite na listu objava `'posts'`.

### Rezime formi

- Napravite formu modela tako što ćete naslediti `ModelForm`.
- Dodajte `novalidate` svojstvo u formu da biste privremeno onemogućili HTML5 validaciju za testiranje validacije servera.
- Koristite `form.is_valid()` da proverite da li je forma validna.
- Koristite `form.save()` za čuvanje vrednosti forme u bazi podataka.
- Koristite `redirect()` za preusmeravanje na putanju.

[Sadržaj](#sadržaj)

## Fleš poruke

U ovom tutorijalu ćete naučiti kako da

- koristite Django fleš poruke,
- uključujući kreiranje fleš poruka i
- njihovo prikazivanje.

`Fleš poruka` je jednokratna poruka obaveštenja. Da biste prikazali fleš poruku u Django-u, koristite `messages` iz `django.contrib` modula:

```py
from django.contrib import messages
```

Poruke imaju neke korisne funkcije za prikazivanje informacija, upozorenja i poruka o greškama:

- `messages.debug` – prikazuje poruku za otklanjanje grešaka.
- `messages.info` – prikazuje informativnu poruku.
- `messages.success` – prikazuje poruku o uspehu.
- `messages.warning` – prikazuje poruku upozorenja.
- `messages.error` – prikazuje poruku o grešci.

Sve ove funkcije prihvataju `HttpRequest` objekat kao prvi argument i poruku kao drugi argument.

### Primer Flash poruke

Implementiraćemo fleš poruke za `blog` aplikaciju u `django_project`.

### Kreiranje fleš poruka

Izmenite `create_post()` funkciju dodavanjem fleš poruka:

```py
from django.shortcuts import render,redirect
from django.contrib import messages
from .models import Post
from .forms import PostForm

def create_post(request):
    if request.method == 'GET':
        context = {'form': PostForm()}
        return render(request,'blog/post_form.html',context)
    elif request.method == 'POST':
        form = PostForm(request.POST)
        if form.is_valid():
            form.save()
            messages.success(request, 'The post has been created successfully.')
            return redirect('posts')
        else:
            messages.error(request, 'Please correct the following errors:')
            return render(request,'blog/post_form.html',{'form':form})
```

Kako to funkcioniše.

- Uvezite poruke iz django.contrib:

    ```py
    from django.contrib import messages
    ```

- Kreirajte poruku o uspehu nakon čuvanja vrednosti forme u bazu podataka pozivanjem `success()` funkcije:

    ```py
    messages.success(request, 'The post has been created successfully.')
    ```

- Kreirajte poruku o grešci ako forma nije validan pozivanjem `error()` funkcije:

    ```py
    messages.error(request, 'Please correct the following errors:')
    ```

### Prikazivanje fleš poruke

Izmenite `base.html` šablon da bi prikazivao fleš poruke:

```html
{%load static %}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="{% static 'css/style.css' %}" />
    <script src="{% static 'js/app.js' %}" defer></script>
    <title>My Site</title>
  </head>
  <body>
    {% if messages %}
      <div class="messages">
        {% for message in messages %}
          <div class="alert {% if message.tags %} alert-{{ message.tags }}"
            {{ message }}
          </div>
        {% endfor %}
      </div>
    {% endif %}
    
    {%block content%} 
    {%endblock content%}
  </body>
</html>
```

Ako odete na obrazac <http://127.0.0.1/post/create>, unesite vrednosti i kliknite na dugme `Sačuvaj` videćete poruku o uspehu.

Ako osvežite stranicu, poruka će nestati jer se prikazuje samo jednom.

Kada preskočite unos vrednosti za `title` polje i kliknete na dugme `Sačuvaj`,
videćete poruku o grešci.

### Rezime fleš poruka

- Koristite `messages` from `django.contrib` za kreiranje i prikazivanje poruka.

[Sadržaj](#sadržaj)

## Edit forme

U ovom tutorijalu ćete naučiti kako da

- kreirate Django formu za uređivanje kako biste ažurirali objavu i sačuvali izmene u bazi podataka.

Napravićemo Django `Edit Form` koji ažurira objavu na blogu i čuva izmene u bazi podataka.

### Napravite URL obrazac

Prvo, kreirajte URL obrazac za uređivanje objave:

```py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='posts'),
    path('post/create', views.create_post, name='post-create'),
    path('post/edit/<int:id>/', views.edit_post, name='post-edit'),
    path('about/', views.about, name='about'),
]
```

URL adresa za uređivanje objave prihvata `ID` kao ceo broj određen šablonom `<int:id>`. Na primer, ako uredite objavu sa ID-om 1, URL adresa će biti:

```shell
<http://127.0.0.1/post/update/1/>
```

Django će proslediti `id` (1) drugom argumentu funkcije `edit_post()`.

Ako prosledite vrednost koja nije ceo broj u URL adresu ovako:

```shell
<http://127.0.0.1/post/update/abcd/>
```

Django će preusmeriti na grešku `404` jer se ne podudara ni sa jednim URL-om u URL obrascima.

### Definišite funkciju prikaza

Definišite `edit_post()` funkciju u `views.py` datoteci:

```py
from django.shortcuts import render,redirect, get_object_or_404
from django.contrib import messages
from .models import Post
from .forms import PostForm

def edit_post(request, id):
    post = get_object_or_404(Post, id=id)

    if request.method == 'GET':
        context = {'form': PostForm(instance=post), 'id': id}
        return render(request,'blog/post_form.html', context)

# other functions
```

Kako kod funkcioniše:

- Uvezite `get_object_or_404` funkciju iz `django.shortcuts` modula:

    ```py
    from django.shortcuts import render,redirect, get_object_or_404
    ```

    Funkcija `get_object_or_404()` dobija objekat po `ID` ili preusmerava na `404` stranicu ako `ID` ne postoji.

- Definišite `edit_post()` funkciju koja prihvata `HttpRequest` objekat  `request` i `id` kao ceo broj.

    Funkcija `edit_post()` izvršava sledeće korake:
        - Dobija `Post` objekat po ID-u ili preusmerava na stranicu `404` ako `ID` ne postoji.
        - Kreira `PostForm` objekat i postavlja instancu argument objekta `post`.
        - Renderuje `post_form.html` šablon.

- Izmenite `post_form.html` šablon da biste promenili zaglavlje obrasca. Trenutno prikazuje `Create Post`.

    ```html
    {% extends 'base.html' %}

    {% block content %}

    <h2>{% if id %} Edit {% else %} New {% endif %} Post</h2>
    <form method="post" novalidate>
        {% csrf_token %}
        {{ form.as_p }}
        <input type="submit" value="Save" />
    </form>
    
    {% endblock content %}
    ```

    Ako je `id` (id objave) dostupan, onda je forma u režimu uređivanja. U suprotnom, nalazi se u režimu kreiranja. Na osnovu ove logike, menjamo zaglavlje obrasca u skladu sa tim.

- Izmenite `home.html` šablon tako da uključite vezu za uređivanje u svakoj objavi:

    ```html
    {% extends 'base.html' %}
        
    {% block content %}
    <h1>My Posts</h1>
        {% for post in posts %}
            <h2>{{ post.title }}</h2>
            <small>Published on {{ post.published_at | date:"M d, Y" }} by {{ post.author | title}}</small>
            <p>{{ post.content }}</p>
            <p><a href="{% url 'post-edit' post.id %}">Edit</a></p>
        {% endfor %}
    {% endblock content %}
    ```

- Otvorite URL liste objava <http://127.0.0.1/>, videćete listu objava sa linkom za uređivanje na svakoj kao što je prikazano.

    Ako kliknete na vezu `"Uredi"` da biste izmenili objavu, videćete formu popunjen vrednostima polja. Na primer, možete da izmenite objavu "Ravno je bolje od ugnežđenog".

    Da biste izmenili objavu, promenite vrednosti i kliknite na dugme `Sačuvaj`. Međutim, još uvek nismo dodali kod koji obrađuje HTTP POST zahtev.

- Dodajte kod koji obrađuje HTTP POST zahtev, tj. kada se klikne na dugme `"Sačuvaj"`:

    ```py
    def edit_post(request, id):
        post = get_object_or_404(Post, id=id)

        if request.method == 'GET':
            context = {'form': PostForm(instance=post), 'id': id}
            return render(request,'blog/post_form.html',context)
        
        elif request.method == 'POST':
            form = PostForm(request.POST, instance=post)
            if form.is_valid():
                form.save()
                messages.success(request, 'The post has been updated successfully.')
                return redirect('posts')
            else:
                messages.error(request, 'Please correct the following errors:')
                return render(request,'blog/post_form.html',{'form':form})
    ```

    Ažurirajte objavu dodavanjem tri zvezdice (***) naslovu:

    Kliknite na dugme `"Sačuvaj"` i videćete da je objava ažurirana.

### Rezime edit formi

- Uključite `<int:id>` šablon u URL da biste kreirali URL za uređivanje koji prihvata `ID` modela kao ceo broj.
- Koristite `get_object_or_404()` funkciju da biste dobili objekat po `ID`-u ili preusmerili na stranicu `404` ako objekat ne postoji.
- Prosledite instancu ModelaForm da biste prikazali polja modela.

[Sadržaj](#sadržaj)

## Forma za brisanje

U ovom tutorijalu ćete naučiti kako da

- kreirate Django formu za brisanje objave.

Napravićemo obrazac koji briše objavu po njenom ID-u.

### Kreiranje URL-a forme

Dodajte URL šablon na listu šablona u `urls.py` aplikacije blog:

```py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='posts'),
    path('post/create', views.create_post, name='post-create'),
    path('post/edit/<int:id>/', views.edit_post, name='post-edit'),
    path('post/delete/<int:id>/', views.delete_post, name='post-delete'),
    path('about/', views.about, name='about'),
]
```

URL za brisanje prihvata ID kao ceo broj koji određuje ID objave koju treba obrisati. Kada otvorite URL:

```shell
<http://127.0.0.1/post/delete/1/Y>
```

Django će izvršiti `delete_post()` funkciju u views.py.

### Definisanje funkcije prikaza

Definišite `delete_post()` funkciju u `views.py` aplikaciji `blog`:

```py
from django.shortcuts import render,redirect, get_object_or_404
from django.contrib import messages
from .models import Post
from .forms import PostForm

def delete_post(request, id):
    post = get_object_or_404(Post, pk=id)
    context = {'post': post}    
    
    if request.method == 'GET':
        return render(request, 'blog/post_confirm_delete.html',context)
    elif request.method == 'POST':
        post.delete()
        messages.success(request,  'The post has been deleted successfully.')
        return redirect('posts')

# ...
```

Kako to funkcioniše.

- Preuzmite objavu po ID-u koristeći `get_object_or_404()` i prikažite `post_confirm_delete.html` šablon. Ako objava ne postoji, onda se preusmerava na 404 stranicu.
- Prikažite `post_confirm_delete.html` šablon ako je HTTP zahtev GET.
- Obrišite objavu, kreirajte fleš poruku i preusmerite na listu objava ako je HTTP zahtev POST.

### Kreiranje šablona

Napravite `post_confirm_delete.html` šablon u `templates/blog` direktorijumu aplikacije `blog`. Ovaj šablon proširuje `base.html` šablon projekta:

```html
{% extends 'base.html' %} 

{% block content %}
<h2>Delete Post</h2>

<form method="POST">
  {% csrf_token %}
  <p>Are you sure that you want to delete the post "{{post.title}}"?</p>
  <div>
    <button type="submit">Yes, Delete</button>
    <a href="{% url 'posts' %}">Cancel</a>
  </div>
</form>

{% endblock content %}
```

Ovaj šablon sadrži obrazac koji ima dva dugmeta ( `Yes`, `Delete` ). Ako kliknete na dugme za slanje, poslaće se HTTP POST zahtev na navedenu URL adresu. U suprotnom, preusmeriće vas na URL adresu liste objava.

### Dodavanje linka za brisanje u objavu

Dodajte vezu za brisanje svakoj objavi u `home.html` šablonu:

```html
{% extends 'base.html' %}

{% block content %}
<h1>My Posts</h1>
    {% for post in posts %}
        <h2>{{ post.title }}</h2>
        <small>Published on {{ post.published_at | date:"M d, Y" }} by {{ post.author | title}}</small>
        <p>{{ post.content }}</p>
        <p>
            <a href="{% url 'post-edit' post.id %}">Edit</a> 
            <a href="{% url 'post-delete' post.id%}">Delete</a>
        </p>
    {% endfor %}
{% endblock content %}
```

Ako otvorite URL adresu <http://127.0.0.1/>, videćete vezu za brisanje koja se pojavljuje pored veze za uređivanje.

Ako kliknete na vezu za brisanje, bićete preusmereni na URL adresu za brisanje.
Kada kliknete na `Yes, Delete` dugme, Django će izvršiti `delete_post()` funkciju koja briše objavu i preusmerava vas na listu objava.

### Rezime formi za brisanje

- Koristite `delete()` metodu za brisanje modela iz baze podataka

[Sadržaj](#sadržaj)

## Forma za prijavu

U ovom tutorijalu ćete naučiti kako da kreirate Django formu za prijavu koji omogućava korisnicima da se

- prijave pomoću korisničkog imena i lozinke.

### Napravite novu aplikaciju

- kreirajte novu aplikaciju koja se zove `users` izvršavanjem `startapp` komande:

    ```shell
    django-admin startapp users
    ```

    Direktorijum projekta će izgledati ovako:

    ```shell
    ├── blog
    ├── db.sqlite3
    ├── manage.py
    ├── mysite
    ├── static
    ├── templates
    └── users
    ```

- Registrujte `users` aplikaciju u instaliranim aplikacijama projekta settings.py:

    ```py
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        # local
        'blog.apps.BlogConfig',
        'users.apps.UsersConfig'
    ]
    ```

- Kreirajte novu `urls.py` datoteku unutar `users` aplikacije sa sledećim kodom:

    ```py
    from django.urls import path
    from . import views

    urlpatterns = []
    ```

   Konačno, uključite `urls.py` aplikacije `users` u `urls.py` projekta koristeći `include()` funkciju:

    ```py
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('',include('blog.urls')),
        path('',include('users.urls'))
    ]
    ```

### Napravite formu za prijavu

- Kreirajte URL adresu za prijavu u `urls.py` aplikacije users:

    ```py
    from django.urls import path
    from . import views

    urlpatterns = [
        path('login/', views.sign_in, name='login'),
    ]
    ```

- Kreirajte `forms.py` datoteku u `users` aplikaciji i definišite `LoginForm` koja nasleđuje od `Form` klase:

    ```py
    from django import forms

    class LoginForm(forms.Form):
        username = forms.CharField(max_length=65)
        password = forms.CharField(max_length=65, widget=forms.PasswordInput)
    ```

    `LoginForm` ima dva polja, za korisničko ime i lozinku.

- Kreirajte `sign_in()` funkciju u `views.py` datoteci `users` aplikacije za prikazivanje `login.html` šablona:

    ```py
    from django.shortcuts import render
    from .forms import LoginForm

    def sign_in(request):
        if request.method == 'GET':
            form = LoginForm()
            return render(request, 'users/login.html', {'form': form})
    ```

- Kreirajte `templates/users` direktorijum unutar `users` aplikacije:

    ```shell
    mkdir templates
    cd templates
    mkdir users
    ```

- Kreirajte `login.html` šablon unutar `templates/users` direktorijuma koji proširuje `base.html` šablon:

    ```html
    {% extends 'base.html' %}

    {% block content %}

    <form method="POST" novalidate>
        {% csrf_token %}
        <h2>Login</h2>
        {{form.as_p}}
        <input type="submit" value="Login" />
    </form>

    {% endblock content%}
    ```

- Otvorite URL za prijavu i videćete formu za prijavu:

    ```shell
    http://127.0.0.1:8000/login
    ```

    Ako unesete `korisničko ime`/`lozinku` i kliknete na dugme `"Prijava"`, dobićete grešku jer još nismo dodali kod koji obrađuje HTTP POST zahtev.
    </br></br>

- Izmenite `sign_in()` funkciju da bi obrađivala proces prijavljivanja:

    ```py
    from django.shortcuts import render, redirect
    from django.contrib import messages
    from django.contrib.auth import login, authenticate
    from .forms import LoginForm

    def sign_in(request):

        if request.method == 'GET':
            form = LoginForm()
            return render(request,'users/login.html', {'form': form})
        
        elif request.method == 'POST':
            form = LoginForm(request.POST)
            
            if form.is_valid():
                username = form.cleaned_data['username']
                password = form.cleaned_data['password']
                user = authenticate(request,username=username,password=password)
                if user:
                    login(request, user)
                    messages.success(request,f'Hi {username.title()}, welcome back!')
                    return redirect('posts')
            
            # form is not valid or user is not authenticated
            messages.error(request,f'Invalid username or password')
            return render(request,'users/login.html',{'form': form})
    ```

Kako to funkcioniše.

- Uvezite funkciju `authenticate` i `login` iz `django.contrib.auth` modula:

    ```py
    from django.contrib.auth import login, authenticate
    ```

    Funkcija `authenticate()` proverava korisničko ime i lozinku. Ako su korisničko ime i lozinka važeći, vraća instancu `User` klase ili `None`.

    Funkcija `login()` prijavljuje korisnika. Tehnički, ona kreira `ID` sesije na serveru i šalje ga nazad veb pregledaču u obliku kolačića.

    U sledećem zahtevu, veb pregledač šalje `ID` sesije nazad veb serveru, Django upoređuje vrednost kolačića sa `ID`-om sesije i kreira `User` objekat.

- Proverite korisničko ime i lozinku koristeći authenticate()funkciju ako je obrazac važeći:

    ```py
    user = authenticate(request, username=username, password=password)
    ```

- Prijavite korisnika, kreirajte fleš poruku i preusmerite korisnika na postsURL ako su korisničko ime i lozinka važeći:

    ```py
    if user:
        login(request, user)
        messages.success(request,f'Hi {user.username.title()}, welcome back!')
        return redirect('posts')
    ```

- U suprotnom, kreirajte fleš poruku o grešci i preusmerite korisnika nazad na stranicu za prijavu:

    ```py
    messages.error(request,f'Invalid username or password')
    return render(request,'users/login.html')
    ````

- Ako unesete korisničko ime bez lozinke i kliknete na dugme `Prijava`, dobićete poruku o grešci. Međutim, ako unesete ispravno korisničko ime/lozinku, bićete preusmereni na stranicu sa listom objava sa porukom dobrodošlice:

### Dodajte formu za odjavu

- Definišite rutu za odjavljivanje korisnika:

    ```py
    from django.urls import path
    from . import views

    urlpatterns = [
        path('login/', views.sign_in, name='login'),
        path('logout/', views.sign_out, name='logout'),
    ]

- Definišite `sign_out()` funkciju u `views.py` datoteci koja obrađuje rutu za odjavu:

    ```py
    from django.shortcuts import render, redirect
    from django.contrib import messages
    from django.contrib.auth import login, authenticate, logout
    from .forms import LoginForm

    def sign_in(request):

        if request.method == 'GET':
            form = LoginForm()
            return render(request,'users/login.html', {'form': form})
        
        elif request.method == 'POST':
            form = LoginForm(request.POST)
            
            if form.is_valid():
                username = form.cleaned_data['username']
                password=form.cleaned_data['password']
                user = authenticate(request,username=username,password=password)
                if user:
                    login(request, user)
                    messages.success(request,f'Hi {username.title()}, welcome back!')
                    return redirect('posts')
            
            # either form not valid or user is not authenticated
            messages.error(request,f'Invalid username or password')
            return render(request,'users/login.html',{'form': form})
            
    def sign_out(request):
        logout(request)
        messages.success(request,f'You have been logged out.')
        return redirect('login')
    ```

    Ako se prijavite i pristupite stranici za prijavu, i dalje ćete videti obrazac za prijavu. Stoga je bolje preusmeriti prijavljenog korisnika na listu postova ako korisnik pristupi stranici za prijavu.

- Izmenite `sign_in()` funkciju u `views.py` aplikacije `users`:

    ```py
    def sign_in(request):

        if request.method == 'GET':
            if request.user.is_authenticated:
                return redirect('posts')
            
            form = LoginForm()
            return render(request,'users/login.html', {'form': form})
        
        elif request.method == 'POST':
            form = LoginForm(request.POST)
            
            if form.is_valid():
                username = form.cleaned_data['username']
                password=form.cleaned_data['password']
                user = authenticate(request,username=username,password=password)
                if user:
                    login(request, user)
                    messages.success(request,f'Hi {username.title()}, welcome back!')
                    return redirect('posts')
            
            # either form not valid or user is not authenticated
            messages.error(request,f'Invalid username or password')
            return render(request,'users/login.html',{'form': form})
    ```

    `request.user.is_authenticated` vraća `True` ako je korisnik prijavljen ili `False` ako nije.

- Izmenite `base.html` šablon tako da uključuje vezu za odjavu ako je korisnik autentifikovan, a vezu za prijavu u suprotnom:

    ```html
    {%load static %}
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <link rel="stylesheet" href="{% static 'css/style.css' %}" />
        <script src="{% static 'js/app.js' %}" defer></script>
        <title>My Site</title>
    </head>
    <body>
        <header>
            {%if request.user.is_authenticated %}
                <span>Hi {{ request.user.username | title }}</span>
                <a href="{% url 'logout' %}">Logout</a>
            {%else%}
                <a href="{% url 'login' %}">Login</a>
            {%endif%}
        </header>
        <main>
            {% if messages %}
                <div class="messages">
                    {% for message in messages %}
                        <div class="alert {% if message.tags %}alert-{{ message.      
                            tags }}"
                        {{ message }}
                        </div>
                    {% endfor %}
                </div>
            {% endif %}

            {%block content%} 
            {%endblock content%}
        </main>

    </body>
    </html>
    ```

    Ako pristupite sajtu, videćete link za prijavu.

    Kada kliknete na link za prijavu, otvoriće se stranica za prijavu.

    Ako unesete važeće korisničko ime i lozinku i prijavite se, videćete poruku dobrodošlice kao i link za odjavu.

    Ako kliknete na link za odjavu, bićete preusmereni na stranicu za prijavu.

### Sakrivanje linkova za uređivanje i brisanje na listi

Ako je korisnik prijavljen, `request.user.is_authenticated` je `True`. Stoga, možete koristiti ovaj objekat da biste prikazali i sakrili elemente stranice, bez obzira da li je korisnik prijavljen ili ne.

Na primer, sledeće skriva veze za uređivanje i brisanje na `blog/home.html` šablonu ako je korisnik autentifikovan:

```html
{% extends 'base.html' %}

{% block content %}
<h1>My Posts</h1>

    {% for post in posts %}
        <h2>{{ post.title }}</h2>
        <small>Published on {{ post.published_at | date:"M d, Y" }} by {{ post.author | title}}</small>
        <p>{{ post.content }}</p>

        {% if request.user.is_authenticated %}
        <p>
            <a href="{% url 'post-edit' post.id %}">Edit</a> 
            <a href="{% url 'post-delete' post.id%}">Delete</a>
        </p>
        {% endif %}

    {% endfor %}
{% endblock content %}
```

### Zaštita stranica od neautorizovanog pristupa

Obično bi trebalo da dozvolite autentifikovanim korisnicima pristup stranicama za kreiranje, ažuriranje i brisanje objava. Da biste to uradili, možete koristiti Django-ov `login_required` dekorator.

Ako funkcija prikaza ima `login_required` dekorator i neautentifikovani korisnik pokuša da je pokrene, Django će preusmeriti korisnika na stranicu za prijavu.

Zaštitićemo funkcije kreiranja, ažuriranja i brisanja objava koristeći `login_required` dekorator.

- Podesite URL za prijavu u `settings.py` na URL za prijavu:

    ```py
    LOGIN_URL = 'login'
    ```

    Ako ovo ne uradite, Django će preusmeriti na podrazumevani URL za prijavu koji nije users/login kakav smo definisali u ovom projektu.

- Izmenite `views.py` aplikacije `blog` dodavanjem `@login_required` dekoratora funkcijama `create_post`, `edit_post` i `delete_post`:

    ```py
    from django.shortcuts import render, redirect, get_object_or_404
    from django.contrib import messages
    from django.contrib.auth.decorators import login_required
    from .models import Post
    from .forms import PostForm

    @login_required
    def delete_post(request, id):
        post = get_object_or_404(Post, pk=id)
        context = {'post': post}

        if request.method == 'GET':
            return render(request, 'blog/post_confirm_delete.html', context)
        elif request.method == 'POST':
            post.delete()
            messages.success(request,  'The post has been deleted successfully.')
            return redirect('posts')

    @login_required
    def edit_post(request, id):
        post = get_object_or_404(Post, id=id)

        if request.method == 'GET':
            context = {'form': PostForm(instance=post), 'id': id}
            return render(request, 'blog/post_form.html', context)

        elif request.method == 'POST':
            form = PostForm(request.POST, instance=post)
            if form.is_valid():
                form.save()
                messages.success(
                    request, 'The post has been updated successfully.')
                return redirect('posts')
            else:
                messages.error(request, 'Please correct the following errors:')
                return render(request, 'blog/post_form.html', {'form': form})

    @login_required
    def create_post(request):
        if request.method == 'GET':
            context = {'form': PostForm()}
            return render(request, 'blog/post_form.html', context)
        elif request.method == 'POST':
            form = PostForm(request.POST)
            if form.is_valid():
                form.save()
                messages.success(
                    request, 'The post has been created successfully.')
                return redirect('posts')
            else:
                messages.error(request, 'Please correct the following errors:')
                return render(request, 'blog/post_form.html', {'form': form})

    def home(request):
        posts = Post.objects.all()
        context = {'posts': posts}
        return render(request, 'blog/home.html', context)

    def about(request):
        return render(request, 'blog/about.html')
    ```

    Ako otvorite URL za kreiranje, ažuriranje ili brisanje, na primer:

    ```html
    http://127.0.0.1/post/create
    ```

    bićete preusmereni na stranicu za prijavu.

### Rezime forme za prijavu

- Koristite `authenticate()` funkciju za verifikaciju korisnika pomoću korisničkog imena i lozinke.
- Koristite `login()` funkciju za prijavljivanje korisnika.
- Koristite `logout()` funkciju za odjavu korisnika.
- Koristite `request.user.is_authenticated` za proveru da li je trenutni korisnik autentifikovan.
- Dekorišite stranice sa `@login_required` za zaštitu od neautentifikovanih korisnika.

[Sadržaj](#sadržaj)

## Registraciona forma

U ovom tutorijalu ćete naučiti kako da:

- kreirate Django formu za registraciju koji omogućava korisnicima da se registruju.

### Kreiranje registracione forme

- Definišite URL za registraciju u `urls.py` aplikacije `users`:

    ```py
    from django.urls import path
    from . import views

    urlpatterns = [
        path('login/', views.sign_in, name='login'),
        path('logout/', views.sign_out, name='logout'),
        path('register/', views.sign_up, name='register'),
    ]
    ```

- Definišite `RegisterForm` klasu u `forms.py` datoteci aplikacije `users`:

    ```py
    from django import forms
    from django.contrib.auth.models import User
    from django.contrib.auth.forms import UserCreationForm

    class LoginForm(forms.Form):
        username = forms.CharField(max_length=65)
        password = forms.CharField(max_length=65, widget=forms.PasswordInput)

    class RegisterForm(UserCreationForm):
        class Meta:
            model=User
            fields = ['username','email','password1','password2'] 
    ```

    RegisterForm koristi ugrađeni `User` model i uključuje četiri polja, uključujući `username`, `email,`password1` i `password2`, koja korisnik treba da popuni za registraciju.

- Definišite `sign_up()` funkciju prikaza u `views.py` aplikaciji `users`:

    ```py
    from django.shortcuts import render, redirect
    from django.contrib import messages
    from django.contrib.auth import login, authenticate, logout
    from .forms import LoginForm, RegisterForm

    def sign_up(request):
        if request.method == 'GET':
            form = RegisterForm()
            return render(request, 'users/register.html', { 'form': form})   
    ```

    Funkcija `sign_up()` prikaza kreira RegisterForm objekat i prikazuje ga u `register.html` šablonu.

- Kreirajte `register.html` šablon u `templates/users` direktorijumu aplikacije `users`:

    ```html
    {% extends 'base.html' %}

    {% block content %}

    <form method="POST" novalidate>
        {% csrf_token %}
        <h2>Sign Up</h2>
        {{ form.as_p }}  
        <input type="submit" value="Register" />
    </form>

    {% endblock content%}
    ```

    `registerl.html` proširuje šablon `base.html` projekta. Prikazuje `RegisterForm`.

    Imajte na umu da `novalidate` svojstvo uklanja HTML5 validaciju. Nakon završetka projekta, možete ukloniti ovo svojstvo da biste omogućili HTML5 validaciju.

- Otvorite URL adresu za registraciju:

    ```shell
    http://127.0.0.1:8000/register/
    ```

    ... videćete formular za registraciju na sledeći.

### Prilagodite formu za registraciju

Forma za registraciju ima četiri polja sa puno informacija. Ove informacije potiču iz podrazumevanog `User` modela.

Ako želite da prilagodite informacije prikazane na obrascu, možete izmeniti `register.html` šablon na sledeći način:

```html
{% extends 'base.html' %}

{% block content %}

<form method="POST" novalidate>
    {% csrf_token %}
    <h2>Sign Up</h2>

    {% for field in form %}
    <p>
        {% if field.errors %}
        <ul class="errorlist">
            {% for error in field.errors %}
                <li>{{ error }}</li>
            {% endfor %}
        </ul>
        {% endif %}
        {{ field.label_tag }} {{ field }}
    </p>
    {% endfor %}
    <input type="submit" value="Register" />
</form>

{% endblock content%}
```

Šablon iterira kroz polja obrasca i prikazuje svako polje pojedinačno. Za svako polje prikazuje listu grešaka ako validacija ne uspe.

Ako popunite informacije i kliknete na registraciju, dobićete grešku jer nismo dodali kod koji obrađuje HTTP POST zahtev.

### Logika rukovanja registracijom

Da biste obradili HTTP POST zahtev, modifikujete `sign_up()` funkciju u `views.py` datoteci aplikacije `users`:

```py
def sign_up(request):
    if request.method == 'GET':
        form = RegisterForm()
        return render(request, 'users/register.html', {'form': form})

    if request.method == 'POST':
        form = RegisterForm(request.POST) 
        if form.is_valid():
            user = form.save(commit=False)
            user.username = user.username.lower()
            user.save()
            messages.success(request, 'You have singed up successfully.')
            login(request, user)
            return redirect('posts')
        else:
            return render(request, 'users/register.html', {'form': form})
```

Kako ovo funkcioniše:

- Kreirate novu instancu `RegisterForm`,

    ```py
    form = RegisterForm(request.POST)
    ```

- Ako je obrazac validan, čuvamo ga, ali ga ne čuvamo odmah u bazi podataka. To se radi prosleđivanjem argumenta `commit=False` metodi `save()` objekta Forme.

    Razlog je taj što želimo da korisničko ime bude napisano malim slovima pre nego što ga sačuvamo u bazi podataka:

    ```py
    user.username = user.username.lower()
    user.save()
    ```

    Nakon što sačuvamo korisnika, kreiramo fleš poruku, prijavljujemo korisnika i preusmeravamo ga na stranicu sa listom objava:

    ```py
    messages.success(request, 'You have singed up successfully.')
    login(request, user)
    return redirect('posts')
    ```

- Ako obrazac nije validan, ponovo ga renderujemo sa prethodno unetim vrednostima tako što funkciji prosleđujemo objekat obrasca render():

    ```py
    return render(request, 'users/register.html', {'form': form})
    ```

### Dodavanje linkova

- Uključite link za registraciju izmenom `base.html` šablona. Takođe, uključite linkove `My Posts` i `New Post` ako je korisnik prijavljen:

    ```html
    {%load static %}
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <link rel="stylesheet" href="{% static 'css/style.css' %}" />
        <script src="{% static 'js/app.js' %}" defer></script>
        <title>My Site</title>
    </head>
    <body>
    <header>
        {%if request.user.is_authenticated %}
        <a href="{% url 'posts' %}">My Posts</a>
        <a href="{% url 'post-create' %}">New Post</a>
        <span>Hi {{ request.user.username | title }}</span>
        <a href="{% url 'logout' %}">Logout</a>
        {%else%}
        <a href="{% url 'login' %}">Login</a>
        <a href="{% url 'register' %}">Register</a>
        {%endif%}
    </header>
    <main>
        {% if messages %}
    <div class="messages">
    {% for message in messages %}
        <div class="alert {% if message.tags %}alert-{{ message.tags }}"{% endif %}>
        {{ message }}
        </div>
    {% endfor %}
    </div>
    {% endif %}
        
        {%block content%} 
        {%endblock content%}
    </main>
    
    </body>
    </html>
    ```

- Uključite link za registraciju u `login.html` šablon:

    ```html
    {% extends 'base.html' %}

    {% block content %}

    <form method="POST" novalidate>
    {% csrf_token %}
    <h2>Login</h2>
    {{form.as_p}}
    <input type="submit" value="Login" />
    <p>Don't have an account? <a href="{%url 'register' %}">Register</a></p>
    </form>

    {% endblock content%}
    ```

- Dodajte link za prijavu na `register.html` stranicu:

    ```html
    {% extends 'base.html' %}

    {% block content %}
    <form method="POST" novalidate>
    {% csrf_token %}
    <h2>Sign Up</h2>
    
    {% for field in form %}
    <p>
    {% if field.errors %}
    <ul class="errorlist">
    {% for error in field.errors %}
    <li>{{ error }}</li>
    {% endfor %}
    </ul>
    {% endif %}
    {{ field.label_tag }} {{ field }}
    </p>
    {% endfor %}
    <input type="submit" value="Register" />
    <p>Already has an account? <a href="{%url 'login' %}">Login</a></p>
    </form>
    {% endblock content%}
    ```

### Sprečavanje korisnika da uređuje/briše objave drugih korisnika

Obično korisnik ne bi trebalo da bude u mogućnosti da uređuje ili briše objave drugih korisnika. Međutim, korisnik može da vidi objave drugih korisnika.

Da bismo implementirali ovu funkciju, potrebno je da proverimo da li je autor objave isti kao i trenutno prijavljeni korisnik. Ako jeste, prikazujemo forme za izmene/brisanje. U suprotnom, možemo preusmeriti korisnike na stranicu 404.

Takođe, potrebno je da sakrijemo linkove za uređivanje i brisanje na stranici sa listom objava ako objave ne pripadaju korisniku.

- Izmenite `views.py` aplikaciju blog:

    ```py
    from django.shortcuts import render,redirect, get_object_or_404
    from django.contrib import messages
    from django.contrib.auth.decorators import  login_required
    from .models import Post
    from .forms import PostForm

    @login_required
    def delete_post(request, id):
        queryset = Post.objects.filter(author=request.user)
        post = get_object_or_404(queryset, pk=id)
        context = {'post': post}

        if request.method == 'GET':
            return render(request, 'blog/post_confirm_delete.html',context)
        elif request.method == 'POST':
            post.delete()
            messages.success(request,  'The post has been deleted successfully.')
            return redirect('posts')        

    @login_required
    def edit_post(request, id):
        queryset = Post.objects.filter(author=request.user)
        post = get_object_or_404(queryset, pk=id)

        if request.method == 'GET':
            context = {'form': PostForm(instance=post), 'id': id}
            return render(request,'blog/post_form.html',context)
        
        elif request.method == 'POST':
            form = PostForm(request.POST, instance=post)
            if form.is_valid():
                form.save()
                messages.success(request, 'The post has been updated successfully.')
                return redirect('posts')
            else:
                messages.error(request, 'Please correct the following errors:')
                return render(request,'blog/post_form.html',{'form':form})

    @login_required
    def create_post(request):
        if request.method == 'GET':
            context = {'form': PostForm()}
            return render(request,'blog/post_form.html',context)
        elif request.method == 'POST':
            form = PostForm(request.POST)
            if form.is_valid():
                form.save()
                messages.success(request, 'The post has been created successfully.')
                return redirect('posts')
            else:
                messages.error(request, 'Please correct the following errors:')
                return render(request,'blog/post_form.html',{'form':form})

    def home(request):
        posts = Post.objects.all()
        context = {'posts': posts  }
        return render(request,'blog/home.html', context)
    ```

    I u brisanju i u ažuriranju, filtriramo objavu prema trenutnom korisniku pre nego što je prosledimo funkciji `get_object_or_404()`.

    Ako se prijavite kao Jana i pokušate da izmenite objavu koja ne pripada Jani, dobićete grešku `404`.

- Izmenite `home.html` šablon da biste sakrili veze za uređivanje i brisanje.

    ```html
    {% extends 'base.html' %}
    
    {% block content %}

    <h1>My Posts</h1>
    {% for post in posts %}
    <h2>{{ post.title }}</h2>
    <small>Published on {{ post.published_at | date:"M d, Y" }} by {{ post.author | title}}</small>
    <p>{{ post.content }}</p>
    
    {% if request.user.is_authenticated and request.user == post.author %}
    <p>
    <a href="{% url 'post-edit' post.id %}">Edit</a> 
    <a href="{% url 'post-delete' post.id%}">Delete</a>
    </p>
    {% endif %}
    
    {% endfor %}

    {% endblock content %}
    ```

### Uklanjanje autora iz obrasca za kreiranje objave

- Uklonite `author` polje iz `fields` klase `PostForm`:

    ```py
    from django.forms import ModelForm
    from .models import Post

    class PostForm(ModelForm):
        class Meta:
            model = Post
            fields = ['title','content']
    ```

    Obrazac menja svoj izgled.

- Izmenite `create_post()` funkciju u `views.py` aplikacije `blog` da biste ažurirali `autor`-a objave na trenutno prijavljenog korisnika:

    ```py
    @login_required
    def create_post(request):
        if request.method == 'GET':
            context = {'form': PostForm()}
            return render(request,'blog/post_form.html',context)
        elif request.method == 'POST':
            form = PostForm(request.POST)
            if form.is_valid():
                user = form.save(commit=False)
                user.author = request.user
                user.save()
                messages.success(request, 'The post has been created successfully.')
                return redirect('posts')
            else:
                messages.error(request, 'Please correct the following errors:')
                return render(request,'blog/post_form.html',{'form':form})  
    ```

### Rezime registracione forme

- Podklasirajte `UserCreationForm` da biste kreirali prilagođeni Django obrazac za registraciju.


# Prikazi zasnovani na klasama

U ovom odeljku ćete naučiti prikaze zasnovane na klasama tako što ćete napraviti aplikaciju za listu zadataka koja omogućava korisnicima da se registruju, prijavljuju, resetuju lozinke, kreiraju profile i upravljaju sopstvenim zadacima.

## Sadržaj

- [Todo aplikacija](#todo-aplikacija)  
  Kako da kreirate strukturu projekta Todo aplikacije od nule.

- [ListView](#listview)  
  Kako da koristite klasu ListView za prikazivanje liste zadataka.

- DetailView  
  Kako da koristite klasu DetailView za prikazivanje zadatka.

- CreateView  
  Kako da koristite klasu CreateView za kreiranje formulara koji kreira zadatak.

- UpdateView  
  Kako da koristite klasu UpdateView za kreiranje obrasca koji uređuje zadatak.

- DeleteView  
  Kako da koristite klase DeleteView za brisanje postojećeg zadatka.

- LoginView  
- Kako da koristite LoginView za kreiranje stranice za prijavu za aplikaciju Todo.

- FormView  
  Kako da koristite FormView za kreiranje stranice za registraciju koja omogućava korisnicima da se registruju.

- Resetovanje lozinke  
  kako da implementirate funkciju resetovanja lozinke za Django aplikaciju.

- Korisnički profil  
  Kako da implementirate funkcije korisničkog profila za aplikaciju Todo.

## Todo aplikacija

U ovom tutorijalu ćete naučiti kako da kreirate projekat Todo aplikacije, uključujući:

- Kreiranje virtuelno okruženje
- Instalirate Django paket
- Napravite novi projekat
- Dodate statičkih datoteka
- Podesite šablone
- Napravite aplikaciju sa obavezama
- Kreirate model zadatka i primenite migracije

### Kreiranje virtuelnog okruženja

Pokrenite sledeću komandu u shell-u da biste kreirali virtuelno okruženje koristeći ugrađeni `venv` modul:

```shell
python -m venv venv
```

Aktivirajte `venv` virtuelno okruženje pomoću sledeće komande:

```shell
venv\scripts\activate
```

Ako koristite macOS i Linux, možete koristiti python3 komandu da biste kreirali novo virtuelno okruženje:

```shell
python3 -m venv venv
```

I aktivirate je pomoću sledeće komande:

```shell
source venv/bin/activate
```

### Instaliranje Django paketa

Pošto je Django paket treće strane, potrebno ga je instalirati pomoću sledeće `pip` komande:

```shell
pip install django
```

### Kreiranje novog projekta

Da biste kreirali novi projekat `todo_list`, koristite `startproject` komandu:

```shell
django-admin startproject todo_list
```

### Dodavanje statičkih datoteka

Kreirajte `static` direktorijum unutar direktorijuma projekta:

```shell
mkdir static
```

Podesite `STATICFILES_DIRS` i `STATIC_URL` na `static` direktorijum u `settings.py` datoteci projekta kako bi Django mogao da pronađe statičke datoteke projekta:

```shell
STATIC_URL = 'static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
```

Kreirajte tri direktorijuma `js`, `css` i `images` unutar `static` direktorijuma:

```shell
cd static
mkdir css images js
```

Direktorijum `static` će izgledati ovako:

```shell
├── static
|  ├── css
|  ├── images
|  └── js
```

Na kraju, kopirajte `style.css` datoteku i `feature.jpg` sliku iz datoteke za preuzimanje u `css` i  `images` direktorijume.

### Podešavanje šablona

Kreirajte `templates` direktorijum unutar direktorijuma projekta:

```shell
mkdir templates
```

Kreirajte `base.html` šablon unutar `templates` direktorijuma sa sledećim sadržajem:

```html
{%load static %}
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="{% static 'css/style.css' %}" />
        <title>Todo List</title>
    </head>

    <body>
        <header class="header">
            <div class="container">
            </div>
        </header>
        <main>
            <div class="container">
            </div>
        </main>
        <footer class="footer">
            <div class="container">
               <p>&copy; Copyright {% now "Y" %} by <a href="https://www.pythontutorial.net">Python Tutorial</a></p>
            </div>
        </footer>
    </body>

</html>
```

Šablon `base.html` koristi `style.css` datoteku iz `static/css` direktorijuma.

Konfigurišite direktorijum `TEMPLATES` u datoteci `settings.py` na `templates` tako da Django može da pronađe `base.html` šablon.

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

Kreirajte `home.html` šablon unutar `templates` direktorijuma:

```html
{%extends 'base.html'%}
{%load static %}
{%block content%}
    <section class="feature">
        <div class="feature-content">
            <h1>Todo</h1>
            <p>Todo helps you more focus, either work or play.</p>
            <a class="btn btn-primary cta" href="#">Get Started</a>
        </div>
        <img src="{%static 'images/feature.jpg'%}" alt="" class="feature-image">
    </section>
{%endblock content%}
```

### Kreiranje aplikacije sa zadacima

Kreirajte `todo` aplikaciju u `todo_list` projektu koristeći `startapp` komandu:

```shell
django-admin startapp todo
```

Registrujte `todo` aplikaciju u `settings.py` projekta `todo_list` tako što ćete je dodati na `INSTALLED_APPS` listu:

```py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'todo',
]
```

Kreirajte `templates` direktorijum unutar `todo` direktorijuma aplikacije:

```shell
cd todo
mkdir templates
```

Kreirajte `todo` direktorijum unutar `templates` direktorijuma. Naziv direktorijuma mora biti isti kao i naziv aplikacije.

```shell
cd templates
mkdir todo
```

Definišite `home()` funkciju prikaza unutar `views.py` aplikacije zadataka koja prikazuje `home.html` šablon:

```py
from django.shortcuts import render

def home(request):
    return render(request,'home.html')
```

Kreirajte `urls.py` datoteku u `todo` aplikaciji i definišite rutu koja se mapira na početnu URL adresu pomoću `views.home()` funkcije prikaza:

```py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

Uključite `urls.py` aplikacije `todo` u `urls.py` deo projekta:

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('todo.urls'))
]
```

Pokrenite Django razvojni server iz `todo_list` direktorijuma:

```py
python manage.py runserver
```

Konačno, otvorite <http://127.0.0.1:8000/> u veb pregledaču, videćete stranicu bloga koja prikazuje početnu Home stranicu.

### Kreirajte model zadatka

Definišite `Task` model u `models.py` aplikacije `todo`:

```py
from django.db import models
from django.contrib.auth.models import User

class Task(models.Model):
    title = models.CharField(max_length=255)
    description = models.TextField(null=True, blank=True)
    completed = models.BooleanField(default=False)
    created_at = models.DateTimeField(auto_now_add=True)
    user = models.ForeignKey(User,on_delete=models.CASCADE, null=True, blank=True)
    
    def __str__(self):
        return self.title
    
    class Meta:
        ordering = ['completed']
```

Registrujte `Task` model u `admin.py` aplikaciji `todo` kako biste mogli da ga upravljate modelom na administratorskoj stranici.

```py
from django.contrib import admin
from .models import Task

admin.site.register(Task)
```

Izvršite migracije pokretanjem `makemigrations` komande:

```shell
python manage.py makemigrations

Migrations for 'todo':
  todo\migrations\0001_initial.py
    - Create model Task
```

Primenite migracije na bazu podataka:

```shell
python manage.py migrate

Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions, todo
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
  Applying sessions.0001_initial... OK
  Applying todo.0001_initial... OK
```

Kreirajte `superuser`-a izvršavanjem `createsuperuser``komande:

```shell
python manage.py createsuperuser

Username: john
Email address:
Password:
Password (again):
Superuser created successfully.
```

Ponovo pokrenite Django razvojni server:

```shell
python manage.py runserver
```

Prijavite se na administratorsku stranicu i kreirajte tri zadatka.

> [!Note]
>
> Konačni kod za ovaj Django ListView tutorijal možete preuzeti ovde.

[Sadržaj](#sadržaj)

## ListView

U ovom tutorijalu ćete naučiti kako da koristite Django `ListView` klasu za prikazivanje liste zadataka za aplikaciju `Todo` list.

U prethodnim tutorijalima ste naučili kako da napravite `blog` aplikaciju koristeći prikaze zasnovane na funkcijama.

Prikazi zasnovani na funkcijama su jednostavni i fleksibilni. U ranijim verzijama, Django je podržavao samo prikaze zasnovane na funkcijama. Kasnije je Django dodao podršku za prikaze zasnovane na klasama koji vam omogućavaju da definišete prikaze pomoću klasa.

Prikazi zasnovani na klasama su alternativni način implementacije prikaza. Oni ne zamenjuju prikaze zasnovane na funkcijama. Međutim, imaju neke prednosti u poređenju sa prikazima zasnovanim na funkcijama:

- Organizujte kod povezan sa HTTP metodama kao što su GET i POST koristeći odvojene metode, umesto uslovnog grananja u istoj funkciji.
- Iskoristite višestruko nasleđivanje da biste kreirali klase prikaza koje se mogu ponovo koristiti.

Koristićemo prikaze zasnovane na klasama da bismo izgradili `Todo` aplikaciju.

> [!Note]
>
> Konačni kod za ovaj Django ListView tutorijal možete preuzeti ovde.

### Prikazi zasnovani na klasi

Da biste prikazali listu objekata, definišete klasu koja nasleđuje `ListView` klasu. Na primer, sledeće definiše `TaskList` klasu u `views.py` aplikaciji `todo`:

```py
from django.shortcuts import render
from django.views.generic.list import ListView
from .models import Task

class TaskList(ListView):
    model = Task
    context_object_name = 'tasks'
# ...
```

je `TaskList` prikaz zasnovan na klasi koji nasleđuje od `ListView` klase. U `TaskList` klasi definišemo sledeće atribute:

- `model` određuje objekte iz čijeg modela želite da prikažete.
   U ovom primeru koristimo `Task` model. Interno, Django će upitivati sve objekte iz `Task` modela ( `Task.objects.all()` ) i prosleđivati ga šablonu.
- `context_object_name` određuje naziv promenljive liste modela u šablonu. Podrazumevano, Django koristi `object_list`. Međutim, naziv `object_list` je prilično generički. Stoga, poništavamo `context_object_name` postavljanjem vrednosti na `tasks`.

Po konvenciji, `TaskList` klasa će učitati `todo/task_list.html` šablon. Ime šablona prati ovu konvenciju:

```shell
app/model_list.html
```

Ako želite da podesite drugačije ime, možete koristiti `template_name` atribut. U ovom tutorijalu ćemo koristiti podrazumevano ime šablona, koje je `task_list`.html.

### Definišite rutu

Promenite naziv `urls.py` aplikacije `todo` na sledeći način:

```py
from django.urls import path
from .views import home, TaskList

urlpatterns = [
    path('', home, name='home'),
    path('tasks/', TaskList.as_view(), name='tasks'),
]
```

Kako to funkcioniše?

Uvezite `TaskList` klasu iz `views.py` modula.

```py
from .views import home, TaskList
```

Definišite tasks/URL adresu koja prikazuje listu zadataka:

```py
path('tasks/', TaskList.as_view(), name='tasks'),
```

U ovom kodu, mapiramo URL adresu `tasks/` na rezultat metode `as_view()` klase `TaskList`.

Imajte na umu da možete navesti atribute klase `TaskList` u `as_view()` metodi. Na primer, možete proslediti ime šablona `as_view()` metodi na sledeći način:

```py
path('tasks/', TaskList.as_view(template_name='mytodo.html'), name='tasks'),
```

Metoda `as_view()` ima argumente koji odgovaraju atributima klase `TaskList`.

### Kreiranje ListView šablona

Definišite `task_list.html` u `templates/todo` direktorijumu aplikacije `Todo`:

```html
{%extends 'base.html'%}

{%block content%}
<div class="center">
    <h2>My Todo List</h2>
    {% if tasks %}
    <ul class="tasks">
        {% for task in tasks %}
            <li><a href="#" class="{% if task.completed%}completed{%endif%}">{{ task.title }}</a> 
                <div  class="task-controls">
                    <a href="#"><i class="bi bi-trash"></i> </a>
                    <a href="#"><i class="bi bi-pencil-square"></i></a>
                </div>
            </li>
        {% endfor %}
    {% else %}
        <p>🎉 Yay, you have no pending tasks!</p>
    {% endif %}
    </ul>
</div>
{%endblock content%}
```

Šablon `task_list.html` proširuje `base.html` šablon projekta. U `task_list.html` šablonu, iteriramo kroz `tasks` `QuerySet` i prikazujemo svaki od njih kao stavku na listi.

Takođe, dodajemo `completed` CSS klasu oznaci `a` ako je zadatak završen. Ova CSS klasa će dodati liniju kroz stavku.

Ako `tasks` `QuerySet` je prazno, prikazuje se poruka da nema zadataka na čekanju.

### Uključivanje ListView linka u osnovni šablon

Izmenite `base.html` šablon da biste uključili `My Tasks` vezu u navigaciju:

```html
{%load static %}
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="{% static 'css/style.css' %}" />
        <title>Todo List</title>
    </head>

    <body>
        <header class="header">
            <div class="container">
                <a href="{%url 'home'%}" class="logo">Todo</a>
                <nav class="nav">
                    <a href="{%url 'home'%}"><i class="bi bi-house-fill"></i> Home</a>
                    <a href="{% url 'tasks' %}"><i class="bi bi-list-task"></i> My Tasks</a>
                </nav>
            </div>
        </header>
        <main>
            <div class="container">
             {%block content %}
             {%endblock content%}
            </div>
        </main>
        <footer class="footer">
            <div class="container">
               <p>© Copyright {% now "Y" %} by <a href="https://www.pythontutorial.net">Python Tutorial</a></p>
            </div>
        </footer>
    </body>

</html>
```

Ako otvorite URL adresu: `html<http://128.0.0.1:8000/tasks/>` videćete listu zadataka.

> [!Note]
>
> Konačni kod za ovaj Django ListView tutorijal možete preuzeti ovde.

Rezime ListView

- Napravite prikaz zasnovan na klasi koji prikazuje listu objekata nasleđivanjem iz `ListView` klase.

[Sadržaj](#sadržaj)


# Prikazi zasnovani na klasama

U ovom odeljku ćete naučiti prikaze zasnovane na klasama tako što ćete napraviti aplikaciju za listu zadataka koja omogućava korisnicima da se registruju, prijavljuju, resetuju lozinke, kreiraju profile i upravljaju sopstvenim zadacima.

## Sadržaj

- [Todo aplikacija](#todo-aplikacija)  
  Kako da kreirate strukturu projekta Todo aplikacije od nule.

- [ListView](#listview)  
  Kako da koristite klasu ListView za prikazivanje liste zadataka.

- [DetailView](#detailview)  
  Kako da koristite klasu DetailView za prikazivanje zadatka.

- [CreateView](#createview)  
  Kako da koristite klasu CreateView za kreiranje forme koja kreira zadatak.

- [UpdateView](#updateview)
  Kako da koristite klasu UpdateView za kreiranje forme koja uređuje zadatak.

- DeleteView  
  Kako da koristite klase DeleteView za brisanje postojećeg zadatka.

- LoginView  
- Kako da koristite LoginView za kreiranje stranice za prijavu na aplikaciju Todo.

- FormView  
  Kako da koristite FormView za kreiranje stranice za registraciju.

- Resetovanje lozinke  
  Kako da implementirate funkciju resetovanja lozinke za aplikaciju.

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
python3 -m venv venv
```

Aktivirajte `venv` virtuelno okruženje pomoću sledeće komande:

```shell
venv\scripts\activate
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

Kreirajte `urls.py` datoteku u `todo` aplikaciji i definišite rutu koja mapira na početnu URL adresu `views.home()` funkciju prikaza:

```py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

Uključite `urls.py` aplikacije `todo` u `urls.py` projekta:

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('todo.urls'))
]
```

Pokrenite Django razvojni server iz `todo_list` direktorijuma:

```py
python manage.py runserver
```

Konačno, otvorite <http://127.0.0.1:8000/> u veb pregledaču, videćete Home stranicu Todo aplikacije.

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
    user = models.ForeignKey(User, on_delete=models.CASCADE, null=True, blank=True)
    
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

Kreirajte korisnika `superuser` izvršavanjem `createsuperuser` komande:

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

[Sadržaj](#sadržaj)

## ListView

U ovom tutorijalu ćete naučiti kako da koristite Django `ListView` klasu za prikazivanje liste zadataka za aplikaciju `Todo`.

U prethodnim tutorijalima ste naučili kako da napravite aplikaciju koristeći prikaze zasnovane na funkcijama.

Prikazi zasnovani na funkcijama su jednostavni i fleksibilni. U ranijim verzijama, Django je podržavao samo prikaze zasnovane na funkcijama. Kasnije je Django dodao podršku za prikaze zasnovane na klasama koji vam omogućavaju da definišete prikaze pomoću klasa.

Prikazi zasnovani na klasama su alternativni način implementacije prikaza. Oni ne zamenjuju prikaze zasnovane na funkcijama. Međutim, imaju neke prednosti u poređenju sa prikazima zasnovanim na funkcijama:

- Organizujte kod povezan sa HTTP metodama kao što su GET i POST koristeći odvojene metode, umesto uslovnog grananja u istoj funkciji.
- Iskoristite višestruko nasleđivanje da biste kreirali klase prikaza koje se mogu ponovo koristiti.

Koristićemo prikaze zasnovane na klasama da bismo izgradili `Todo` aplikaciju.

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

- `model` određuje model iz koga će objekti biti prikazani.
   U ovom primeru koristimo `Task` model. Interno, Django će praviti upit za sve objekte iz `Task` modela ( `Task.objects.all()` ) i prosleđivati ih šablonu.

- `context_object_name` određuje naziv promenljive liste objekata modela u šablonu. Podrazumevano, Django koristi `object_list`. Međutim, naziv `object_list` je prilično generički. Stoga, poništavamo `context_object_name` postavljanjem vrednosti na `tasks`.

Po konvenciji, `TaskList` klasa će učitati `todo/task_list.html` šablon. Ime šablona prati ovu konvenciju:

```shell
app/model_list.html
```

Ako želite da podesite drugačije ime, možete koristiti `template_name` atribut. U ovom tutorijalu ćemo koristiti podrazumevano ime šablona, koje je `task_list.html`.

### Definišite ListView rutu

Promenite `ListView` rutu u `urls.py` aplikacije `todo` na sledeći način:

```py
from django.urls import path
from .views import home, TaskList

urlpatterns = [
    path('', home, name='home'),
    path('tasks/', TaskList.as_view(), name='tasks'),
]
```

Kako ovo funkcioniše?

Uvezite `TaskList` klasu iz `views.py` modula.

```py
from .views import home, TaskList
```

Definišite task listu i URL adresu koja prikazuje listu zadataka:

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

Ako je `tasks` prazan, prikazuje se poruka da nema zadataka na čekanju.

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

### Rezime ListView

- Napravite prikaz zasnovan na klasi koji prikazuje listu objekata nasleđivanjem iz `ListView` klase.

[Sadržaj](#sadržaj)

## DetailView

U ovom tutorijalu ćete naučiti kako da koristite `DetailView` klasu za prikazivanje objekta.

### Definisanje detaljnog prikaza

Django `DetailView` vam omogućava da definišete prikaz zasnovan na klasi koji prikazuje detalje objekta. Da biste koristili `DetailView` klasu, definišete klasu koja nasleđuje tDetailView` klasu.

Na primer, sledeće definiše `TaskDetail` prikaz zasnovan na klasi koji prikazuje detalje zadatka aplikacije `Todo`:

```py
from django.shortcuts import render
from django.views.generic.list import ListView
from django.views.generic.detail import DetailView
from .models import Task

class TaskDetail(DetailView):
    model = Task
    context_object_name = 'task'
    
#...  
```

Kako to funkcioniše?

Uvezite `DetailView` iz `django.views. generic.detail`:

```py
from django.views.generic.detail import DetailView
```

Definišite `TaskDetail` klasu koja nasleđuje klasu `DetailView`. U `TaskDetail` klasi definišemo sledeće atribute:

- `modelo` dređuje klasu objekta koji će biti prikazan.

- `context_object_name` određuje ime objekta u šablonu. Podrazumevano, Django koristi `object` kao ime objekta u šablonu. Da bismo to učinili očiglednijim, umesto toga koristimo `task` kao ime objekta.

Podrazumevano, `TaskDetail` klasa će učitati šablon sa imenom `task_detail.html` iz `templates/todo` aplikacije.

Ako želite da koristite drugačije ime šablona, možete koristiti `template_name` atribut u `TaskDetail` klasi.

### Napravite DetailView šablon

Napravite `task_detail.html` šablon u `templates/todo` direktorijumu pomoću sledećeg koda:

```html
{%extends 'base.html'%}

{%block content%}

 <article class="task">
    <header>
        <h2>{{ task.title }}</h2>
        <span class="badge {% if task.completed %}badge-completed{% else %}badge-pending{%endif%}">
            {% if task.completed %} Completed {%else%} Pending {%endif%}
        </span>
    </header>
    <p>{{task.description}}</p>
</article>

{%endblock content%}
```

Šablon `task_detail.html` proširuje `base.html` šablon.

Šablon `task_detail.html` koristi `task` kao objekat i prikazuje atribute zadatka, uključujući naslov, status (završen ili ne) i opis.

### Definisanje DetailView rute

Definišite rutu koja mapira URL adresu koja prikazuje zadatak sa rezultatom metode `as_view()` klase `TaskDetail`:

```py
from django.urls import path
from .views import home, TaskList, TaskDetail

urlpatterns = [
    path('', home, name='home'),
    path('tasks/', TaskList.as_view(),name='tasks'),
    path('task/<int:pk>/',TaskDetail.as_view(),name='task'),
]
```

URL prihvata ceo broj kao ID (ili primarni ključ, pk) zadatka. `TaskDetail` će uzeti ovaj pkparametar, izabrati zadatak iz baze podataka prema ID-u, konstruisati objekat `Task` i proslediti ga šablonu.

### Izmena DetailView šablona

Izmenite `task_list.html` šablon da biste uključili vezu do svakog zadatka na listi zadataka koristeći `url` oznaku:

```html
{%extends 'base.html'%}

{%block content%}

<div class="center">
    <h2>My Todo List</h2>
    {% if tasks %}
    <ul class="tasks">
        {% for task in tasks %}
            <li><a href="{% url 'task' task.id %}" class="{% if task.completed%}completed{%endif%}">{{ task.title }}</a> 
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

Kada kliknete na vezu svake oznake, bićete preusmereni na stranicu sa detaljima zadatka.

### Pokrenite Django dev server

```shell
python manage.py runserver
```

i otvorite listu zadataka: <http://127.0.0.1:8000/tasks/>.

Ako kliknite na zadatak, npr. Learn Python, bićete preusmereni na stranicu sa detaljima zadatka.

### Rezime DetailView

- Koristite `DetailView` za prikaz detalja objekta.

[Sadržaj](#sadržaj)

## CreateView

U ovom tutorijalu ćete naučiti kako da koristite `CreateView` klasu za definisanje prikaza zasnovanog na klasi koji kreira zadatak za aplikaciju `Todo`.

### Definisanje klase

Klasa `CreateView` vam omogućava da kreirate prikaz zasnovan na klasi koji prikazuje obrazac za kreiranje objekta, ponovno prikazivanje obrasca sa greškama u validaciji i čuvanje objekta u bazi podataka.

Da biste koristili `CreateView` klasu, definišete klasu koja nasleđuje od nje i dodate joj neke atribute i metode.

Na primer, sledeći primer koristi `CreateView` klasu za definisanje prikaza zasnovanog na klasi koji prikazuje obrazac za kreiranje novog zadatka u aplikaciji `Todo`:

```py
# ..
from django.views.generic.edit import CreateView
from django.contrib import messages
from django.urls import reverse_lazy
from .models import Task

class TaskCreate(CreateView):
  model = Task
  fields = ['title','description','completed']
  success_url = reverse_lazy('tasks')
    
  def form_valid(self, form):
    form.instance.user = self.request.user
    messages.success(self.request, "The task was created successfully.")
    return super(TaskCreate,self).form_valid(form)

# other classes & functions
```

Kako ovo funkcioniše?

Uvezite `CreateView` klasu, `reverse_lazy()` funkciju i `messages` modul.

Definišite `TaskCreate` klasu koja nasleđuje od `CreateView` klase. U `CreateView` klasi definišemo sledeće atribute i metode:

- `model` određuje klasu objekta koji treba kreirati ( `Task` ).

- `fields` je lista polja koja se prikazuju na obrascu. U ovom primeru, obrazac će prikazivati naslov, opis i popunjene atribute modela `Task`.

- `success_url` je ciljni URL na koji će Django preusmeriti nakon što se zadatak uspešno kreira. U ovom primeru, preusmeravamo na listu zadataka koristeći `reverse_lazy()` funkciju. `reverse_lazy()` prihvata ime prikaza i vraća URL.

- `form_valid()` je metoda koja se poziva nakon uspešnog slanja forme. U ovom primeru, postavljamo korisnika na trenutno prijavljenog korisnika, kreiramo fleš poruku i vraćamo rezultat `form_valid()` metode nadklase.

Podrazumevano, `CreateView` klasa koristi `task_form.html` šablon iz `templates/todo` sa sledećom konvencijom imenovanja:

```py
model_form.html
```

Ako želite da koristite drugi šablon, možete da zamenite podrazumevani šablon koristeći `template_name` atribut u `TaskCreate` klasi.

### Kreiranje CreateView šablona

Kreirajte `task_form.html` u `templates/todo` direktorijumu pomoću sledećeg koda:

```html
{%extends 'base.html'%}

{%block content%}

<div class="center">
    <form method="post" novalidate class="card">
         {%csrf_token %}
         
         <h2>Create Task</h2>
        {% for field in form %}
            {% if field.name == 'completed' %}
                <p>
                    {{ field.label_tag }}
                    {{ field }}
                </p>
                {% if field.errors %}
                    <small class="error">{{ field.errors|striptags  }}</small> 
                {% endif %}
            {% else %}
                {{ field.label_tag }} 
                {{ field }}
                {% if field.errors %}
                    <small class="error">{{ field.errors|striptags  }}</small> 
                {% endif %}
            {% endif %}
        {% endfor %}
        
        <div class="form-buttons">
            <input type="submit" value="Save" class="btn btn-primary"/>
            <a href="{%url 'tasks'%}" class="btn btn-outline">Cancel</a>
        </div>
    </form>
</div>

{%endblock content%}
```

U `task_form.html`, polja obrasca prikazujemo ručno. Ako želite da automatski generišete obrazac, možete koristiti jedan od sledećih atributa:

```html
{{ form.as_p }}   # render the form as <p>
{{ form.as_div }} # render the form as <div>
{{ form.as_ul }}  # redner the form as <ul>
```

### Definisanje CreateView rute

Dodajte rutu u `urls.py` aplikacije `todo` mapiranjem URL-a sa rezultatom metode `as_view()` klase `TaskCreate`:

```py
from django.urls import path
from .views import home, TaskList, TaskDetail, TaskCreate

urlpatterns = [
    path('', home, name='home'),
    path('tasks/', TaskList.as_view(),name='tasks'),
    path('task/<int:pk>/', TaskDetail.as_view(),name='task'),
    path('task/create/', TaskCreate.as_view(),name='task-create'),
]
```

### Prikazivanje fleš poruka i dodavanje CreateView linka

Izmenite `base.html` šablon projekta na:

- Prikažite fleš poruke.
- Dodajte New Task vezu u navigaciju.

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
                    <a href="{% url 'home'%}"><i class="bi bi-house-fill"></i> Home</a>
                    <a href="{% url 'tasks' %}"><i class="bi bi-list-task"></i> My Tasks</a>
                    <a href="{% url 'task-create' %}"><i class="bi bi-plus-circle"></i> Create Task</a>
                </nav>
            </div>
        </header>
        <main>
            <div class="container">
                {% if messages %}
                    {% for message in messages %}
                        <div class="alert alert-{{message.tags}}">
                            {{message}}
                        </div>
                    {% endfor %}
                {% endif %}
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

Pokrenite Django dev server i otvorite URL adresu <http://127.0.0.1:8000/task/create/>, videćete obrazac za kreiranje novog `Todo` objekta.

Unesite naslov i opis i kliknite na dugme "Save", bićete preusmereni na stranicu sa listom zadataka sa porukom.

### Rezime CreateView

- Koristite klasu `CreateView` da definišete prikaz zasnovan na klasi koji kreira objekat.

## UpdateView

U ovom tutorijalu ćete naučiti kako da koristite `UpdateView` klasu za kreiranje prikaza zasnovanog na klasi koji uređuje postojeći objekat.

### Definisanje klase UpdateView

Klasa `UpdateView` vam omogućava da kreirate prikaz zasnovan na klasi koji:

- Prikažite formu za uređivanje postojećeg objekta.
- Ponovo prikažite formu  ako sadrži greške u validaciji.
- Sačuvajte izmene objekta u bazi podataka.

Forma se generiše automatski iz modela objekta, osim ako eksplicitno ne navedete klasu forme.

Da bismo demonstrirali `UpdateView` klasu, kreiraćemo prikaz koji uređuje zadatak `Todo` aplikacije.

Da bismo to uradili, modifikujemo `views.py` aplikacije `todo` i definišemo `TaskUpdate` klasu koja nasleđuje tu `UpdateView` klasu ovako:

```py
# ...
from django.views.generic.edit import CreateView, UpdateView
from django.contrib import messages
from django.urls import reverse_lazy
from .models import Task

class TaskUpdate(UpdateView):
    model = Task
    fields = ['title','description','completed']
    success_url = reverse_lazy('tasks')
    
    def form_valid(self, form):
        messages.success(self.request, "The task was updated successfully.")
        return super(TaskUpdate,self).form_valid(form)
# ...
```

Kako ovo funkcioniše?

Uvezite `UpdateView` iz `django.views.generic.edit`:

```py
from django.views.generic.edit import CreateView, UpdateView
```

Definišite `TaskUpdate` klasu koja nasleđuje `UpdateView` klasu. U `TaskUpdate` klasi definišite sledeće atribute i metode:

- `model` određuje klasu objekta koji treba uređivati. Zato u ovom primeru određujemo Task kao model.
- `fields` je lista koja određuje polja forme. U ovom primeru koristimo polja za naslov, opis i popunjena polja.
- `success_url` je ciljni URL (lista zadataka) na koji će Django preusmeriti nakon što se zadatak uspešno ažurira.
- form_valid() metoda se poziva nakon uspešnog slanja forme. U ovom primeru, kreiramo fleš poruku i vraćamo rezultat metode `form_valid()` nadklase.

Podrazumevano, `TaskUpdate` klasa koristi `task_form.html` šablon iz `templates/todo` direktorijuma. Imajte na umu da klase `CreateView` i `UpdateView` dele isto ime šablona.

Ako želite da koristite drugačije ime šablona, možete ga navesti pomoću `template_name` atributa.

### Kreiranje UpdateList šablona

Izmenite `task_form.html` šablon koji prikazuje `UpdateTask` naslov ako je promenljiva zadatka dostupna u šablonu ( režim uređivanja ) ili `CreateTask` ako nije (režim kreiranja).

```html
{%extends 'base.html'%}

{%block content%}

<div class="center">
<form method="post" novalidate class="card">
    {%csrf_token %}
    <h2>{% if task %} Update {%else %} Create {%endif%} Task</h2>
    {% for field in form %}
        {% if field.name == 'completed' %}
            <p>
                {{ field.label_tag }}
                {{ field }}
            </p>
            {% if field.errors %}
                <small class="error">{{ field.errors|striptags  }}</small> 
            {% endif %}
        {% else %}
            {{ field.label_tag }} 
            {{ field }}
            {% if field.errors %}
                <small class="error">{{ field.errors|striptags  }}</small> 
            {% endif %}
        {% endif %}
    {% endfor %}

    <div class="form-buttons">
        <input type="submit" value="Save" class="btn btn-primary"/>
        <a href="{%url 'tasks'%}" class="btn btn-outline">Cancel</a>
    </div>
</form>
</div>

{%endblock content%}
```

### Definisanje UpdateList rute

Definišite rutu u `urls.py` aplikaciji `todo` koja mapira URL adresu sa rezultatom metode `as_view()` klase `TaskUpdate`:

```py
from django.urls import path
from .views import (
    home, 
    TaskList, 
    TaskDetail, 
    TaskCreate, 
    TaskUpdate
)

urlpatterns = [
    path('', home, name='home'),
    path('tasks/', TaskList.as_view(),name='tasks'),
    path('task/<int:pk>/', TaskDetail.as_view(),name='task'),
    path('task/create/', TaskCreate.as_view(),name='task-create'),
    path('task/update/<int:pk>/', TaskUpdate.as_view(),name='task-update'),
]
```

### Uključi UpdateList link za izmene

Izmenite `task_list.html` šablon da biste uključili vezu za uređivanje za svaki zadatak na listi zadataka:

```html
{%extends 'base.html'%}

{%block content%}

<div class="center">
    <h2>My Todo List</h2>
    {% if tasks %}
    <ul class="tasks">
        {% for task in tasks %}
            <li><a href="{% url 'task' task.id %}" class="{% if task.completed%}completed{%endif%}">{{ task.title }}</a> 
                <div  class="task-controls">
                    <a href="#"><i class="bi bi-trash"></i> </a>
                    <a href="{%url 'task-update' task.id %}"><i class="bi bi-pencil-square"></i></a>
                </div>
            </li>
        {% endfor %}
    {% else %}
        <p>🎉 Yay, you have no pending tasks! <a href="{%url 'task-create'%}">Create Task</a></p>
    {% endif %}
    </ul>
</div>

{%endblock content%}
```

Ako uredite zadatak sa liste obaveza dodavanjem tri zvezdice ( *** ) naslovu i označite zadatak kao završen.

Kliknite na dugme "Save" i videćete da su naslov i status zadatka ažurirani:

### Rezime UpdateView LInk

- Definišite novu klasu koja nasleđuje `UpdateView` klasu da biste kreirali prikaz zasnovan na klasi koji uređuje postojeći objekat.

[Sadržaj](#sadržaj)

## DeleteView

U ovom tutorijalu ćete naučiti kako da koristite `DeleteView` klasu za definisanje prikaza zasnovanog na klasi koji briše postojeći objekat.

Izgradnja `DeleteView` klasu

Klasa `DeleteView` vam omogućava da definišete prikaz zasnovan na klasi koji prikazuje stranicu za potvrdu i briše postojeći objekat.

Ako je metod HTTP zahteva `GET`, `DeleteView` će prikazati stranicu za potvrdu. Međutim, ako je zahtev `POST`, `DeleteView` prikaz će obrisati objekat.

Da biste koristili `DeleteView` klasu, definišete klasu koja nasleđuje od nje i dodajete atribute i metode da biste poništili podrazumevana ponašanja.

Na primer, sledeće definiše `TaskDelete` klasu koja briše zadatak za aplikaciju `Todo`:

```py
#...
from django.views.generic.edit import DeleteView, CreateView, UpdateView
from django.contrib import messages
from django.urls import reverse_lazy

from .models import Task

class TaskDelete(DeleteView):
    model = Task
    context_object_name = 'task'
    success_url = reverse_lazy('tasks')
    
    def form_valid(self, form):
        messages.success(self.request, "The task was deleted successfully.")
        return super(TaskDelete,self).form_valid(form)

#...
```

U ovom primeru definišemo TaskDeleteklasu koja je podklasa klase DeleteView. TaskDeleteKlasa ima sledeće atribute:

- `model` određuje klasu modela ( `Task` ) koja će biti obrisana.
- `context_object_name` određuje ime objekta koje će biti prosleđeno šablonu. Podrazumevano, `DeleteView` klasa koristi object kao ime. Međutim, možete zameniti ime pomoću `context_object_name` atributa.
- `success_url` je URL adresa na koju će biti preusmereno nakon što se objekat uspešno obriše.
- `form_valid()` metoda se poziva kada se objekat uspešno obriše. U ovom primeru kreiramo fleš poruku.

Podrazumevano, `DeleteView` klasa koristi `task_confirmation_delete.html` šablon ako ga eksplicitno ne navedete.

### Kreiranje DeleteView šablona

Napravite novi `task_confirm_delete.html` šablon datoteke u `templates/todo` aplikaciji pomoću sledećeg koda:

```html
{%extends 'base.html'%}

{%block content%}
<div class="center">
    <form method="post" class="card">
        {% csrf_token %}
        <h2>Delete Task</h2>
        <p>Are you sure that you want to delete "{{task}}"?</p>
        <p class="form-buttons">
            <input type="submit" class="btn btn-primary" value="Delete">
            <a href="{% url 'tasks'%}" class="btn btn-outline">Cancel</a>
        </p>
    </form>
</div>

{%endblock content%}
```

Ovaj kod proširuje `task_confirm_delete.html` šablon `base.html` i sadrži obrazac koji briše zadatak.

### Definisanje DeleteView rute

Definišite novu rutu u `urls.py` koja mapira URL adresu koja briše zadatak sa rezultatom metode `as_view()` klase `TaskDelete` prikaza:

```py
from django.urls import path
from .views import (
    home, 
    TaskList, 
    TaskDetail, 
    TaskCreate, 
    TaskUpdate,
    TaskDelete
)

urlpatterns = [
    path('', home, name='home'),
    path('tasks/', TaskList.as_view(),name='tasks'),
    path('task/<int:pk>/', TaskDetail.as_view(),name='task'),
    path('task/create/', TaskCreate.as_view(),name='task-create'),
    path('task/update/<int:pk>/', TaskUpdate.as_view(),name='task-update'),
    path('task/delete/<int:pk>/', TaskDelete.as_view(),name='task-delete'),
]
```

### Uključite DeleteView link za brisanje zadatka

Izmenite `task_list.html` šablon da biste dodali vezu koja briše zadatak svakom zadatku na listi zadataka:

```html
{%extends 'base.html'%}

{%block content%}

<div class="center">
    <h2>My Todo List</h2>
    {% if tasks %}
    <ul class="tasks">
        {% for task in tasks %}
            <li><a href="{% url 'task' task.id %}" class="{% if task.completed%}completed{%endif%}">{{ task.title }}</a> 
                <div  class="task-controls">
                    <a href="{%url 'task-delete' task.id %}"><i class="bi bi-trash"></i> </a>
                    <a href="{%url 'task-update' task.id %}"><i class="bi bi-pencil-square"></i></a>
                </div>
            </li>
        {% endfor %}
    {% else %}
        <p>🎉 Yay, you have no pending tasks! <a href="{%url 'task-create'%}">Create Task</a></p>
    {% endif %}
    </ul>
</div>

{%endblock content%}
```

Ako kliknete na dugme za brisanje da biste obrisali zadatak sa liste, dobićete stranicu za potvrdu brisanja.

Klikom na dugme "Delete" obrisaćete zadatak iz baze podataka i vratiti ga na listu zadataka.

Konačni kod za ovaj `DeleteView` tutorijal možete preuzeti ovde.

### Rezime DeleteView

Koristite `DeleteView` klasu da definišete prikaz zasnovan na klasi koji briše postojeći objekat.

## LoginView

U ovom tutorijalu ćete naučiti kako da koristite `LoginView` za kreiranje stranice za prijavu za `Todo` aplikaciju.

`LoginView` vam omogućava da prikažete formu za prijavu i obradite akciju prijave. Koristićemo LoginView klasu da kreiramo stranicu za prijavu za `Todo` aplikaciju.

### Kreiranje i konfigurisanje users aplikacije za Task projekat

Aplikacija `users` će imati sledeće funkcionalnosti:

- Prijava / Odjava korisnika
- Registrovanje korisnika
- Resetovanje lozinke korisnika

U ovom tutorijalu ćemo se fokusirati na funkcije `Prijava / Odjava`.

Koristite `startapp` komandu za kreiranje `users` aplikacije:

```shell
django-admin startapp users
```

Zatim, registrujte `users` aplikaciju u `settings.py` projektu:

```py
INSTALLED_APPS = [
    #...
    'users'
]
```

Zatim, kreirajte `urls.py` u `users` aplikaciji sa sledećim kodom:

```py
from django.urls import path

urlpatterns = []
```

Nakon toga, uključite `urls.py` aplikaciju `user` su `urls.py` projekat:

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('todo.urls')),
    path('',include('users.urls'))
]
```

Konačno, kreirajte `templates` direktorijum i `users` direktorijum unutar `templates` direktorijuma u `users` aplikaciji za čuvanje šablona.

### Kreiranje LoginView stranice

Sledeći kod definiše klasu koja nasleđuje od klase `LoginView` u datoteci `views.py`.

```py
from django.contrib.auth.views import LoginView
from django.urls import reverse_lazy
from django.contrib import messages

class MyLoginView(LoginView):
    redirect_authenticated_user = True
    
    def get_success_url(self):
        return reverse_lazy('tasks') 
    
    def form_invalid(self, form):
        messages.error(self.request,'Invalid username or password')
        return self.render_to_response(self.get_context_data(form=form))
```

Kako ovo funkcioniše?

Uvezite `LoginView` iz `django.contrib.auth.views`, `reverse_lazy` iz `django.urls` i messages iz `django.contrib`.

```py
from django.contrib.auth.views import LoginView
from django.urls import reverse_lazy
from django.contrib import messages
```

Definišite klasu `MyLoginView` koja nasleđuje od klase `LoginView`. Ona ima sledeće atribute i metode:

- `redirect_authenticated_user` je podešeno na `True` da bi naložilo Django-u da preusmeri korisnike nakon što se uspešno prijave. Podrazumevano, `redirect_authenticated_user` je `False`, što isključuje preusmeravanje.

- `get_success_url()` vraća URL adresu za preusmeravanje nakon što se korisnici uspešno prijave.

- `form_invalid()` se poziva kada prijava ne uspe. U `form_invalid()`, kreiramo fleš poruku i ponovo prikazujemo obrazac za prijavu.

### Definisanje LoginView rute

Izmenite `views.py` datoteku da biste definisali rutu za `LoginView` stranicu:

```py
from django.urls import path
from .views import MyLoginView

urlpatterns = [
    path('login/', MyLoginView.as_view(),name='login'),
]
```

Kako ovo funkcioniše?

Uvezite `MyLoginView` klasu iz `views.py`:

```py
from .views import MyLoginView
```

Mapirajte login rutu do rezultata `as_view()` metode klase `MyLoginView`.

```py
urlpatterns = [
    path('login/', MyLoginView.as_view(),name='login'),
]
```

### Kreiranje LoginView šablona

Kreirajte `login.html` šablon u `templates/users` direktorijumu pomoću sledećeg koda:

```html
{%extends 'base.html'%}

{%block content%}

<div class="center">
    <form method="post" class="card" novalidate>
        {% csrf_token %}
        <h2 class="text-center">Log in to your account</h2>
        {% for field in form %}
            {{ field.label_tag }} 
            {{ field }}
            {% if field.errors %}
                <small>{{ field.errors|striptags }}</small> 
            {% endif %}
        {% endfor %}

        <input type="submit" value="Login" class="btn btn-primary full-width">
        <hr>
        <p class="text-center">Forgot your password <a href="#">Reset Password</a></p>
        <p class="text-center">Don't have a account? <a href="#">Join Now</a></p>
    </form>
</div>

{%endblock content%}
```

Ako otvorite URL adresu za prijavu <http://127.0.0.1:8000/login/> videćete obrazac za prijavu.

Ako unesete važeće korisničko ime i lozinku, uspešno ćete se prijaviti. U suprotnom, dobićete poruku da ste uneli nevažeće korisničko ime ili lozinku.

Dodajte LOGIN_URL u `settings.py` projekat:

```py
LOGIN_URL = 'login'
```

Ako pokušate da pristupite stranici koja zahteva prijavu, Django će koristiti `LOGIN_URL` za preusmeravanje. Ako ne dodate `LOGIN_URL` u `settings.py`, Django će koristiti podrazumevani URL za prijavu, koji je `accounts/login/`.

### Kreiranje URL-a za odjavu

`LogoutView` odjavljuje korisnika i prikazuje poruku. Koristićemo `LogoutView`da kreiramo link za odjavu.

Za razliku od `LoginView` klase, možete koristiti `LogoutView` klasu direktno u `urls.py`. Na primer, možete izmeniti `views.py` da biste kreirali rutu za URL adresu za odjavu:

```py
from django.urls import path
from .views import MyLoginView
from django.contrib.auth.views import LogoutView 

urlpatterns = [
    path('login/', MyLoginView.as_view(),name='login'),
    path('logout/', LogoutView.as_view(next_page='login'),name='logout'),
]
```

Kako to funkcioniše?

Uvezite `LogoutView` iz `django.contrib.auth.views`:

```py
from django.contrib.auth.views import LogoutView
```

Mapirajte URL adresu `logout/` sa rezultatom metode `as_view()`klase `LogoutView`. `next_page` argument određuje URL adresu na koju će korisnici biti preusmereni nakon što se uspešno odjave.

path('logout/', LogoutView.as_view(next_page='login'),name='logout')
Kodni jezik:  Pajton  ( python )

Dodavanje linkova za prijavu/odjavu u zaglavlje

Ako se korisnik prijavi, zaglavlje prikazuje početnu stranicu, moje zadatke, novi zadatak i vezu za odjavu. Kada se korisnik odjavi, zaglavlje prikazuje veze početna stranica, prijava i pridruži se sada.

Da biste to postigli, izmenite base.htmlšablon projekta na sledeći način.

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
                    {% if request.user.is_authenticated %}
                            <a href="{% url 'tasks' %}"><i class="bi bi-list-task"></i> My Tasks</a>
                            <a href="{% url 'task-create' %}"><i class="bi bi-plus-circle"></i> Create Task</a>
                            <a href="#">Hi {{request.user | title}}</a>
                            <a href="{% url 'logout' %}" class="btn btn-outline">Logout</a>
                    {% else %}
                        <a href="{% url 'login' %}" class="btn btn-outline">Login</a>
                        <a href="#" class="btn btn-primary">Join Now</a>
                    {% endif %}
                  </nav>
            </div>
        </header>
        <main>
            <div class="container">
                {% if messages %}
                {% for message in messages %}
                    <div class="alert alert-{{message.tags}}">
                           {{message}}
                         </div>
                       {% endfor %}
                {% endif %}
            
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

Imajte na umu da ako se korisnik prijavi, request.user.is_authenticatedvraća True. Stoga, možete koristiti ovo svojstvo da proverite da li je korisnik prijavljen ili ne.

Ako niste prijavljeni, videćete sledeće linkove u navigaciji:

Međutim, ako se prijavite, videćete sledeće linkove za navigaciju:
Prijava je obavezna

Iako niste prijavljeni, i dalje možete upravljati listom zadataka, kao što je pregled, dodavanje, uređivanje i brisanje zadataka. Da biste zaštitili ove stranice, koristićete LoginRequiredMixinklasu.

Da biste to uradili, modifikujete views.pyaplikaciju todo i koristite LoginRequiredMixinklasu na sledeći način:

```py
from django.shortcuts import render
from django.views.generic.list import ListView
from django.views.generic.detail import DetailView
from django.views.generic.edit import CreateView, UpdateView, DeleteView
from django.contrib import messages
from django.urls import reverse_lazy
from django.contrib.auth.mixins import LoginRequiredMixin

from .models import Task

class TaskDelete(LoginRequiredMixin, DeleteView):
    model = Task
    context_object_name = 'task'
    success_url = reverse_lazy('tasks')
    
    def form_valid(self, form):
        messages.success(self.request, "The task was deleted successfully.")
        return super(TaskDelete,self).form_valid(form)

class TaskUpdate(LoginRequiredMixin, UpdateView):
    model = Task
    fields = ['title','description','completed']
    success_url = reverse_lazy('tasks')
    
    def form_valid(self, form):
        messages.success(self.request, "The task was updated successfully.")
        return super(TaskUpdate,self).form_valid(form)
    
class TaskCreate(LoginRequiredMixin, CreateView):
    model = Task
    fields = ['title','description','completed']
    success_url = reverse_lazy('tasks')
    
    def form_valid(self, form):
        form.instance.user = self.request.user
        messages.success(self.request, "The task was created successfully.")
        return super(TaskCreate,self).form_valid(form)
        
class TaskDetail(LoginRequiredMixin, DetailView):
    model = Task
    context_object_name = 'task'
    
class TaskList(LoginRequiredMixin,ListView):
    model = Task
    context_object_name = 'tasks'
    
def home(request):
    return render(request,'home.html')
```

Ako se niste prijavili i pokušali ste da pristupite zaštićenoj stranici, Django će vas preusmeriti na stranicu za prijavu. Na primer:

```html
http://127.0.0.1:8000/task/create
```

Django će vas preusmeriti na stranicu za prijavu koristeći LOGIN_URLkonfigurisano u `settings.py`:

```html
http://127.0.0.1:8000/login/?next=/task/create/
```

### Rezime

- Koristite `LoginView` klasu da biste kreirali stranicu za prijavu.
- Koristite `LogoutView` klasu da odjavite korisnika.
- Koristite `LoginRequiredMixin` klasu da zaštitite stranicu.

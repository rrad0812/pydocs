
# Generalno
  
## Sadržaj

  [Nazad](sadrzaj.md)
  
- [Kako promeniti 'Django administration' tekst?](#kako-promeniti-django-administration-tekst)
- [Kako postaviti naslov modela u množini?](#kako-postaviti-naslov-modela-u-množini)
- [Kako kreirati dva nezavisna sajta?](#kako-kreirati-dva-nezavisna-sajta)
- [Kako ukloniti podrazumevane aplikacije iz Django admina?](#kako-ukloniti-podrazumevane-aplikacije-iz-django-admina)
- [Kako dodati logo u Django admin?](#kako-dodati-logo-u-django-admin)
- [Kako nadjačati Django admin šablone?](#kako-nadjačati-django-admin-šablone)
- [Kako da postavimo prilagodjeni poredak aplikacija i modela?](#kako-da-postavimo-prilagodjeni-poredak-aplikacija-i-modela)
- [Kako dodati jedan model dva puta na admin?](#kako-dodati-jedan-model-dva-puta-na-admin)
- [Kako dodati db view na Django admin?](#kako-dodati-db-view-na-django-admin)

### Kako promeniti "Django administration" tekst?

Podrazumevano Django admin prikazuje "Django administration". Hajde da zamenimo sa "UMSRA Administration".

Ovaj tekst je na sledećim stranama:

- Login strana
- Listview strana
- HTML title tag

Možemo napraviti sve tri promene u `urls.py`:

```py
admin.site.site_header = "UMSRA Admin"
admin.site.site_title = "UMSRA Admin Portal"
admin.site.index_title = "Welcome to UMSRA Researcher Portal"
```

> [!Note]
>
> Ispravan način je nasledjivanje admin klase i zadavanje ova tri atributa klase. Kako mi radimo sa admin.site klasom (default klasa admin strana) ove klasne atribute zadajemo u `urls.py`.

[Sadržaj](#sadržaj)

### Kako postaviti naslov modela u množini?

Podrazumevano admin prikazuje imena modela u množini dodajući 's'. Često je to nepravilna množina. Od vas je zatraženo da postavite tačne pravopisne množine: `Categories` i `Heroes`. To možete učiniti postavljanjem `verbose_name_plural` u svojim modelima. Promenite u `models.py`:

```py
class Category(models.Model):
  ...
  class Meta:
    verbose_name_plural = "Categories"

class Hero(Entity):
  ...
  class Meta:
    verbose_name_plural = "Heroes"
```

[Sadržaj](#sadržaj)

### Kako kreirati dva nezavisna sajta?

Uobičajeni način kreiranja admin strana je stavljanje svih modela u jednog admina. Međutim, moguće je imati više admin lokacija u jednoj Django aplikaciji. Trenutno su naši modeli entiteta i događaja na istom mestu. UMSRA ima dve različite grupe koje istražuju događaje i entitete, i zato želi da podeli admine. Zadržaćemo zadanog admina za entitete i kreiraćemo novu subklasu AdminSite za događaje.

U events/admin.py uradimo sledeće:

```py
from django.contrib.admin import AdminSite

class EventAdminSite(AdminSite):
  site_header = "UMSRA Events Admin"
  site_title = "UMSRA Events Admin Portal"
  index_title = "Welcome to UMSRA Researcher Events Portal"

event_admin_site = EventAdminSite(name='event_admin')

event_admin_site.register(Epic)
event_admin_site.register(Event)
event_admin_site.register(EventHero)
```

I promenimo urls.py na:

```py
from events.admin import event_admin_site

urlpatterns = [
  path('entity-admin/', admin.site.urls),
  path('event-admin/', event_admin_site.urls),
]
```

Ovo razdvaja aplikaciju na dva admin sajta. Oba su raspoloživa na:

- /entity-admin/
- /event-admin/.

[Sadržaj](#sadržaj)

### Kako ukloniti podrazumevane aplikacije iz Django admina?

Django će uključiti `django.contrib.auth` u `INSTALLED_APPS`, to znači da će `User` i `Groups` modeli da budu uključeni u admin sajt automatski.

Ako želite možete ih ukloniti:

```py
from django.contrib.auth.models import User, Group
admin.site.unregister(User)
admin.site.unregister(Group)
```

[Sadržaj](#sadržaj)

### Kako dodati logo u Django admin?

UMSRA marketing želi logo na admin stranama. Potrebno je da nadjačaš podrazumevani šablon. U Django `settings.py`, treba `TEMPLATES` da varijabla izgleda ovako:

```py
TEMPLATES = [
  {
    'BACKEND': 'django.template.backends.django.DjangoTemplates',
    DIRS': [],
    'APP_DIRS': True,
    OPTIONS': {
      'context_processors': [
      'django.template.context_processors.debug',
      'django.template.context_processors.request',
      'django.contrib.auth.context_processors.auth',
      'django.contrib.messages.context_processors.messages',
      ],
    },
  }
]
```

Ovo znači da Django gleda šablone u `app` direktorijumu sa nazivom `templates` unutar svakog `app` direktorijuma, ali možete nadjačati postavkom vrednosti u `TEMPLATES.DIRS`.

Promenimo

```py
'DIRS': [], 
```

na:

```py
'DIRS': [os.path.join(BASE_DIR, 'templates/')],
```

I kreirajmo `templates` direktorijum u projektnom direktorijumu.

Ako je `STATICFILES_DIRS` postavka prazna postavimo je na:

```py
STATICFILES_DIRS = [
  os.path.join(BASE_DIR, "static"),
]
```

Sada kopirajmo `base_site.html` iz admin app u `templates\admin` direktorijum koji ste kreirali. Zamenimo podrazumevani tekst u `brend` bloku sa:

```html
<h1 id="site-name">
<a href="{% url 'admin:index' %}">

<img src="{% static 'umsra_logo.png' %}" height="40px" />
</a>

</h1>
```

Sa promenama `base_site.html` sada izleda kao:

```html
{% extends "admin/base.html" %}
{% load staticfiles %}

{% block title %}
{{ title }} | {{ site_title|default:_('Django site admin')}}
{% endblock %}

{% block branding %}
<h1 id="site-name">
<a href="{% url 'admin:index' %}">
<img src="{% static 'umsra_logo.png' %}" height="40px" />
</a>
</h1>
{% endblock %}

{% block nav-global %}
{% endblock %}
```

[Sadržaj](#sadržaj)

### Kako nadjačati Django admin šablone?

Na ovom linku je originalna dokumentacija:
<https://docs.djangoproject.com/en/dev/ref/contrib/admin/#overriding-admin-templates>

[Sadržaj](#sadržaj)

### Kako da postavimo prilagodjeni poredak aplikacija i modela?

Django podrazumevano redja modele u adminu alfabetski. Šablon korišćen za renderovanje admin index strane je `admin/index.html` i funkcija pogleda je `ModelAdmin.index`.

```py
def index(self, request, extra_context=None):
  """
  Display the main admin index page, which lists all of
  the installed apps that have been registered in this site.
  """
  app_list = self.get_app_list(request)
  context = {
    self.each_context(request),
    'title': self.index_title,
    'app_list': app_list,
    (extra_context or {}),
  }
  request.current_app = self.name

  return TemplateResponse(request, self.index_template or admin/index.html', context)
  ```

Metod `get_app_list`, postavlja poredak modela:

```py
def get_app_list(self, request):
  """
  Return a sorted list of all the installed apps that have
  been registered in this site.
  """
  app_dict = self._build_app_dict(request)
```

#### Sort the apps alphabeticall

```py
app_list = sorted(app_dict.values(), key=lambda x: x['name'].lower())
```

#### Sort the models alphabetically within each app

for app in app_list:

```py
app['models'].sort(key=lambda x: x['name'])
return app_list
```

Da bi imali prilagodjeni poredak, nadjačajmo `get_app_list`:

```py
class EventAdminSite(AdminSite):
  def get_app_list(self, request):
    """
    Return a sorted list of all the installed apps
    that have been registered in this site.
    """
    ordering = {
      "Event heros": 1, "Event villains": 2, "Epics": 3, "Events": 4
    }
    app_dict = self._build_app_dict(request) a.sort(key=lambda x: b.index(x[0]))

    # Sort the apps alphabetically.
    app_list = sorted(app_dict.values(), key=lambda x: x['name'].lower())

    # Sort the models alphabetically within each app.
    for app in app_list:
      app['models'].sort(key=lambda x: ordering[x['name']])
    
    return app_list
```

Kod:

```py
app['models'].sort(key=lambda x: ordering[x['name']])
```

postavlja željeni poredak.

[Sadržaj](#sadržaj)

### Kako dodati jedan model dva puta na admin?

Možete to uraditi, jednom kao regularan admin, i drugi put kao readonly admin. ( Neki korisnici će videti samo `read only` zbog dozvola. )

Ako pokušate da registrujete isti model dva puta:

```py
admin.site.register(Hero)
admin.site.register(Hero)
```

dobićete grešku:

```py
raise AlreadyRegistered('The model %s is already registered' % model. name )
```

Rešenje je naslediti `Hero` model kao `ProxyModel`:

`models.py`

```py
class HeroProxy(Hero):
  class Meta:
    proxy = True

  ...
```

`admin.py`

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
  list_display = ("name", "is_immortal", "category", "origin", "is_very_benevolent")
  ....

@admin.register(HeroProxy)
class HeroProxyAdmin(admin.ModelAdmin):
  readonly_fields = ("name", "is_immortal", "category", "origin", ...)
```

[Sadržaj](#sadržaj)

### Kako dodati db view na Django admin?

Kreirajmo db pogled:

```sql
create view entities_entity as
  select id, name from
    entities_hero union

select 10000+id as id, name from entities_villain
```

Kao rezultat dobićemo imena Hero i Vilian objekata. Id za Viliano objekte je povećan na 10000+id:

```sql
sqlite> select * from entities_entity;

1|Krishna
2|Vishnu
3|Achilles
4|Thor
5|Zeus
6|Athena
7|Apollo
10001 |Ravana
10002 |Fenrir
```

Tada dodajemo

```py
managed=False
```

na model:

```py
class AllEntity(models.Model):
  name = models.CharField(max_length=100)
  
  class Meta:
    managed = False
    db_table = "entities_entity"
```

I dodajemo u admin:

```py
@admin.register(AllEntity)
class AllEntiryAdmin(admin.ModelAdmin):
  list_display = ("id", "name")
```

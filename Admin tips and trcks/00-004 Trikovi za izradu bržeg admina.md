
# Trikovi za izradu bržeg admina

## Strani ključevi za uređivanje u list_editable

Ako imate strane ključeve na `list_editable` Django će napraviti 1 upit prema bazi podataka za svaku stavku na `listview`. Dosta za promenu 100 predmeta. Trik je da premestite `choices` za taj formfield:

```py
class MyAdmin(admin.ModelAdmin):
  list_editable = 'myfield',

  def formfield_for_dbfield(self, db_field, **kwargs):
    request = kwargs['request']
    formfield = super(MyAdmin, self).formfield_for_dbfield(db_field, **kwargs)
    
    if db_field.name == 'myfield':
      choices = getattr(request, '_myfield_choices_cache', None)
      if choices is None:
        request._myfield_choices_cache = choices = list(formfield.choices)
    
    formfield.choices = choices
    return formfield
```

## Strani ključevi ili ManyToMany polja u inline adminu

Ako imate FK ili M2M polja na InlineModelAdmin za svaki objekt u obrascu
dobićete pogodak baze podataka. To možete izbeći tako što ćete imati nešto slično:

```py
class MyAdmin(admin.TabularInline):
  fields = 'myfield',

  def formfield_for_dbfield(self, db_field, **kwargs):
    formfield = super(MyAdmin, self).formfield_for_dbfield(db_field,  
      **kwargs)
  
  if db_field.name == 'myfield':
    # dirty trick so queryset is evaluated and cached in .choices
    formfield.choices = formfield.choices
  
  return formfield
```

## Omogućite keširanje šablona

Neverovatno je koliko je lako zaboraviti da dodate u svoja podešavanja:

```py
TEMPLATE_LOADERS = (
  ('django.template.loaders.cached.Loader', (
  'django.template.loaders.filesystem.Loader',
  'django.template.loaders.app_directories.Loader',
  )),
)
```

## Koristite select_related za obrasce za uređivanje

U slučaju da imate neka polja za čitanje na formi za uređivanje i potrebni su srodni podaci. Da biste prikazali `listu_select_related` ne pomaže.

Npr.:

```py
class MyAdmin(admin.ModelAdmin):
  readonly_fields = 'myfield',

  def queryset(self, request):
    return super(MyAdmin, self).queryset(request).select_related('myfield')
```

## Koristite annotacije za funkcijske ulaze `list_display` umesto pravljenja dodatinih upita

Proverite API da biste videli da li možete da koristite ovo.
Evo tipičnog primera:

```py
class AuthorAdmin(admin.ModelAdmin):
  list_display = 'books_count',
  
  def books_count(self, obj):
    return obj.books_count
  
  def queryset(self, request):
    return super(AuthorAdmin, self).queryset(
      request).annotate(books_count=Count('books'))
```

Modeli bi izgledali ovako:

```py
class Author(models.Model):
  name = models.CharField(max_length=100)

class Book(models.Model):
  name = models.CharField(max_length=100)
  author = models.ForeignKey(Author, related_name="books")
```

## Keš memorija filtera iz promene admina

Ovo ima očigledan trostruko da ćete imati stale podatke na listi filtera, ali ako se ne promene, a oni različiti upiti ubijaju vašu bazu podataka, onda će to mnogo pomoći. Samo dodajte prilagođenu `change_list.html` formu u svom projektu (npr. `templates/Myappy/Change_list.html`):

```html
{% extends "admin/change_list.html" %}
{% load admin_list i18n cache %}

{% block filters %}
{% cache 300 admin_filters request.GET request.path request.user.username%}
{% if cl.has_filters %}
  <div id="changelist-filter">
    <h2>{% trans 'Filter' %}</h2>

{% for spec in cl.filter_specs %}
{% admin_list_filter cl spec %}
{%endfor %}
  </div>
{% endif %}
{% endcache %}

{% endblock %}
```

Prvobitno je primer imao zahtev.get.get.get.items unutra, ali to je pogrešno kao u Pajton 3 koji vraća generator. Oznaka šablona će koristiti
String predstavljanje svih argumenata tako da nema razloga
Bilo šta tamo.

## Bonus trick

```py
frame = sys._getframe(1)
while frame:
  if frame.f_code.co_name == 'render_change_form':
    if 'request' in frame.f_locals:
      request = frame.f_locals['request']
      break
      frame = frame.f_back
  else:
    raise RuntimeError("Could not find request object.")
    # do stuff with request

To bi se moglo koristiti u nekim određenim slučajevima (npr. Potreban vam je zahtev u metodi prikaza vidgeta), kao krajnje sredstvo naravno;


# Prevodjenje

[Sadržaj](00_sadrzaj.md)

Sa gore navedenim konfiguracijama, prevodi se sada mogu dodati na glavnu lokaciju! Koraci ispod prikazuju kako da dodate prevode posebno za admina. Ako ne postoji posebna potreba, koristite leni prevod za sve slučajeve.

## Gotove fraze

[Sadržaj](00_sadrzaj.md)

Admin stranice sajta automatski se generišu pomoću šablona sa puno konzerviranih fraza za stvari poput "login" i "save" i "delete". Kako da njih prevedete? Srećom, Django već ima prevode za mnoge glavne jezike. Pogledajte listu na: 

```html
django/contrib/admin/locale
```

za dostupne jezike. Django će automatski koristiti prevode za ove jezike na veb lokaciji Admin - nema šta drugo što treba da uradite! Ako vam je potreban jezik koji nije dostupan, snažno vas ohrabrujem da doprinesete novim prevodima na projektu Django tako da ih svi mogu deliti.

## Prilagođeni admin naslovi

[Sadržaj](00_sadrzaj.md)

Postoji nekoliko načina za postavljanje prilagođenih naslova Admin stranice. Moja poželjna metoda je da ih postavim u root `urls.py` datoteku. Gde god da su postavljeni, označite ih za lenji prevod. Lako ih je prevideti!

```py
from django.contrib import admin
from django.utils.translation import gettext_lazy as _

admin.site.index_title = _('My Index Title')
admin.site.site_header = _('My Site Administration')
admin.site.site_title = _('My Site Management')
```

## Imena aplikacija

[Sadržaj](00_sadrzaj.md)

Imena aplikacija su još jedan skup fraza koji se mogu lako propustiti. Dodajte polje `verbose_name` sa prevodnim stringom na svaku klasu `AppConfig` u projektu. Nemojte jednostavno pokušati prevesti string za polje `name`: Django će dati runtime izuzetak!

```py
from django.apps import AppConfig
from django.utils.translation import gettext_lazy as _
class CustomersConfig(AppConfig):

name = 'customers'
verbose_name = _('Customers')
```

## Imena modela

[Sadržaj](00_sadrzaj.md)

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

## Pokreni komande

[Sadržaj](00_sadrzaj.md)

Kada su svi stringovi markirani za prevodjenje, generiši datoteke poruka:

python manage.py makemessages -lzh_Hans

Zatim kompajliraj poruke

python manage.py compilemessages

Upozorenje:
Jezički kod i naziv lokala mogu biti različiti! Na primer, uzmite pojednostavljene kineske: Kod jezika je "ZH-
Hans", ali ime lokala je "zh_hans". Primjetite podvlaku i velika slova.

Imena lokala često uključuju šifru zemlje i različite jezičke nijanse, poput američkog engleskog vs.
britanskog engleskog. Pogledajte django/contrib/admin/local za listu primera.

## Bonus: Dugmad za jezike

[Sadržaj](00_sadrzaj.md)

Sa LocaleMiddleware i i18n_patterns, stranice trebaju biti automatski prevedene na osnovu konteksta ili prefiksa URL-a. Međutim, ipak bi bilo sjajno dozvoliti korisnikuručno da prebaci jezik iz admin interfejsa.
Klik na dugme je intuitivnije od rada sa prefiksima URL-a.

Postoji mnogo načina za dodavanje jezičnih preklopnika na veb lokaciju Admin. Za mene je najlakši način da dodate ikone zastave na naslovnu traku. Iza scene, svaka ikona zazastave bila bi povezana sa URL-om prefiks jezika za stranicu. Na taj način, kad god korisnik klikne na zastavu, tada je ista stranica učitana za željeni jezik.

### Prekidač prefiksa jezičkog koda

[Sadržaj](00_sadrzaj.md)

Pošto URL putevi koriste i18n_patterns, njihovi jezički kodovi mogu biti uniformni. Pomoćna funkcija može lako da doda ili zameni željeni jezik koda kao prefiks URL-u. Na primer, da pretvorimo "/admin/" i "/en/admin /" u "/zh-hans/admin/" za pojednostavljeni kineski. Ova funkcija takođe treba da potvrdi da su put i jezik tačni. Može se staviti bilo gde u projektu.

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

## Prekidač prefiksa šablonskog filtera

[Sadržaj](00_sadrzaj.md)

Konačno, ovu funkciju mora da zove Django šablon kako bi se pružile veze do stranica specifičnih za jezik. Dakle, potreban nam je prilagođeni filter šablona. Modul za implementaciju filtera može se staviti u bilo koju aplikaciju, ali mora biti u podpaketu po imenu templatetags - tako Django zna da traži oznake prilagođenih šablonai filtere. Novi filteri će biti lako pisati jer već imamo funkciju switch_lang_code. (Razdvajanje logike za rukovanje prefiksom iz samog filtra.)

Kod je ispod:

# [app]/templatetags/i18n_switcher.py
from django import template
from django.template.defaultfilters import stringfilter

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

## Nadjačavanje Admin šablona

[Sadržaj](00_sadrzaj.md)

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

Primjetite da je ceo userlinks blok morao da se prepisuje kako bi se na mesto postavilazastava. Statičke datoteke slika zastava su jednostavno besplatne emojis zastave. One su hiperlinkovane na odgovarajući jezik URL za stranicu: switch_18n primenjen je na objekt aktivnog zahteva da biste dobili željeni jezik - prefiksovan put. (Napomena: U mom primeru sam uklonio vezu "View site" jer je moj sajt nije trebao.)

## Završni prikaz


U mom projektu, ja sam izabrao da postavim jezički prefiks prekidač kod u posebnu aplikaciju i18n_switcher. Fajlovi u mom projektu potrebni za admin jezičku dugmad su oraganizaovani na sledeći način:

Fajlovi u mom projektu potrebni za admin jezičku dugmad su oraganizaovani na sledeći način:

```shell
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
```

Pošto sam kreirao novu aplikaciju za novi kod, takodje sam morao da dodam ime app u INSTALLED_APPS u `settings.py`:

U `settings.py`:

```py
INSTALLED_APPS = [
  ...
  'i18n_switcher.apps.I18NSwitcherConfig',
...
]
```

Kao što je već pomenuto, ikone zastava u naslovnoj traci su jedan od načina da se omoguće jednostavne veze do prevedenih stranica. Dobro funkcioniše kada postoji samo nekoliko dostupnih jezičkih izbora. Drugačiji pogled bio bi bolji za više jezika, poputpadajuće liste, druge linije u naslovnoj traci, ili čak i na footer-u stranice.

[Sadržaj][00_sadrzaj]
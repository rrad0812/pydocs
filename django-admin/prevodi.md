
# Prevodi

## Sadržaj

[Nazad](sadrzaj.md)

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

Django je fantastičan veb okvir Pithon-a, a jedna od njegovih sjajnih funkcija je internacionalizacija (ili
"i18n"). Prilično je lako dodati prevode na skoro bilo koji string u aplikaciji Django, ali šta je sa
prevođenjem Admin stranice sajta? Naslovi, imena i akcije - svima su potrebni prevodi. Te admin stranice
se automatski generišu, pa kako se njihove reči mogu prevesti?

Ovaj vodič vam pokazuje kako je to lako uraditi.

### I18N Pregled

Ako ste novi za prevode u Django, prvo pročitajte zvaničnu Translation stranicu. Ukratko, svi stringovi
kojima je potreban prevod treba da se prenesu u funkciju prevođenja za Pithon kod ili blok prevod za
Django šablonski kod. Django Managementkomande generišu datoteke poruka za specifične za jezik, u
kojima prevodioci dodajuprevode za obeležene stringove i konačno ih sastavljaju za upotrebu u aplikaciji.
Imajte na umu da prevodi zahtevaju da se gettext alati postave na vašu mašinu. Djangotakođe pruža neku
naprednu logiku za rukovanje posebnim slučajevima kao i formatima datuma i pluralizacijom.

### Početno podešavanje

Projektu Django je potrebno malo osnovne konfiguracije pre nego što je uradi prevode,
koji je potreban i za glavnu lokaciju i admin.

#### Omogućavanje internacionalizacije
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
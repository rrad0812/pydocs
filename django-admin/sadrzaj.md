
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

- [Polja za pretragu](polja_za_pretragu.md)
  - [Polja za pretragu na listview strani](polja_za_pretragu.md#polja-za-pretragu-na-listview-strani)
  - [DjangoQL](polja_za_pretragu.md#djangoql)
    - [Instalacija](polja_za_pretragu.md#instalacija)
    - [Korišćenje DjangoQL sa standardnim Django admin search-om](polja_za_pretragu.md#korišćenje-djangoql-sa-standardnim-django-admin-search-om)
    - [Jezičke reference](polja_za_pretragu.md#jezičke-reference)
    - [DjangoQL šema](polja_za_pretragu.md#djangoql-šema)
    - [Prilagodjena polja za pretragu](polja_za_pretragu.md#prilagodjena-polja-za-pretragu)
      - [Search po queryset annotacijama](polja_za_pretragu.md#search-po-queryset-annotacijama)
      - [Prilagođene suggest_options](polja_za_pretragu.md#prilagođene-suggest_options)
      - [Prilagodjeni search lookup](polja_za_pretragu.md#prilagodjeni-search-lookup)
      - [Puni prilagodjeni search lookup](polja_za_pretragu.md#puni-prilagodjeni-search-lookup)
      - [Mogu li koristiti Django QL van Django admina](polja_za_pretragu.md#mogu-li-koristiti-django-ql-van-django-admina)

- [Admin akcije](admin_akcije.md#)
  - [Pisanje akcija](admin_akcije.md#pisanje-akcija)
    - [Pisanje akcionih funkcija](admin_akcije.md#pisanje-akcionih-funkcija)
    - [Dodavanje akcija u ModelAdmin](admin_akcije.md#dodavanje-akcija-u-modeladmin)
    - [Rukovanje greškama u akcijama](admin_akcije.md#rukovanje-greškama-u-akcijama)
    - [Akcije kao ModelAdmin metode](admin_akcije.md#akcije-kao-modeladmin-metode)
  - [Omogućavanje-onemogućavanje akcija](admin_akcije.md#omogućavanje-onemogućavanje-akcija)
    - [Onemogućavanje akcije na celoj lokaciji](admin_akcije.md#onemogućavanje-akcije-na-celoj-lokaciji)
    - [Onemogućavanje svih akcija za određeni ModelAdmin](admin_akcije.md#onemogućavanje-svih-akcija-za-određeni-modeladmin)
    - [Uslovno omogućavanje ili onemogućavanje akcija](admin_akcije.md#uslovno-omogućavanje-ili-onemogućavanje-akcija)
  - [Postavljanje dozvola za akcije](admin_akcije.md#postavljanje-dozvola-za-akcije)
  - [Prilagodjene admin akcije sa prolaznom stranom](admin_akcije.md#prilagodjene-admin-akcije-sa-prolaznom-stranom)
    - [Dodavanje prolazne strane](admin_akcije.md#dodavanje-prolazne-strane)
    - [Dodavanje forme da izvrši našu akciju](admin_akcije.md#dodavanje-forme-da-izvrši-našu-akciju)
  - [Admin akcija bez prolazne stranom](admin_akcije.md#admin-akcija-bez-prolazne-strane)
  - [Kako dodati prilagodjenu akcijsku dugmad na listview stranu?](admin_akcije.md#kako-dodati-prilagodjenu-akcijsku-dugmad-na-listview-stranu)
    - [Prilagodjene akcije na pojedinačnim objektima](admin_akcije.md#prilagodjene-akcije-na-pojedinačnim-objektima)
    - [Prilagodjene akcije po objektu modela](admin_akcije.md#prilagodjene-akcije-po-objektu-modela)

- [Add/Change strane](add-change_strane.md)
  - [Kako prikazati sliku iz Imagefield na Django adminu?](add-change_strane.md#kako-prikazati-sliku-iz-imagefield-na-django-adminu)
  - [Kako povezati model sa trenutnim korisnikom koji upisuje/menja model?](add-change_strane.md#kako-povezati-model-sa-trenutnim-korisnikom-koji-upisujemenja-model)
  - [Kako označiti polje kao readonly u adminu?](add-change_strane.md#kako-označiti-polje-kao-readonly-u-adminu)
  - [Kako prikazati needitabilna polja u adminu?](add-change_strane.md#kako-prikazati-needitabilna-polja-u-adminu)
  - [Kako napraviti polje editabilnim kada se objekat kreira, inače readonly?](add-change_strane.md#kako-napraviti-polje-editabilnim-kada-se-objekat-kreira-inače-readonly)
  - [Kako filtrirati FK vrednosti iz padajućih lista u Django adminu?](add-change_strane.md#kako-filtrirati-fk-vrednosti-iz-padajućih-lista-u-django-adminu)
  - [Kako upravljati modelom sa FK vezom prema modelu sa velikim brojem objekata?](add-change_strane.md#kako-upravljati-modelom-sa-fk-vezom-prema-modelu-sa-velikim-brojem-objekata)
  - [Kako promeniti tekst na FK padajućoj listi?](add-change_strane.md#kako-promeniti-tekst-na-fk-padajućoj-listi)
  - [Kako dodati prilagodjeno dugme na Django changeview stranu?](add-change_strane.md#kako-dodati-prilagodjeno-dugme-na-django-changeview-stranu)
  - [Kako upravljati History/Model logovima u Django adminu?](add-change_strane.md#kako-upravljati-historymodel-logovima-u-django-adminu)
  - [Kako prilagoditi add/change formu modela?](add-change_strane.md#kako-prilagoditi-addchange-formu-modela)
  - [Kako nadjačati save ponašanje za admin?](add-change_strane.md#kako-nadjačati-save-ponašanje-za-django-admin)

- [Veze](veze.md)
  - [Povezana polja na admin listview strani](veze.md#povezana-polja-na-admin-listview-strani)
  - [Navigacija po povezanim poljima](veze.md#navigacija-po-povezanim-poljima)
    - [to_parent_link](veze.md#to_parent_link)
    - [to_child_link](veze.md#to_child_link)
  - [Pružanje veza do drugih stranica sa listama objekata modela](veze.md#pružanje-veza-do-drugih-stranica-sa-listama-objekata-modela)
  - [Reverzne veze](veze.md#reverzne-veze)
    - [related_name](veze.md#related_name)
    - [related_query_name](veze.md#related_query_name)
  - [Autokompletiranje za povezana polja](veze.md#autokompletiranje-za-povezana-polja)
  - [Hiperlinkovi](veze.md#hiperlinkovi)
  - [Kako dobiti admin urls za specifične objekte?](veze.md#kako-dobiti-admin-urls-za-specifične-objekte)

- [Inlines](inlines.md)
  - [Kako editovati višestruke modele iz jednog Django admina?](inlines.mdkako-editovati-višestruke-modele-iz-jednog-django-admina)
  - [Kako dodati OneToOne vezu kao inline?](inlines.mdkako-dodati-onetoone-vezu-kao-inline)
  - [Kako dodati ugnježdene inlines u Django adminu?](inlines.mdkako-dodati-ugnježdene-inlines-u-django-adminu)
  - [Kako kreirati jedan Django admin iz dva različita modela?](inlines.mdkako-kreirati-jedan-django-admin-iz-dva-različita-modela)
  - [Kako pristupiti parent instanci?](inlines.mdkako-pristupiti-parent-instanci)
  - [Kako dobiti objekt iz formfield_for_foreignkey?](inlines.mdkako-dobiti-objekt-iz-formfield_for_foreignkey)
  - [Pristup parent model instanci iz modelforme admin inline-a](inlines.mdpristup-parent-model-instanci-iz-modelforme-admin-inline-a)

- [Filtriranje](filtriranje.md)
  - [Kako dodati osnovno filtriranje u admin?](filtriranje.md#kako-dodati-osnovno-filtriranje-u-admin)
  - [Kako dodati prilagođeni filter u admin?](filtriranje.md#kako-dodati-prilagođeni-filter-u-admin)
  - [Kako ulančati filtere u adminu?](filtriranje.md#kako-ulančati-filtere-u-django-adminu)
  - [Kako dodati podrazumevani filter u admin?](filtriranje.md#kako-dodati-podrazumevani-filter-u-admin)
  - [Podrazumevani filteri - II način](filtriranje.md#podrazumevani-filteri---ii-način)
  - [Prilagodjeni tekst filteri](filtriranje.md#prilagodjeni-tekst-filteri)
  - [Prilagodjena lista filtera nad datumskim poljem](filtriranje.md#prilagodjena-lista-filtera-nad-datumskim-poljem)
  - [Napredni filteri](filtriranje.md#napredni-filteri)

- [Validacija prilagođene forme](validacija.md)
  - [Ugrađena validacija forme](validacija.md#ugrađena-validacija-forme)
  - [Validacija pojedinačnog polja](validacija.md#validacija-pojedinačnog-polja)
  - [Validacija više polja](validacija.md#validacija-više-polja)
  - [Zaključak](validacija.md#zaključak)

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

- Efikasna paginacija
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

- [Dozvole](dozvole.md)
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

- [Sigurnost admina](sigurnost_admina.md)
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

- [Prevodi](prevodi.md)
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


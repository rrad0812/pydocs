
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

Validacija Django prilagođene forme
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


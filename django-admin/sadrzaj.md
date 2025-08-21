
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

- [Djangove optimizacija QuerySet upita](optimizacija_queryset_upita.md)
  - [select_related](optimizacija_queryset_upita.md#select_related)
    - [*fields parametri](optimizacija_queryset_upita.md#fields-parametri)
    - [depth parameter](optimizacija_queryset_upita.md#depth-parameter)
    - [*parameters](optimizacija_queryset_upita.md#parameters)
    - [Zaključak select_related](optimizacija_queryset_upita.md#zaključak-select_related)
  - [prefetch_related](optimizacija_queryset_upita.md#prefetch_related)
    - [*lookups parameter](optimizacija_queryset_upita.md#lookups-parameter)
    - [Prefetch objekat](optimizacija_queryset_upita.md#prefetch-objekat)
    - [None](optimizacija_queryset_upita.md#none)
    - [Zaključak prefetch related](optimizacija_queryset_upita.md#zaključak-prefetch_related)
  - [prefetch_related vs selected_related](optimizacija_queryset_upita.md#prefetch_related-vs-selected_related)
    - [Zaključak prefetch_related vs selected_related](optimizacija_queryset_upita.md#zaključak-prefetch_related-vs-selected_related)
  - [Kombinovano korišćenje](optimizacija_queryset_upita.md#kombinovano-korišćenje)
  - [Prefetching](optimizacija_queryset_upita.md#prefetching)
    - [Šta je problem?](optimizacija_queryset_upita.md#šta-je-problem)
    - [prefetch_related 2](optimizacija_queryset_upita.md#prefetch_related-2)
    - [Uvod u Prefetch objekat](optimizacija_queryset_upita.md#uvod-u-prefetch-objekat)

- [Efikasna paginacija](paginacija.md)
  - [Razumevanje naivne paginacije](paginacija.md#razumevanje-naivne-paginacije)
  - [Predstava naivne paginacije](paginacija.md#predstava-naivne-paginacije)
  - [Rukovanje paginacijom u Djangu](paginacija.md#rukovanje-paginacijom-u-djangu)
    - [Uklanjanje Count upita](paginacija.md#uklanjanje-count-upita)
    - [Približno COUNT](paginacija.md#približno-count)
    - [Paginacija sa setovanjem ključa](paginacija.md#paginacija-sa-setovanjem-ključa)
  - [Korišćenje dodatka dj-pagination](paginacija.md#korišćenje-dodatka-dj-pagination)
  - [Zaključak paginacija](paginacija.md#zaključak-paginacija)

- [Kako skalirati admin](skaliranje_admina.md)
  - [Evidentiranje](skaliranje_admina.md#evidentiranje)
  - [N + 1 problem](skaliranje_admina.md#n--1-problem)
  - [Nadjačavanje get_queryset](skaliranje_admina.md#nadjačavanje-get_queryset)
  - [Poboljšani search sa DjangoQL](skaliranje_admina.md#poboljšani-search-sa-djangoql)
  - [Elminisanje COUNT upita](skaliranje_admina.md#elminisanje-count-upita)
  - [show_full_result_count](skaliranje_admina.md#show_full_result_count)
  - [deffer](skaliranje_admina.md#deffer)
  - [Paginator](skaliranje_admina.md#paginator)
  - [readonly_fields](skaliranje_admina.md#readonly_fields)
  - [Skaliranje admin date_hierarchy](skaliranje_admina.md#skaliranje-admin-date_hierarchy)
    - [Paket](skaliranje_admina.md#paket)
    - [date_hierarchy](skaliranje_admina.md#date_hierarchy)

- [JavaScript u adminu](javascript_u_adminu.md)
  - [Događaji u inline formi](javascript_u_adminu.md#događaji-u-inline-formi)
  - [Kako raditi sa AJAX Request u Django](javascript_u_adminu.md#kako-raditi-sa-ajax-request-u-django)
    - [Kako Ajax radi u Django](javascript_u_adminu.md#kako-ajax-radi-u-django)
  - [Pogled na Django](javascript_u_adminu.md#pogled-na-django)
    - [Korišćenje jQuery za AJAX u Django](javascript_u_adminu.md#korišćenje-jquery-za-ajax-u-django)
    - [Zašto koristiti jQuery](javascript_u_adminu.md#zašto-koristiti-jquery)
    - [Inicijalni setup](javascript_u_adminu.md#inicijalni-setup)
    - [AJAX Request implementacija](javascript_u_adminu.md#ajax-request-implementacija)
    - [Najbolje prakse za AJAX JavaScript u Django-u](javascript_u_adminu.md#najbolje-prakse-za-ajax-javascript-u-django-u)
  - [Ajax request u Django adminu na edit strani](javascript_u_adminu.md#ajax-request-u-django-adminu-na-edit-strani)
    - [Izgradnja Ajax request-a od nule u admin-u](javascript_u_adminu.md#izgradnja-ajax-request-a-od-nule-u-admin-u)

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

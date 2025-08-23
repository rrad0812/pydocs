
# DJANGO ADMIN KUVAR

Prošireno internet izdanje

RADOSAV RADOVANOVIĆ
Lazarevac, avgust 2025.

## Sadržaj

### Uvod

- [Prvi admin](0101_prvi_admin.md)
- [Modeli koji se koriste u ovom tutorijalu](0102_modeli.md)
  
### Generalno
  
- [Promena glavnih naslova](0201_promena_glavnih_naslova.md)
- [Naziv modela u množini](0202_Naziv_modela_u_množini.md)
- [Nezavisni sajtovi](0203_nezavisni_sajtovi.md)
- [Uklanjanje aplikacija iz admina](0204_uklanjanje_app_iz_admina.md)
- [Logo u adminu](0205_logo_u_adminu.md)
- [Nadjačavanje admin šablona](0206_nadjačavanje_admin_šablona.md)
- [Poredak aplikacija i modela](0207_poredak_aplikacija_i_modela.md)
- [Jedan model dva puta na adminu](0208_jedan_model_dva_puta_na_adminu.md)
- [Db view na adminu](0209_Db_view_na_adminu.md)
  
### List_View strane

- [Veliki broj zapisa](0301_veliki_broj_zapisa.md)
- [Ukidanje admin paginacije](0302_ukidanje_admin_paginacije.md)
- [Datumski bazirano filtriranje](0303_datumski_bazirano_filtriranje.md)
- [Unutrašnje veze](0304_unutrašnja_veze.md)
  
### Izračunata polja

- [Izračunata polja](0401_izračunata_polja.md)
- [Opitimizovanje upita sa annotacijama](0402_optimizovanje_upite_sa_annotacijama.md)
- [Sortiranje po izračunatim poljima](0403_sortiranje_po_izračunatim_poljima.md)
- [Filtriranje po izračunatim poljima](0404_filtriranje_po_izračunatim_poljima.md)
- [’on’ ili ’off’ ikone umesto izračunatih boolean polja](0405_on_ili_off_%20ikone_umesto_izračunatih_boolean_polja.md)
  
### Polja za pretragu

- [Polja za pretragu](0501_polja_za_pretragu.md)
- [DjangoQL](0502_djangoql.md)
  - [Korišćenje DjangoQL sa standardnim admin search](0503_korišćenje_sa_standardnim_admin_searchom.md)
  - [Jezičke reference](0504_jezičke_reference.md)
  - [DjangoQL šema](0505_djangql_šema.md)
  - [Prilagodjena polja za pretragu](0506_prilagodjena_polja_za_pretragu.md)
  - [Puni prilagodjeni search lookup](0507_puni_prilagodjeni_search_lookup.md)
  - [DjangoQL van admina](0508_djangoql_van%20admina.md)
  
### Admin akcije

- [Pisanje akcija](0601_pisanje_akcija.md)
  - [Pisanje akcionih funkcija](0602_pisanje_akcionih_funkcija.md)
  - [Dodavanje akcija u ModelAdmin](0603_dodavanje_akcija_u_ModelAdmin.md)
  - [Rukovanje greškama u akcijama](0604_rukovanje_greškama.md)
  - [Akcije kao ModelAdmin metode](0605_akcije_kao_ModelAdmin_metode.md)
- [Omogućavanje-onemogućavanje akcija](0606_omogućavanje-onemogućavanje_akcija.md)
  - [Onemogućavanje akcije na celoj lokaciji](0607_onemogućavanje_akcija_na_celoj_lokaciji.md)
  - [Onemogućavanje svih akcija za ModelAdmin](0608_onemogućavanje_svih_akcija%20za_Model_Admin.md)
  - [Uslovno omogućavanje-onemogućavanje akcija](0609_uslovno_omogućavanje-onemogućavanje_akcija.md)
- [Dozvole za akcije](0610_dozvole%20za%20akcije.md)
- [Prilagodjene admin akcije](0611_prilagodjene_admin_akcije.md)
- [Prilagodjene admin akcije sa prolaznom stranom i formom](0612_prolazna_strana_i_forme.md)
- [Prilagodjene akcije bez prolazne strane](0613_prilagodjene_admin_akcije_bez_prolazne_strane.md)
- [Prilagodjena akcijska dugmad](0614_prilagodjena_akcijska_dugmad.md)
  - [Prilagodjene akcije na pojedinačnim objektima](0615_prilagodjene_akcije_na_pojedinačnim_objektima.md)
  - [Prilagodjene akcije po objektu modela](0616_prilagodjene_akcije_po_objektu_modela.md)

### Add/Change strane

- [Prikazivanje slike na admin strani](0701_prikazivanje_slike_na_admin_strani.md)
- [Povezivanje modela sa trenutnim korisnikom](0702_povezivanje%20modela_sa_trenutnim_korisnikom.md)
- [Označavanje polje kao readonly](0703_označavanje_polja_kao_readonly.md)
- [Needitabilna polja](0704_needitabilna_polja.md)
- [Editabilno polje inače readonly](0705_editabilno_polje_inače_readonly.md)
- [Filtriranje FK vrednosti](0706_filtriranje_FK_vrednosti.md)
- [Upravljanje velikim FK modelom](0707_Upravljanje_velikim_FK_modelom.md)
- [Promena teksta na FK padajućoj listi](0708_promena_teksta%20na%20FK_padajućoj_listi.md)
- [Prilagodjeno dugme na changeview strani](0709_dodavanje_prilagodjenog_dugmeta.md)
- [Upravljanje History/Model logovima](0710_Upravljanje_History-Model%20logovima.md)
- [Prilagodjavanje add/change forme modela](0711_prilagodjenje_add-change_forme_modela.md)
- [Nadjačavanje save ponašanja za admin](0712_nadjačavanje_save_ponašanja%20.md)
  
### Veze

- [Povezana polja na admin listview strani](0801_povezana_polja_na_listview_strani.md)
- [Navigacija po povezanim poljima](0802_navigacija_po_povezanim_poljima.md)
- [Veze do drugih strana sa listama objekata modela](0803_veze%20do%20drugih_strana%20sa%20listama_objekata.md)
- [Reverzne veze](0804_reverzne_veze.md)
- [Autokompletiranje za povezana polja](0805_autokompletiranje%20za%20povezana_polja.md)
- [Hiperlinkovi](0806_Hiperlinkovi.md)
- [Kako dobiti admin urls za specifične objekte?](0807_dobijanje_admin_url_za%20specifične%20objekte.md)
  
### Inlines
  
- [Editovanje višestrukih modela](0901_editovanje_višestrukih_modela.md)
- [Dodavanje OneToOne veze kao inline](0902_editovanje_onetoone_veze_kao_inline.md)
- [Dodavanje ugnježdenih inlines](0903_dodavanje_ugnježdjenih_inlines.md)
- [Pristup parent instanci iz inlines](0904_pristup_parent_instanci_iz_inlines.md)
- [Jedan admin iz dva različita modela](0905_jedan_admin_iz_dva_različita_modela.md)
- [Kako dobiti objekt iz formfield_for_foreignkey?](0906_dobijanje_objekta_iz_formfield_for_foreignkey.md)
- [Pristup parent model instanci iz inline modelforme](0907_Pristup_parent_model_instanci_iz_inline_model_forme.md)

### Filtriranje

- [Modeli](1001_modeli.md)
- [Osnovno filtriranje u adminu](1002_osnovno_filtriranje_u_adminu.md)
- [Prilagodjeni filteri](1003_prilagodjeni_filter.md)
- [Ulančavanje filtera u admin](1004_ulančavanje_filtera.md)
- [Dodavanje podrazumevanog filtera](1005_dodavanje_podrazumevanog_filtera.md)
- [Prilagodjeni tekst filteri](1006_prilagodjeni_tekst_filteri.md)
- [Prilagodjene liste filtera nad datumskim poljima](1007_prilagodjene_liste_filtera_nad_datumskim_poljem.md)
- [Napredni filteri](1008_napredni_filteri.md)
  
### Validacija prilagođene forme
  
- [Ugrađena validacija forme](1101_ugradjena_validacija_forme.md)
- [Validacija pojedinačnog polja](1102_Validacija_pojedinačnog_polja.md)
- [Validacija više polja](1103_validacija_više_polja.md)
- [Završno o validaciji](1104_završno%20o%20validaciji.md)
  
### Optimizacija QuerySet upita

- [Modeli](1201_Modeli.md)
- [select_related](1202_select_related.md)
- [prefetch_related](1203_prefetch_related.md)
- [prefetch_related vs select_related](1204_prefetch_related_vs_select_related.md)
- [Kombinovano korišćenje](1205_kombinovano_korišćenje.md)
- [Prefetching](1206_prefetching.md)
- [Prefetch objekat](1207_prefetch_objekat.md)
  
### Efikasna paginacija

- [Naivna paginacijaja](1301_naivna_paginacija.md)
  - [Uklanjanje Count upita](1302_uklanjanje_count_upita.md)
  - [Približno COUNT](1303_prbližno_count.md)
  - [Paginacija sa skupom ključeva](1304_paginacija_sa_skupom_ključeva.md)
  - [Dodatak dj-pagination](1305_dodatak_dj-pagination.md)
- [Zaključak paginacija](1306_završno_o_paginaciji.md)
  
### Skaliranje admina

- [Skaliranje admina](1401_skaliranje_admina.md)
  - [Nadjačavanje get_queryset](1201_Modeli.md)
  - [Poboljšani search sa DjangoQL](1403_poboljšani_search_sa%20DjangoQL.md)
  - [Elminisanje COUNT upita](1404_eleiminisanje_count_upita.md.md)
  - [show_full_result_count](1405_show_full_result_count.md)
  - [deffer](1406_deffer.md)
  - [Paginator](1407_paginator.md)
  - [readonly_fields](1408_readonly_fields.md)
  - [Skaliranje admin date_hierarchy](1409_skaliranje_date_hierarchy.md)
  - [Dodatak django-admin-lightweight-date-hierarchy](1410_dodatak_django-admin-lightweight-date-hierarchy.md)
  - [date_hierarchy index](1411_date_hierarchy_index.md)
  
### JavaScript u adminu
  
### CSV export-import
  
### Dozvole
  
### Prevodi

### Sigurnost admina

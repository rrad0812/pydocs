# Baze podataka

Django zvanično podržava sledeće baze podataka:

- `PostgreSQL`
- `MarijaDB`
- `MySQL`
- `Oracle`
- `SQLite`

Takođe postoji i niz bekendova baza podataka koje pružaju treće strane.

Django pokušava da podrži što više funkcija na svim bekendovima baza podataka. Međutim, nisu svi bekendovi baza podataka isti i morali smo da donosimo dizajnerske odluke o tome koje funkcije da podržimo i koje pretpostavke možemo bezbedno da napravimo.

Ova datoteka opisuje neke od funkcija koje mogu biti relevantne za korišćenje Django-a. Nije namenjena kao zamena za dokumentaciju ili referentne priručnike specifične za server.

## Sadržaj

[Opšte napomene](#opšte-napomene)

- [Trajne veze](#trajne-veze)
  - [Upravljanje vezama](#upravljanje-vezama)
- [Kodiranje](#kodiranje)

[PostgreSQL beleške](#postgresql-beleške)

- [Podešavanja konekcije sa PostgreSQL](#podešavanja-konekcije-sa-postgresql)
- [Optimizacija konfiguracije PostgreSQL](#optimizacija-konfiguracije-postgresql)
- [Nivo izolacije PostgreSQL](#nivo-izolacije-postgresql)
- [Uloge](#uloge)
- [Parametri povezivanja na strani servera](#parametri-povezivanja-na-strani-servera)
- [Indeksi za varchar i text kolone](#indeksi-za-varchar-i-text-kolone)
- [Operacija migracije za dodavanje ekstenzija](#operacija-migracije-za-dodavanje-ekstenzija)
- [Kursori na strani servera](#kursori-na-strani-servera)
  - [Objedinjavanje transakcija i kursora na strani servera](#objedinjavanje-transakcija-i-kursora-na-strani-servera)
- [Ručno određivanje vrednosti automatski inkrementirajućih primarnih ključeva](#ručno-određivanje-vrednosti-automatski-inkrementirajućih-primarnih-ključeva)
- [Šabloni za testiranje baze podataka](#šabloni-za-testiranje-baze-podataka)
- [Ubrzavanje izvršavanja testa sa netrajnim podešavanjima](#ubrzavanje-izvršavanja-testa-sa-netrajnim-podešavanjima)

[MariaDB beleške](#mariadb-beleške)

[MySQL beleške](#mysql-beleške)

- [Podrška za verzije](#podrška-za-verzije)
- [Motori za skladištenje](#motori-za-skladištenje)
- [MySQL DB API drajveri](#mysql-db-api-drajveri)
  - [mysqlclient](#mysqlclient)
  - [MySQL konektor/Pajton](#mysql-konektorpajton)
- [Definicije vremenskih zona](#definicije-vremenskih-zona)
- [Kreiranje baze podataka](#kreiranje-baze-podataka)
  - [Podešavanja ređanja](#podešavanja-ređanja)
- [Povezivanje sa MySQL bazom podataka](#povezivanje-sa-mysql-bazom-podataka)
  - [Podešavanje sql_mode](#podešavanje-sql_mode)
  - [Nivo izolacije MySQL](#nivo-izolacije-mysql)
- [Kreiranje tabela](#kreiranje-tabela)
- [Imena tabela](#imena-tabela)
- [SAVEPOINTS](#savepoints)
- [Napomene o određenim poljima](#napomene-o-određenim-poljima)
  - [Polja za karaktere](#polja-za-karaktere)
  - [`TextField` ograničenja](#textfield-ograničenja)
  - [Podrška za razlomke sekundi za polja `Time` i `Datetime`](#podrška-za-razlomke-sekundi-za-polja-time-i-datetime)
  - [Zaključavanje redova pomoću `QuerySet.select_for_update()`](#zaključavanje-redova-pomoću-querysetselect_for_update)
- [Automatsko pretvaranje tipova može prouzrokovati neočekivane rezultate](#automatsko-pretvaranje-tipova-može-prouzrokovati-neočekivane-rezultate)

[SQLite beleške](#sqlite-beleške)

- [Podudaranje podstringova i osetljivost na velika i mala slova](#podudaranje-podstringova-i-osetljivost-na-velika-i-mala-slova)
- [Decimalna obrada](#decimalna-obrada)
- [Greške "Baza podataka je zaključana"](#greške-baza-podataka-je-zaključana)
- [QuerySet.select_for_update() nije podržano](#querysetselect_for_update-nije-podržano)
- [Izolacija pri korišćenju QuerySet.iterator()](#izolacija-pri-korišćenju-querysetiterator)
- [Omogućavanje JSON1 ekstenzije na SQLite](#omogućavanje-json1-ekstenzije-na-sqlite)

[Oracle beleške](#oracle-beleške)

- [Povezivanje sa Oracle bazom podataka](#povezivanje-sa-oracle-bazom-podataka)
  - [Kompletan DSN i jednostavno povezivanje](#kompletan-dsn-i-jednostavno-povezivanje)
- [Opcija sa nitima](#opcija-sa-nitima)
- [INSERT … RETURNING INTO](#insert--returning-into)
- [Problemi sa imenovanjem](#problemi-sa-imenovanjem)
- [NULL i prazni stringovi](#null-i-prazni-stringovi)

[Podklasiranje ugrađenih bekendova baze podataka](#podklasiranje-ugrađenih-bekendova-baze-podataka)

[Korišćenje baze podataka treće strane](#korišćenje-baze-podataka-treće-strane)

## Opšte napomene  

[Na vrh](#baze-podataka)

### Trajne veze

[Na vrh](#baze-podataka)

Trajne veze izbegavaju troškove ponovnog uspostavljanja veze sa bazom podataka u svakom HTTP zahtevu. Njima upravlja CONN_MAX_AGEparametar koji definiše maksimalni vek trajanja veze. Može se podesiti nezavisno za svaku bazu podataka.

Podrazumevana vrednost je 0, čime se čuva istorijsko ponašanje zatvaranja veze sa bazom podataka na kraju svakog zahteva. Da biste omogućili trajne veze, podesite `CONN_MAX_AGE` na pozitivan ceo broj sekundi. Za neograničene trajne veze, podesite na `None`.

### Upravljanje vezama

[Na vrh](#baze-podataka)

Django otvara vezu sa bazom podataka kada prvi put napravi upit ka bazi podataka. Ovu vezu drži otvorenom i ponovo je koristi u narednim zahtevima. Django zatvara vezu kada ona pređe maksimalnu starost definisanu od `CONN_MAX_AGE` ili kada više nije upotrebljiva.

> Detaljnije, Django automatski otvara vezu sa bazom podataka kad god mu je  potrebna, a već je nema — bilo zato što je ovo prva veza ili zato što je prethodna veza bila zatvorena.

Na početku svakog zahteva, Django zatvara vezu ako je dostigla maksimalnu starost. Ako vaša baza podataka prekida neaktivne veze nakon nekog vremena, trebalo bi da podesite `CONN_MAX_AGE` na nižu vrednost, kako Django ne bi pokušavao da koristi vezu koju je prekinuo server baze podataka.

> Ovaj problem može uticati samo na sajtove sa veoma malim prometom.

Na kraju svakog zahteva, Django zatvara vezu ako je dostigla maksimalnu starost ili ako je u stanju nepopravljive greške. Ako je došlo do grešaka u bazi podataka tokom obrade zahteva, Django proverava da li veza i dalje radi i zatvara je ako ne radi.

> Dakle, greške u bazi podataka utiču na najviše jedan zahtev po radnoj niti svake aplikacije; ako veza postane neupotrebljiva, sledeći zahtev dobija novu vezu.

Podešavanje `CONN_HEALTH_CHECKS` na True može se koristiti za poboljšanje robusnosti ponovne upotrebe veze i sprečavanje grešaka kada je veza zatvorena od strane servera baze podataka koji je sada spreman da prihvati i opslužuje nove veze, npr. nakon ponovnog pokretanja servera baze podataka.

> Provera ispravnosti se vrši samo jednom po zahtevu i samo ako se bazi podataka pristupa tokom obrade zahteva.

> [!Note]
>
> **Promenjeno u Django 4.1**: </br>
> Podešavanje `CONN_HEALTH_CHECKS` je dodato.

> [!Warning]
>
> Pošto svaka nit održava sopstvenu vezu, vaša baza podataka mora da podržava barem onoliko istovremenih veza koliko imate radnih niti.

Ponekad većina vaših prikaza neće moći da pristupi bazi podataka, na primer zato što je u pitanju baza podataka eksternog sistema ili zbog keširanja. U takvim slučajevima, trebalo bi da podesite `CONN_MAX_AGE` na nisku vrednost ili čak 0, jer nema smisla održavati vezu koja verovatno neće biti ponovo korišćena. Ovo će pomoći da se broj istovremenih veza sa ovom bazom podataka održi malim.

> Razvojni server kreira novu nit za svaki zahtev koji obrađuje, čime se poništava efekat trajnih veza. Nemojte ih omogućavati tokom razvoja.

Kada Django uspostavi vezu sa bazom podataka, on podešava odgovarajuće parametre, u zavisnosti od korišćenog bekenda. Ako omogućite trajne veze, ovo podešavanje se više ne ponavlja pri svakom zahtevu. Ako izmenite parametre kao što su nivo izolacije veze ili vremenska zona, trebalo bi da ili

- vratite podrazumevane vrednosti Django-a na kraju svakog zahteva,
- nametnete odgovarajuću vrednost na početku svakog zahteva ili
- onemogućite trajne veze.

Ako se veza kreira u dugotrajnom procesu, van Django-ovog ciklusa zahteva i odgovora, veza će ostati otvorena dok se eksplicitno ne zatvori ili dok ne istekne vreme.

### Kodiranje

[Na vrh](#baze-podataka)

Django pretpostavlja da sve baze podataka koriste UTF-8 kodiranje. Korišćenje drugih kodiranja može dovesti do neočekivanog ponašanja, kao što su greške "vrednost je predugačka" u vašoj bazi podataka za podatke koji su validni u Django-u. Pogledajte napomene specifične za bazu podataka u nastavku za informacije o tome kako da pravilno podesite bazu podataka.

## PostgreSQL beleške

[Na vrh](#baze-podataka)

Django podržava `PostgreSQL 12` i novije verzije. Potreban je `psycopg 3.1.8+` ili `psycopg2 2.8.4+`, mada se preporučuje najnoviji psycopg `3.1.8+`.

> [!Note]
>
> Podrška za psycopg2će verovatno biti zastarela i uklonjena u nekom trenutku u budućnosti.

> [!Note]
>
> **Promenjeno u Django 4.2:**  Dodata je psycopg podrška za verziju 3.1.8+.

### Podešavanja konekcije sa PostgreSQL

[Na vrh](#baze-podataka)

Videti `HOST` za detalje.

Da biste se povezali koristeći ime servisa iz datoteke servisa za povezivanje i lozinku iz datoteke sa lozinkama , morate ih navesti u OPTIONSdelu konfiguracije baze podataka u DATABASES:

`settings.py`

```py
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "OPTIONS": {
            "service": "my_service",
            "passfile": ".my_pgpass",
        },
    }
}
```

`.pg_service.conf`

```py
[my_service]
host=localhost
user=USER
dbname=NAME
port=5432
```

`.my_pgpass`

```py
localhost:5432:NAME:USER:PASSWORD
```

> [!Warning]
>
> Korišćenje imena servisa u svrhu testiranja nije podržano. Ovo može biti implementirano kasnije.

### Optimizacija konfiguracije PostgreSQL

[Na vrh](#baze-podataka)

Djangu su potrebni sledeći parametri za konekcije sa bazom podataka:

- `client_encoding`: `'UTF8'`,
- `default_transaction_isolation`: podrazumevano ili vrednost podešena u opcijama povezivanja (videti dole), `'read committed'`

- `timezone`:
    kada `USE_TZ` je `True`, `'UTC'` podrazumevano, ili `TIME_ZONE` vrednost podešena za vezu,
    kada `USE_TZ` je `False`, vrednost globalnog `TIME_ZONE` podešavanja.

Ako ovi parametri već imaju ispravne vrednosti, Django ih neće postavljati za svaku novu vezu, što malo poboljšava performanse. Možete ih konfigurisati direktno u bazi podataka `postgresql.conf` ili, što je praktičnije, za svakog korisnika baze podataka pomoću `ALTER ROLE`.

Django će raditi sasvim dobro i bez ove optimizacije, ali svaka nova veza će izvršiti neke dodatne upite da bi podesila ove parametre.

### Nivo izolacije PostgreSQL

[Na vrh](#baze-podataka)

Kao i sam PostgreSQL, Django podrazumevano koristi `READ COMMITTED` nivo izolacije. Ako vam je potreban viši nivo izolacije, kao što je `REPEATABLE` ili `READSERIALIZABLE`, podesite ga u delu `OPTIONS` konfiguracije vaše baze podataka u `DATABASES`:  

```py
from django.db.backends.postgresql.psycopg_any import IsolationLevel

DATABASES = {
    # ...
    "OPTIONS": {
        "isolation_level": IsolationLevel.SERIALIZABLE,
    },
}
```

> [!Note]
> 
> Pod višim nivoima izolacije, vaša aplikacija bi trebalo da bude spremna da obrađuje izuzetke nastale prilikom grešaka serijalizacije. Ova opcija je namenjena za naprednu upotrebu.

> [!Note]
>
> **Promenjeno u Django 4.2:** </br>
> IsolationLevel je dodato.

### Uloge

[Na vrh](#baze-podataka)

> [!Note]
>
> **Novo u Django 4.2.**

Ako vam je potrebno da koristite drugačiju ulogu za povezivanje sa bazom podataka od one koja se koristi za uspostavljanje veze, podesite je u `OPTIONS` delu konfiguracije baze podataka u `DATABASES`:

```py
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        # ...
        "OPTIONS": {
            "assume_role": "my_application_role",
        },
    },
}
```

### Parametri povezivanja na strani servera

[Na vrh](#baze-podataka)

> [!Note]
>
> **Novo u Django 4.2.**

Sa `psycopg 3.1.8+`, Django podrazumevano koristi povezivanje kursora na strani klijenta. Ako želite da koristite povezivanje na strani servera, podesite ga u `OPTIONS` delu konfiguracije vaše baze podataka u `DATABASES`:

```py
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        # ...
        "OPTIONS": {
            "server_side_binding": True,
        },
    },
}
```

> Ova opcija se ignoriše sa psycopg2.

### Indeksi za varchar i text kolone

[Na vrh](#baze-podataka)

Kada navodite `db_index=True` u poljima vašeg modela, Django obično izbacuje jednu `CREATE INDEX` naredbu. Međutim, ako je tip baze podataka za polje bilo `varchar` or `text` (npr. koristi ga `CharField`, `FileField` i `TextField`), onda će Django kreirati dodatni indeks koji koristi odgovarajuću `PostgreSQL klasu operatora` za kolonu. Dodatni indeks je neophodan za ispravno izvršavanje pretraga koje koriste operator `LIKE` u svom SQL-u, kao što je to urađeno sa tipovima pretraga `contains` i `startswith`.

### Operacija migracije za dodavanje ekstenzija

[Na vrh](#baze-podataka)

Ako treba da dodate `PostgreSQL` ekstenziju (kao što su `hstore`, `postgis`, itd.) koristeći migraciju, koristite `CreateExtension` operaciju.

### Kursori na strani servera

[Na vrh](#baze-podataka)

Kada se koristi `QuerySet.iterator()`, Django otvara kursor na strani servera. Podrazumevano, PostgreSQL pretpostavlja da će biti preuzeto samo prvih 10% rezultata upita kursora. Planer upita troši manje vremena na planiranje upita i počinje brže da vraća rezultate, ali to može smanjiti performanse ako se preuzme više od 10% rezultata. PostgreSQL-ove pretpostavke o broju redova preuzetih za upit kursora kontrolišu se opcijom `cursor_tuple_fraction`.

#### Objedinjavanje transakcija i kursora na strani servera

[Na vrh](#baze-podataka)

Korišćenje objedinjavača konekcija u režimu objedinjavanja transakcija (npr. PgBouncer) zahteva onemogućavanje kursora na strani servera za tu konekciju.

Kursori na strani servera su lokalni za vezu i ostaju otvoreni na kraju transakcije kada je `AUTOCOMMIT` `True`. Naknadna transakcija može pokušati da preuzme više rezultata sa kursora na strani servera.

> U režimu objedinjavanja transakcija, ne postoji garancija da će naredne transakcije koristiti istu vezu.

Ako se koristi druga veza, javlja se greška kada transakcija referencira kursor na strani servera, jer su kursori na strani servera dostupni samo u vezi u kojoj su kreirani.

- Jedno rešenje je onemogućavanje kursora na strani servera za vezu u `DATABASES` podešavanjem `DISABLE_SERVER_SIDE_CURSORS` na `True`.

- Da biste iskoristili prednosti kursora na strani servera u režimu objedinjavanja transakcija, možete podesiti drugu vezu sa bazom podataka kako biste izvršavali upite koji koriste kursore na strani servera. Ova veza mora biti ili direktno sa bazom podataka ili sa objedinjavačem veza u režimu objedinjavanja sesija.

- Druga opcija je da se svaki `QuerySet` kursor na strani servera umota u `atomic()` blok, jer se onemogućava `autocommit` tokom trajanja transakcije. Na ovaj način, kursor na strani servera će biti aktivan samo tokom trajanja transakcije.

### Ručno određivanje vrednosti automatski inkrementirajućih primarnih ključeva

[Na vrh](#baze-podataka)

Django koristi `PostgreSQL` kolone `identiteta` za čuvanje `automatski inkrementirajućih primarnih ključeva`. Kolona identiteta se popunjava vrednostima iz sekvence koja prati sledeću dostupnu vrednost. Ručno dodeljivanje vrednosti automatski inkrementirajućem polju ne ažurira sekvencu polja, što kasnije može izazvati sukob. Na primer:

```py
>>> from django.contrib.auth.models import User
>>> User.objects.create(username="alice", pk=1)
<User: alice>

>>> # The sequence hasn't been updated; its next value is 1.
>>> User.objects.create(username="bob")
IntegrityError: duplicate key value violates unique constraint
"auth_user_pkey" DETAIL:  Key (id)=(1) already exists.
```

Ako je potrebno da navedete takve vrednosti, resetujte sekvencu nakon toga da biste izbegli ponovno korišćenje vrednosti koja je već u tabeli.

`sqlsequencereset` komanda za upravljanje generiše SQL izraze za to.

> [!Note]
>
> **Promenjeno u Django 4.1:** </br>
> U starijim verzijama, `SERIAL` umesto kolona  `identiteta` tip podatka je korišćen.

### Šabloni za testiranje baze podataka

[Na vrh](#baze-podataka)

Možete koristiti TEST['TEMPLATE'] podešavanje da biste odredili šablon (npr. 'template0') iz kojeg će se kreirati testna baza podataka.

### Ubrzavanje izvršavanja testa sa netrajnim podešavanjima

[Na vrh](#baze-podataka)

Možete ubrzati vreme izvršavanja testova tako što ćete konfigurisati PostgreSQL da bude netrajan.

> [!Warning]
>
> Ovo je opasno: učiniće vašu bazu podataka podložnijom gubitku podataka ili oštećenju u slučaju pada servera ili nestanka struje. Koristite ovo samo na razvojnoj mašini gde možete lako da vratite ceo sadržaj svih baza podataka u klasteru.

## MariaDB beleške

[Na vrh](#baze-podataka)

Django podržava `MariaDB 10.4` i novije verzije.

Da biste koristili `MariaDB`, koristite `MySQL` bekend, koji dele dva sistema. Pogledajte `MySQL beleške` za više detalja.

## MySQL beleške

[Na vrh](#baze-podataka)

### Podrška za verzije

[Na vrh](#baze-podataka)

Django podržava `MySQL 8` i novije verzije.

Djangova `inspectdb` funkcija koristi `information_schema` bazu podataka koja sadrži detaljne podatke o svim šemama baze podataka.

Django očekuje da baza podataka podržava Unicode (UTF-8 kodiranje) i delegira joj zadatak sprovođenja transakcija i referencijalnog integriteta. Važno je biti svestan činjenice da `MySQL` zapravo ne sprovodi ova dva poslednja kada se koristi `MyISAM` mehanizam za skladištenje podataka, pogledajte sledeći odeljak.

### Motori za skladištenje

[Na vrh](#baze-podataka)

`MySQL` ima nekoliko mehanizama za skladištenje. Možete promeniti podrazumevani mehanizam za skladištenje u konfiguraciji servera.

MySQL-ov podrazumevani mehanizam za skladištenje je `InnoDB`. Ovaj mehanizam je potpuno `transakcioni` i podržava `reference stranih ključeva`. To je preporučeni izbor. Međutim, brojač automatskog povećanja InnoDB-a se gubi pri ponovnom pokretanju MySQL-a jer ne pamti vrednost `AUTO_INCREMENT`, već je ponovo kreira kao `max(id)+1`. Ovo može dovesti do nenamerne ponovne upotrebe `AutoField` vrednosti.

Glavni nedostaci `MyISAM` su to što ne podržava `transakcije` niti primenjuje `ograničenja stranog ključa`.

### MySQL DB API drajveri

[Na vrh](#baze-podataka)

MySQL ima nekoliko drajvera koji implementiraju Python Database API opisan uPEP 249 :

- mysqlclient je izvorni drajver. To je preporučeni izbor.

- MySQL Connector/Python je čisti Python drajver od kompanije Oracle koji ne zahteva MySQL klijentsku biblioteku ili bilo koje Python module izvan standardne biblioteke.

Ovi drajveri su bezbedni za više niti i pružaju objedinjavanje konekcija.

Pored drajvera za DB API, Django-u je potreban adapter za pristup drajverima baze podataka iz svog ORM-a. Django pruža adapter za mysqlclient, dok MySQL Connector/Python uključuje sopstveni mysqlclient.

#### mysqlclient

Django zahteva `mysqlclient 1.4.3` ili noviji.

#### MySQL konektor/Pajton

[Na vrh](#baze-podataka)

MySQL konektor/Python je dostupan sa stranice za preuzimanje. Django adapter je dostupan u verzijama `1.1.x` i novijim. Možda ne podržava najnovija izdanja Django-a.

### Definicije vremenskih zona

[Na vrh](#baze-podataka)

Ako planirate da koristite Django-ovu podršku za vremenske zone , koristite mysql_tzinfo_to_sql da biste učitali tabele vremenskih zona u MySQL bazu podataka. Ovo je potrebno uraditi samo jednom za vaš MySQL server, a ne za svaku bazu podataka.

### Kreiranje baze podataka

[Na vrh](#baze-podataka)

Možete kreirati bazu podataka pomoću alata komandne linije i ovog SQL-a:

```sql
CREATE DATABASE <dbname> CHARACTER SET utf8;
```

Ovo osigurava da će sve tabele i kolone podrazumevano koristiti UTF-8.

### Podešavanja ređanja

[Na vrh](#baze-podataka)

Podešavanje kolacije za kolonu kontroliše redosled kojim se podaci sortiraju, kao i koji se stringovi upoređuju kao jednaki. Možete navesti `db_collation` parametar da biste podesili ime kolacije kolone za `CharField` i `TextField`.

Kolacija se takođe može podesiti na nivou cele baze podataka i za svaku tabelu. Ovo je detaljno dokumentovano u MySQL dokumentaciji. U takvim slučajevima, morate podesiti kolaciju direktnom manipulacijom podešavanjima baze podataka ili tabela. Django ne pruža API za njihovu promenu.

Podrazumevano, sa UTF-8 bazom podataka, MySQL će koristiti `utf8_general_ci` kolaciju. Ovo rezultira time da se sva poređenja jednakosti stringova vrše bez razlikovanja velikih i malih slova. To jest, "Fred" i "freD" se smatraju jednakim na nivou baze podataka. Ako imate jedinstveno ograničenje za polje, bilo bi nezakonito pokušati umetnuti i "aa"i "AA"u istu kolonu, jer se oni porede kao jednaki (i, stoga, nejedinstveni) sa podrazumevanom kolacijom. Ako želite poređenja koja razlikuju velika i mala slova u određenoj koloni ili tabeli, promenite kolonu ili tabelu da koristi `utf8_bin` kolaciju.

Imajte u vidu da su, prema `MySQL Unicode Character Sets`, poređenja za `utf8_general_ci` kolaciju brža, ali malo manje tačna, od poređenja za `utf8_unicode_ci`. Ako je ovo prihvatljivo za vašu aplikaciju, trebalo bi da koristite `utf8_general_ci`jer je brže. Ako ovo nije prihvatljivo (na primer, ako vam je potreban redosled po nemačkom rečniku), koristite `utf8_unicode_ci` jer je tačniji.

> [!Warning]
>
> Modelski skupovi obrazaca validiraju jedinstvena polja na način koji razlikuje velika i mala slova. Stoga, kada se koristi kolacija koja nije osetljiva na velika i mala slova, skup obrazaca sa jedinstvenim vrednostima polja koje se razlikuju samo po veličini slova će proći validaciju, ali će se prilikom pozivanja `save()` izazvati `IntegrityError`.

### Povezivanje sa MySQL bazom podataka

[Na vrh](#baze-podataka)

Pogledajte `MySQL` dokumentaciju o podešavanjima.

Podešavanja veze se koriste ovim redosledom:

- OPTIONS.
- NAME, USER, PASSWORD, HOST,PORT
- MySQL datoteke opcija.

Drugim rečima, ako postavite ime baze podataka u `OPTIONS`, ovo će imati prednost nad `NAME`, što bi poništilo bilo šta u MySQL datoteci opcija.

Evo primera konfiguracije koja koristi datoteku `my.cnf`:

`settings.py`

```py
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "OPTIONS": {
            "read_default_file": "/path/to/my.cnf",
        },
    }
}
```

`my.cnf`

```shell
[client]
database = NAME
user = USER
password = PASSWORD
default-character-set = utf8
```

Nekoliko drugih opcija za povezivanje sa MySQLdb mogu biti korisne, kao što su `ssl`, `init_command` i `sql_mode`.

#### Podešavanje sql_mode

[Na vrh](#baze-podataka)

Podrazumevana vrednost opcije `sql_mode` sadrži `STRICT_TRANS_TABLES`. Ta opcija eskalira upozorenja u greške kada se podaci skraćuju prilikom umetanja, pa Django toplo preporučuje aktiviranje strogog režima za MySQL kako bi se sprečio gubitak podataka (ili `STRICT_TRANS_TABLES` ili `STRICT_ALL_TABLES`).

Ako treba da prilagodite SQL režim, možete podesiti `sql_mode` promenljivu kao ili ulazom  `'init_command': "SET sql_mode='STRICT_TRANS_TABLES'"` u `OPTIONS` delu konfiguravije baze podataka `DATABASES`.

#### Nivo izolacije MySQL

[Na vrh](#baze-podataka)

Prilikom istovremenih učitavanja, transakcije baze podataka iz različitih sesija (recimo, odvojene niti koje obrađuju različite zahteve) mogu međusobno komunicirati. Na ove interakcije utiče nivo izolacije transakcija svake sesije. Nivo izolacije veze možete podesiti pomoću `'isolation_level'` unosa u `OPTIONS` delu konfiguracije vaše baze podataka u `DATABASES`. Važeće vrednosti za ovaj unos su četiri standardna nivoa izolacije:

- `'read uncommitted'`
- `'read committed'`
- `'repeatable read'`
- `'serializable'`.

ili `None` da koristi konfigurisani nivo izolacije servera. Međutim, Django najbolje radi sa podrazumevano podešanim na čitanje potvrđenih podataka (`read committed`) umesto MySQL-ovog podrazumevanog, ponovljivog čitanja (`repeatable read`). Gubitak podataka je moguć kod ponovljivog čitanja. Konkretno, možete videti slučajeve gde `get_or_create()` će se podići `IntegrityError`, ali se objekat neće pojaviti u narednom `get()` pozivu.

### Kreiranje tabela

[Na vrh](#baze-podataka)

Kada Django generiše šemu, ne određuje mehanizam za skladištenje, tako da će tabele biti kreirane sa podrazumevanim mehanizmom za skladištenje za koji je konfigurisan vaš server baze podataka. Najlakše rešenje je da podesite podrazumevani mehanizam za skladištenje vašeg servera baze podataka na željeni mehanizam.

Ako koristite uslugu hostinga i ne možete da promenite podrazumevani mehanizam za skladištenje podataka na vašem serveru, imate nekoliko opcija.

- Nakon što su tabele kreirane, izvršite naredbu `ALTER TABLE` da biste konvertovali tabelu u novi mehanizam za skladištenje (kao što je InnoDB):

    ```sql
    ALTER TABLE <tablename> ENGINE=INNODB;
    ```

    Ovo može biti zamorno ako imate puno tabela.

- Druga opcija je da koristite `init_command` opciju za MySQLdb pre kreiranja tabela:

    ```sql
    "OPTIONS": {
        "init_command": "SET default_storage_engine=INNODB",
    }
    ```

    Ovo podešava podrazumevani mehanizam za skladištenje prilikom povezivanja sa bazom podataka. Nakon što su vaše tabele kreirane, trebalo bi da uklonite ovu opciju jer ona dodaje upit koji je potreban samo tokom kreiranja tabele svakoj vezi sa bazom podataka.

### Imena tabela

[Na vrh](#baze-podataka)

Postoje poznati problemi čak i u najnovijim verzijama MySQL-a koji mogu prouzrokovati promenu veličine imena tabele kada se određeni SQL izrazi izvršavaju pod određenim uslovima. Preporučuje se da koristite mala slova u nazivima tabela, ako je moguće, kako biste izbegli probleme koji mogu nastati zbog ovog ponašanja. Django koristi mala slova u nazivima tabela kada automatski generiše imena tabela iz modela, tako da je ovo uglavnom važno uzeti u obzir ako zamenjujete ime tabele putem `db_table` parametra.

### SAVEPOINTS

[Na vrh](#baze-podataka)

I Django ORM i MySQL (kada se koristi `InnoDB mehanizam` za skladištenje ) podržavaju tačke čuvanja baze podataka.

Ako koristite `MyISAM` mehanizam za skladištenje podataka, imajte na umu da ćete dobiti greške generisane bazom podataka ako pokušate da koristite metode vezane za tačke čuvanja u okviru API-ja za transakcije. Razlog za to je što je otkrivanje mehanizma za skladištenje podataka `MySQL` baze podataka/tabele skupa operacija, pa je odlučeno da se ne isplati dinamički konvertovati ove metode u metode bez operacija na osnovu rezultata takvog otkrivanja.

### Napomene o određenim poljima

#### Polja za karaktere

[Na vrh](#baze-podataka)

Sva polja koja su sačuvana sa `VARCHAR` tipovima kolona mogu imati `max_length` ograničenje od 255 znakova ako koristite `unique=True` za polje. Ovo utiče na `CharField`, `SlugField`. Pogledajte `MySQL` dokumentaciju za više detalja.

#### `TextField` ograničenja

[Na vrh](#baze-podataka)

MySQL može da indeksira samo prvih `N` znakova kolone `BLOB` ili `TEXT`. Pošto `TextField` nema definisanu dužinu, ne možete je označiti kao `unique=True`. `MySQL` će prijaviti: "BLOB/TEXT kolona '<db_column>' korišćena u specifikaciji ključa bez dužine ključa".

#### Podrška za razlomke sekundi za polja `Time` i `Datetime`

[Na vrh](#baze-podataka)

MySQL može da čuva razlomke sekunde, pod uslovom da definicija kolone uključuje indikator razlomka (npr. `DATETIME(6)`).

Django neće nadograditi postojeće kolone da bi uključio razlomke sekundi ako server baze podataka to podržava. Ako želite da ih omogućite na postojećoj bazi podataka, na vama je da ručno ažurirate kolonu na ciljnoj bazi podataka, izvršavanjem komande kao što je:

```sql
ALTER TABLE `your_table` MODIFY `your_datetime_column` DATETIME(6)
```

ili korišćenje `RunSQL` operacije u migraciji podataka.

#### `TIMESTAMP` kolone

[Na vrh](#baze-podataka)

Ako koristite staru bazu podataka koja sadrži `TIMESTAMP` kolone, morate podesiti da `USE_TZ = False` biste izbegli oštećenje podataka. `inspectdb` mapira ove kolone na i ako omogućite podršku za vremenske zone, i `MySQL` i `Django` će pokušati da konvertuju vrednosti iz `UTC` u lokalno vreme.

### Zaključavanje redova pomoću `QuerySet.select_for_update()`

[Na vrh](#baze-podataka)

`MySQL` i `MariaDB` ne podržavaju neke opcije za `SELECT ... FOR UPDATE` izraz. Ako se `select_for_update()` koristi sa nepodržanom opcijom, onda se podiže `NotSupportedError`.

Opcija       |   MariaDB      |   MySQL       |
-------------|----------------|---------------|
SKIP LOCKED  |   X (≥10,6)    |   X (≥8,0,1)  |
NOWAIT       |   X            |   X (≥8,0,1)  |
OF           |                |   X (≥8,0,1)  |
NO KEY       |                |               |

Kada koristite `select_for_update()` na `MySQL`, obavezno filtrirajte `QuerySet` prema barem jednom skupu polja sadržanih u jedinstvenim ograničenjima ili samo prema poljima obuhvaćenim indeksima. U suprotnom, biće stečena ekskluzivna brava za pisanje nad celom tabelom tokom trajanja transakcije.

### Automatsko pretvaranje tipova može prouzrokovati neočekivane rezultate

[Na vrh](#baze-podataka)

Prilikom izvršavanja upita nad tipom stringa, ali sa celobrojnom vrednošću, `MySQL` će prisiliti tipove svih vrednosti u tabeli na ceo broj pre nego što izvrši poređenje. Ako vaša tabela sadrži vrednosti 'abc', 'def' i upitate za `WHERE mycolumn=0`, oba reda će se podudarati. Slično tome, će se podudarati sa vrednošću `WHERE mycolumn=1` ili `'abc1'`. Stoga, polja tipa stringa uključena u Django će uvek konvertovati vrednost u string pre nego što je koriste u upitu.

Ako implementirate prilagođena polja modela koja direktno nasleđuju iz `Field`, nadjačavaju `get_prep_value()` ili koriste `RawSQL`, `extra()`, ili `raw()`, trebalo bi da se uverite da ste izvršili odgovarajuće pretvaranje tipova.

## SQLite beleške

[Na vrh](#baze-podataka)

Django podržava `SQLite 3.21.0` i novije verzije.

`SQLite` pruža odličnu alternativu za razvoj aplikacija koje su pretežno samo za čitanje ili zahtevaju manji instalacioni prostor. Međutim, kao i kod svih servera baza podataka, postoje neke razlike specifične za `SQLite` kojih bi trebalo da budete svesni.

### Podudaranje podstringova i osetljivost na velika i mala slova

[Na vrh](#baze-podataka)

Za sve verzije `SQLite`, postoji pomalo kontraintuitivno ponašanje pri pokušaju uparivanja nekih tipova stringova. Ovo se pokreće kada se koriste filteri `iexactor` ili `contains` u Queryset-ovima. Ponašanje se deli na dva slučaja:

1. Za podudaranje podstringa, sva podudaranja se vrše bez razlikovanja velikih i malih slova. To je filter kao što je `filter(name__contains="aa")` koji će podudarati sa imenom "Aabb".

2. Za stringove koji sadrže znakove van ASCII opsega, sva tačna podudaranja stringova se izvršavaju osetljivo na velika i mala slova, čak i kada se u upit prosleđuju opcije koje ne razlikuju velika i mala slova. Dakle, `iexact` filter će se ponašati potpuno isto kao i `exact` filter u ovim slučajevima.

Neka moguća rešenja za ovo su dokumentovana na sqlite.org, ali ih ne koristi podrazumevani `SQLite` bekend u Django-u, jer bi njihovo uključivanje bilo prilično teško izvesti robusno. Stoga, Django otkriva podrazumevano SQLite ponašanje i trebalo bi da budete svesni toga kada koristite filtriranje bez razlikovanja velikih i malih slova ili podstringova.

### Decimalna obrada

[Na vrh](#baze-podataka)

SQLite nema pravi interni `decimal` tip. Decimalne vrednosti se interno konvertuju u `REAL` tip podataka (8-bajtni IEEE broj sa pokretnim zarezom), kao što je objašnjeno u dokumentaciji o tipovima podataka SQLite-a, tako da ne podržavaju ispravno zaokruženu decimalno-pokretnu aritmetiku sa pokretnim zarezom.

### Greške "Baza podataka je zaključana"

[Na vrh](#baze-podataka)

`SQLite` je zamišljen kao lagana baza podataka i stoga ne može da podrži visok nivo konkurentnosti. Greške ukazuju na to da vaša aplikacija ima više konkurentnosti nego što može da obradi u podrazumevanoj konfiguraciji. Ova greška znači da jedna nit ili proces ima ekskluzivnu bravu na vezi sa bazom podataka, a drugoj niti je isteklo vreme čekanja da se brava otpusti.

```sql
OperationalError: database is lockeds qlite
```

Pajtonov `SQLite` omotač ima podrazumevanu vrednost tajmauta koja određuje koliko dugo je drugoj niti dozvoljeno da čeka na zaključavanje pre nego što istekne tajming i prikaže grešku.

```sql
OperationalError: database is locked
```

Ako dobijate ovu grešku, možete je rešiti na sledeći način:

- Prelazak na drugu bazu podataka. U određenom trenutku SQLite postaje previše `lagan` za aplikacije u stvarnom svetu, a ove vrste grešaka konkurentnosti ukazuju da ste došli do te tačke.

- Prepisivanje koda kako bi se smanjila konkurentnost i osiguralo da transakcije u bazi podataka budu kratkotrajne.

- Povećajte podrazumevanu vrednost vremenskog ograničenja podešavanjem timeoutopcije baze podataka:

    ```py
    "OPTIONS": {
        # ...
        "timeout": 20,
        # ...
    }
    ```

    Zbog toga će `SQLite` čekati malo duže pre nego što izbaci greške "baza podataka je zaključana;", neće zapravo ništa učiniti da ih reši.

### QuerySet.select_for_update() nije podržano

[Na vrh](#baze-podataka)

SQLite ne podržava `SELECT ... FOR UPDATE` sintaksu. Pozivanje funkcije neće imati nikakvog efekta.

### Izolacija pri korišćenju QuerySet.iterator()

[Na vrh](#baze-podataka)

Postoje posebna razmatranja opisana u odeljku `Izolacija u SQLite` prilikom modifikovanja tabele tokom iteracije kroz nju pomoću `QuerySet.iterator()`. Ako se red doda, promeni ili obriše unutar petlje, onda se taj red može, ali i ne mora pojaviti, ili se može pojaviti dva puta, u narednim rezultatima preuzetim iz iteratora. Vaš kod mora ovo da obradi.

### Omogućavanje JSON1 ekstenzije na SQLite

[Na vrh](#baze-podataka)

Da biste koristili `JSONField` na SQLite, potrebno je da omogućite `JSON1` ekstenziju u Python-ovoj `sqlite3` biblioteci. Ako ekstenzija nije omogućena na vašoj instalaciji,  pojaviće se sistemska greška ( `fields.E180` ).

Da biste omogućili `JSON1` ekstenziju, možete pratiti uputstva na viki stranici.

> [!Note]
>
> `JSON1` ekstenzija je podrazumevano omogućena na `SQLite 3.38+`.

## Oracle beleške

[Na vrh](#baze-podataka)

Django podržava Oracle Database Server verzije `19c` i novije. Potrebna je verzija `7.0` ili novija `cx_Oracle` Python drajvera.

Da bi komanda `python manage.py migrate` funkcionisala, korisnik vaše `Oracle` baze podataka mora imati privilegije za pokretanje sledećih komandi:

- CREATE TABLE
- CREATE SEQUENCE
- CREATE PROCEDURE
- CREATE TRIGGER

Da bi pokrenuo test paket projekta, korisniku su obično potrebne ove dodatne privilegije:

- CREATE USER
- ALTER USER
- DROP USER
- CREATE TABLESPACE
- DROP TABLESPACE
- CREATE SESSION WITH ADMIN OPTION
- CREATE TABLE WITH ADMIN OPTION
- CREATE SEQUENCE WITH ADMIN OPTION
- CREATE PROCEDURE WITH ADMIN OPTION
- CREATE TRIGGER WITH ADMIN OPTION

Iako `RESOURCE` uloga ima potrebne privilegije `CREATE TABLE`, `CREATE SEQUENCE`, `CREATE PROCEDURE`, i `CREATE TRIGGER`, a korisnik kome je dodeljena `RESOURCE WITH ADMIN OPTION` može da dodeli `RESOURCE` takav korisnik ne može da dodeli pojedinačne privilegije (npr. CREATE TABLE), i stoga `RESOURCE WITH ADMIN OPTION` obično nije dovoljna za pokretanje testova.

Neki testovi takođe kreiraju prikaze ili materijalizovane prikaze; da bi ih pokrenuo, korisniku su takođe potrebne `CREATE VIEW WITH ADMIN OPTION` i `CREATE MATERIALIZED VIEW WITH ADMIN OPTION` privilegije.  Ovo je posebno potrebno za Djangove sopstvene testove.

Sve ove privilegije su uključene u ulogu administratora baza podataka, koja je pogodna za upotrebu na bazi podataka privatnog programera.

Oracle baza podataka koristi pakete `SYS.DBMS_LOB` i `SYS.DBMS_RANDOM`, tako da će vašem korisniku biti potrebne dozvole za izvršavanje na njoj. Obično je podrazumevano dostupna svim korisnicima, ali u slučaju da nije, moraćete da dodelite dozvole na sledeći način:

```sql
GRANT EXECUTE ON SYS.DBMS_LOB TO user;
GRANT EXECUTE ON SYS.DBMS_RANDOM TO user;
```

### Povezivanje sa Oracle bazom podataka

[Na vrh](#baze-podataka)

Da biste se povezali koristeći ime servisa vaše Oracle baze podataka, vaša `settings.py` datoteka bi trebalo da izgleda ovako:

```py
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.oracle",
        "NAME": "xe",
        "USER": "a_user",
        "PASSWORD": "a_password",
        "HOST": "",
        "PORT": "",
    }
}
```

U ovom slučaju, trebalo bi da ostavite i `HOST` i `PORT` prazna prazna. Međutim, ako ne koristite `tnsnames.or`a datoteku ili sličan metod imenovanja i želite da se povežete koristeći `SID` („xe“ u ovom primeru), onda popunite oba `HOST` i `PORT` ovako:

```py
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.oracle",
        "NAME": "xe",
        "USER": "a_user",
        "PASSWORD": "a_password",
        "HOST": "dbprod01ned.mycompany.com",
        "PORT": "1540",
    }
}
```

Trebalo bi da navedete i `HOST` i `PORT`, ili da oba ostavite kao prazne stringove. Django će koristiti drugačiji deskriptor povezivanja u zavisnosti od tog izbora.

#### Kompletan DSN i jednostavno povezivanje

[Na vrh](#baze-podataka)

Može se koristiti potpuni `DSN` ili string za jednostavno povezivanje `NAME` ako su i `HOST` i `PORT` prazni. Ovaj format je potreban kada se koristi `RAC` ili priključne baze podataka bez `tnsnames.ora`, na primer.

Primer stringa za jednostavno povezivanje:

```py
"NAME": "localhost:1521/orclpdb1"
```

Primer punog DSN niza:

```py
"NAME": (
    "(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=localhost)(PORT=1521))"
    "(CONNECT_DATA=(SERVICE_NAME=orclpdb1)))"
)
```

### Opcija sa nitima

[Na vrh](#baze-podataka)

Ako planirate da pokrećete Django u višenitnom okruženju (npr. `Apache` koristeći podrazumevani `MPM modul` na bilo kom modernom operativnom sistemu), onda morate podesiti `threaded` opciju konfiguracije vaše `Oracle` baze podataka na `True`:

```py
"OPTIONS": {
    "threaded": True,
}
```

Ako se ovo ne uradi, može doći do padova i drugih neobičnih ponašanja.

### INSERT … RETURNING INTO

[Na vrh](#baze-podataka)

Podrazumevano, Oracle bekend koristi `RETURNING INTO` klauzulu za efikasno preuzimanje vrednosti `AutoField` prilikom umetanja novih redova. Ovo ponašanje može dovesti do `DatabaseError` u određenim neobičnim podešavanjima, kao što je umetanje u udaljenu tabelu ili u prikaz sa `INSTEAD OF` trigerom. Klauzula RETURNING INTO se može onemogućiti podešavanjem opcije `use_returning_into` konfiguracije baze podataka na: `False`.

```py
"OPTIONS": {
    "use_returning_into": False,
}
```

U ovom slučaju, `Oracle` bekend će koristiti poseban `SELECT` upit za preuzimanje `AutoField` vrednosti.

### Problemi sa imenovanjem

[Na vrh](#baze-podataka)

Oracle nameće ograničenje dužine imena od 30 znakova. Da bi se ovo prilagodilo, bekend skraćuje identifikatore baze podataka kako bi se uklopili, zamenjujući poslednja četiri znaka skraćenog imena ponavljajućom MD5 heš vrednošću. Pored toga, bekend pretvara identifikatore baze podataka u velika slova.

Da biste sprečili ove transformacije (ovo je obično potrebno samo kada se radi sa starijim bazama podataka ili pristupa tabelama koje pripadaju drugim korisnicima), koristite navodnike `"` kao vrednost za `db_table`:

```py
class LegacyModel(models.Model):
    class Meta:
        db_table = '"name_left_in_lowercase"'


class ForeignModel(models.Model):
    class Meta:
        db_table = '"OTHER_USER"."NAME_ONLY_SEEMS_OVER_30"'
```

Navodnici se takođe mogu koristiti sa drugim podržanim Django backend-ovima baza podataka; osim za `Oracle`, gde navodnici nemaju nikakvog efekta.

Prilikom pokretanja `migrate`, može doći do greške `ORA-06552` ako se određene ključne reči iz `Oracle` koriste kao ime polja modela ili vrednost opcije. Django citira sve identifikatore koji se koriste u upitima kako bi sprečio većinu takvih problema, ali ova greška se i dalje može pojaviti kada se tip podataka `Oracle` `db_column` koristi kao ime kolone. Posebno vodite računa da izbegavate korišćenje imena `date`,`timestamp`, `number` ili `float` kao ime polja.

### NULL i prazni stringovi

[Na vrh](#baze-podataka)

Django generalno preferira da koristi prazan string ( `''` ) umesto `NULL`, ali Oracle tretira oba identično. Da bi se ovo zaobišlo, Oracle-ov bekend ignoriše eksplicitnu `null` opciju na poljima koja imaju prazan string kao moguću vrednost i generiše DDL kao da je `null=True`. Prilikom preuzimanja iz baze podataka, pretpostavlja se da `NULL` vrednost u jednom od ovih polja zapravo znači prazan string, a podaci se tiho konvertuju da bi odražavali ovu pretpostavku.

### TextField ograničenja

Oracle bekend skladišti `TextFields` kao `NCLOB` kolone. Oracle nameće neka ograničenja na korišćenje takvih `LOB` kolona uopšte:

- `LOB` kolone se ne mogu koristiti kao primarni ključevi.

- `LOB` kolone se ne smeju koristiti u indeksima.

- `LOB` kolone se ne smeju koristiti u `SELECT DISTINCT` listi. To znači da će pokušaj korišćenja `QuerySet.distinct` metode na modelu koji uključuje `TextField` kolone rezultirati greškom kada se pokrene u Oracle-u. Kao zaobilazno rešenje, koristite `QuerySet.defer` metodu zajedno sa `distinct` da biste sprečili uključivanje `TextField` kolona u `SELECT DISTINCT` listu.

## Podklasiranje ugrađenih bekendova baze podataka

[Na vrh](#baze-podataka)

Django dolazi sa ugrađenim bekendovima baze podataka. Možete podklasirati postojeće bekendove baze podataka da biste izmenili njihovo ponašanje, funkcije ili konfiguraciju.

Razmotrite, na primer, da treba da promenite jednu funkciju baze podataka. Prvo, morate da kreirate novi direktorijum sa `base` modulom u njemu. Na primer:

```shell
mysite/
    ...
    mydbengine/
        __init__.py
        base.py
```

Modul `base.py` mora da sadrži klasu pod nazivom `DatabaseWrapper` koja podklasira postojeći mehanizam iz `django.db.backends` modula. Evo primera podklasiranja PostgreSQL mehanizma da bi se promenila klasa karakteristika `allows_group_by_selected_pks_on_model`:

`mysite/mydbengine/base.py`

```py
from django.db.backends.postgresql import base, features

class DatabaseFeatures(features.DatabaseFeatures):
    def allows_group_by_selected_pks_on_model(self, model):
        return True

class DatabaseWrapper(base.DatabaseWrapper):
    features_class = DatabaseFeatures
```

Konačno, morate navesti DATABASE-ENGINE u svojoj `settings.py` datoteci:

```py
DATABASES = {
    "default": {
        "ENGINE": "mydbengine",
        # ...
    },
}
```

Trenutnu listu sistema baza podataka možete videti u `django/db/backends`.

## Korišćenje baze podataka treće strane

[Na vrh](#baze-podataka)

Pored zvanično podržanih baza podataka, postoje i bekendovi koje pružaju treće strane koji vam omogućavaju da koristite druge baze podataka sa Django-om:

- `CockroachDB`
- `Firebird`
- `Google Cloud Spanner`
- `Majkrosoft SQL Server`
- `Snowflake`
- `TiDB`
- `JugabyteDB`

Verzije Django-a i ORM funkcije koje podržavaju ove nezvanične bekendove znatno se razlikuju. Upite u vezi sa specifičnim mogućnostima ovih nezvaničnih bekendova, zajedno sa svim upitima za podršku, treba uputiti na kanale za podršku koje pruža svaki projekat treće strane.

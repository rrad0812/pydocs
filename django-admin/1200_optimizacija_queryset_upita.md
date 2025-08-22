# Optimizacija QuerySet upita

## Sadržaj

[Nazad](sadrzaj.md)

- [select_related](#select_related)
  - [*fields parametri](#fields-parametri)
  - [depth parameter](#depth-parameter)
  - [*parameters](#parameters)
  - [Zaključak select_related](#zaključak-select_related)
- [prefetch_related](#prefetch_related)
  - [*lookups parameter](#lookups-parameter)
  - [Prefetch objekat](#prefetch-objekat)
  - [None](#none)
  - [Zaključak prefetch related](#zaključak-prefetch_related)
- [prefetch_related vs selected_related](#prefetch_related-vs-selected_related)
  - [Zaključak prefetch_related vs selected_related](#zaključak-prefetch_related-vs-selected_related)
- [Kombinovano korišćenje](#kombinovano-korišćenje)
- [Prefetching](#prefetching)
  - [Šta je problem?](#šta-je-problem)
  - [prefetch_related 2](#prefetch_related-2)
  - [Uvod u Prefetch objekat](#uvod-u-prefetch-objekat)

Kada db ima strane ključeve, korišćenjem `select_related()` i `prefetch_related()` možemo smanjiti broj db zahteva i poboljšati performanse.

**Opis primera**:

`models.py:`

```py
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
```

> [!Note]
> Kreirana aplikacija se zove "QSOptimize"
>
> [!Note]
> Zbog jednostavnosti, imamo dva podatka u tabeli "qsoptimize_province":
>
> - Hubei Province i Guangdong Province,
>
> i tri podatka u tabeli "qsoptimize_city":
>
> - Wuhan City, Shiyan City i Guangzhou City.

[Sadržaj](#sadržaj)

### select_related

Za "jedan na jedan" veze i polja stranih ključeva, možete koristiti `select_related` za optimizaciju `QuerySet`-a.

Posle upotrebe `select_related()` funkcije na QuerySet-u, Django vraća objekat koji odgovara stranom ključu, zbog toga eliminiše potrebu za kasnijim upitima na db. Ako treba da štampamo sve gradove i njihove provincije, na direktan način:

```shell
>>> cities = City.objects.all()
>>> for c in cities:
... print c.province
```

Ovo vodi do linearnih SQL upita. Ako postoji n objekata sa k stranih ključeva za svaki objekat, to će rezultovati sa n*k+1 SQL upita. U ovom slučaju postoji tri grada, i biće četiri upita:

```sql
SELECT
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
FROM
  QSOptimize_city

SELECT
  QSOptimize_province.id,
  QSOptimize_province.name
FROM
  QSOptimize_province
WHERE
  QSOptimize_province.id = '1' ;

SELECT
  QSOptimize_province.id,
  QSOptimize_province.name
FROM
  QSOptimize_province
WHERE
  QSOptimize_province.id = '2' ;

SELECT
  QSOptimize_province.id,
  QSOptimize_province.name
FROM
  QSOptimize_province
WHERE
  QSOptimize_province.id = '1' ;
```

> [!Note]
>
> SQL izrazi su uzeti direktno sa Django logger-a.

Ako koristimo `select_related()` funkciju:

```shell
>>> cities = City.objects.select_related().all()
>>> for c in cities:
... print c.province
```

Sada je samo jedan SQL upit:

```sql
SELECT
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id,
  QSOptimize_province.id,
  QSOptimize_province.name
FROM
  QSOptimize_city
INNER JOIN
  QSOptimize_province ON (QSOptimize_city.province_id = QSOptimize_province.id);
```

Django ovde koristi `INNER JOIN` da dobije informacije o provincijama. Rezultati upita su:

Id | city_name      |  province_id | id | province_name
---|----------------|--------------|----|------------------
 1 | Wuhan City     | 1            | 1  | Hubei Province
 2 | Guangzhou City | 2            | 2  | Guangdong Province
 3 | Shiyan City    | 1            | 1  | Hubei Province

Ovaj metod podržava sledeće argumente:

[Sadržaj](#sadržaj)

#### *fields parametri

`select_related()` prihvata varijabilni broj parametara, svaki od njih je ime polja stranog ključa, (tj. sadržaj glavne tabele) koji treba pokupiti, kao i ime stranog ključa itd.

Da bi izabrali polje iz parent tabele treba koristiti dve podcrte "__".

Na primer, da bi dobili Zhang San-ovu provinciju:

```shell
>>> zhangs = Person.objects
      .select_related('living province')
      .get(firstname = u "Zhang", lastname = u "San")
>>> zhangs.living.province
```

Odgovarajući SQL upit je:

```sql
SELECT
  QSOptimize_person.id,
  QSOptimize_person.firstname,
  QSOptimize_person.lastname,
  QSOptimize_person.hometown_id,
  QSOptimize_person.living_id,
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id,
  QSOptimize_province.id,
  QSOptimize_province.name
FROM
  QSOptimize_person
INNER JOIN
  QSOptimize_city ON (QSOptimize_person.living_id = QSOptimize_city.id)
INNER JOIN
  QSOptimize_province ON(QSOptimize_city.province_id=
    QSOptimize_province.id)
WHERE (
  QSOptimize_person.lastname = San AND QSOptimize_person.first name =
    Zhang
);
```

Django koristi dva INNER JOIN-a za kompletiranje upita, dobija sadržaj tabela city i province i dodaje ih na odgovarajuće vrste rezultujuće tabele, tako da ne treba više upita.

Id | name      |Home |Living City_Id | City_Name | Province_id | Id | Province_name | Town  | Town_id
---|-----------|-----|---------------|-----------|-------------|----|---------------|-------|---------
 1 | Zhang San | 3   | 1             | 1         | Wuhan       | 1  | 1             | Hubei | .

Nespecificirani strani ključevi nisu dodati u rezultat. Treba dobiti Zhang San’s hometown, na sledeći način:

>>> zhangs.hometown.province

```sql
SELECT
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
FROM
  QSOptimize_city
WHERE
  QSOptimize_city.id = 3 ;

SELECT
  QSOptimize_province.id,
  QSOptimize_province.name
FROM
  QSOptimize_province
WHERE
  QSOptimize_province.id = 1
```

Pošto strani ključ nije specificiran, napravljena su dva upita. Ako je veća dubina, veći je i broj upita. Ako želite i hometown i province gde živi u pre v1.7:

```shell
zhangs = Person.objects.select_related('town province', 'living province').
    get(first name = u "Zhang", LastName = u "San")

>>> zhangs.hometown.province
>>> zhangs.living.province
```

Od ver. 1.7 možete ulančati operacije kao i druge queryset funkcije:

```shell
zangs = Person.objects.select_related('town province')
  .select_related('living province')
  .get(first name = u "Zhang", LastName = u "San")

>>> zhangs.hometown.province
>>> zhangs.living.province
```

Za verzije do "1.7" dobićete rezultate samo posledje operacije, u ovom slučaju nema rezultata za hometown.

Dva SQL upita su generisana kada štampate vašu provinciju.

[Sadržaj](#sadržaj)

#### depth parameter

`select_related()` prihvata depth parametar, koji definiše dubinu `select_related`. Django rekurzivno prolazi sve `OneToOneField` i `ForeignKey`.

Za ilustraciju:

```shell
>>> zhangs = Person.objects.select_related(depth = d)
```

d = 1 je ekvivalentno sa:

```py
select_related("hometown", "living")
```

d = 2 je ekvivalentno sa:

```py
select_related('citizen province', 'living province')
```

[Sadržaj](#sadržaj)

#### *parameters

`select_related()` može biti parametrizovan, što znači da će Django ići sa `select_related` najdublje što može.

Na primer:

```py
zangs = Person.objects.select_related().get(first name=u"Zhang", LastName=u"San")
```

Dve stvari treba primetiti:

- Django može u kompleksnim situacijama probiti granice rekurzije.
- Django ne zna koja polja koristitite, zbog toga uzima sve, što je gubitak performansi.

[Sadržaj](#sadržaj)

#### Zaključak select_related

- `select_related` optimizuje `jedan na jedan` i `više na jedan` veze.
- `select_related` koristi `JOIN` naredbe da optimizuje i poboljša performanse smanjenjem broja upita.
- Možete specifikovati ime polja. Specifikovani rekurzivni upit može biti implementiran sa duplim podcrtama povezanim na ime polja. Ovde nema keširanja.
- Može se specifikovati rekurzivna dubina, i Django automatski kešira sva polja unutar specifikovanog puta. Ako želite da pristupite poljima van specificiranog puta, Django će raditi SQL upit ponovo.
- Podržani su pozvi bez parametara, i Django će uraditi rekurzivne upite što je dublje moguće. Ali postoji granica u rekurzijama i gubitak na performansama.
- "Django >= 1.7", `select_related` ulančava pozive što je ekvivalentno korišćenju varijabilne dužine parametara, "Django < 1.7." ulančavanjem poziva ruši prednje `select_related` pozive i ostavlja samo poslednje.

[Sadržaj](#sadržaj)

#### prefetch_related

Za `više na više` i `jedan na više` vezu, `prefetch_related()` može biti korišćen za optimizaciju. Ali ovde nema `jedan na više` veza, reći ćete vi. U stvari, `ForeignKey` je `više na jedan` veza, a sa druge strane gledano to je `jedan na više` veza.

`prefetch_related()` i `select_related()` su dizajnirani da smanje broj SQL upita, ali su implementirane na različit način. `select_related()` rešava problem sa JOIN naredbama.

Ali, za `više na više` veze, ako bi koristili `JOIN` vezu, dobijene tabele bile bi ogromne, `n x m` veličine. Rešenje je da `prefetch_related()` odvojeno pravi upite i da zatim Python procesira njihove veze. Da bi dobili gradove koje je Zhang San posetio, `prefetch_related()` bi uradio sledeće:

```py
zhangs = Person.objects.prefetch_related('visitation')
.get(first name=u"Zhang", LastName=u"three")

>>> for city in zhangs.visitation.all():
... print city
...
```

SQL upiti su kao što sledi:

```sql
SELECT
  QSOptimize_person.id,
  QSOptimize_person.firstname,
  QSOptimize_person.lastname,
  QSOptimize_person.hometown_id,
  QSOptimize_person.living_id
FROM
  QSOptimize_person
WHERE
  QSOptimize_person.lastname='San' AND
  QSOptimize_person.firstname='Zhang');

SELECT
  QSOptimize_person_visitation.person_id AS _prefetch_related_val,
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
FROM
  QSOptimize_city
INNER JOIN
  QSOptimize_person_visitation ON ( 
    QSOptimize_city.id = QSOptimize_person_visitation.city_id
)
WHERE
  QSOptimize_person_visitation.person_id IN (1);
```

Prvi SQL upit daje samo Zhang San Person objekat. Drugi upit je kritičan.

Id | name       | Hometown_id | Living id
---|------------|-------------|----------
 1 | Zhang San  | 3           | 1

Prefetch_related | Id | Name           | Province_id
-----------------|----|----------------|------------
 1               | 1  | Wuhan City     | 1
 1               | 2  | Guangzhou City | 2
 1               | 3  | Shiyan City    | 1

Ili ako želimo da dobijemo sve gradove provincije Hubei:

```shell
>>> hb = Province.objects.prefetch_related('city_set')
.get(name iexact = u "Hubei Province")

>>> for city in hb.city_set.all():
... city.name
```

Ovo će dovesti do sledećih SQL upita:

```sql
SELECT
QSOptimize_province.id,
QSOptimize_province.name
FROM
QSOptimize_province
WHERE
QSOptimize_province.name LIKE 'Hubei Province';

SELECT
QSOptimize_city.id,
QSOptimize_city.name,
QSOptimize_city.province_id
FROM
QSOptimize_city
WHERE
QSOptimize_city.province_id IN (1);
```

A rezultujuće tabele :

Id | Name
---|-----
1  | Hubei

Id | Name       | Province_id
---|------------|-----------
1  |Wuhan City  | 1
3  |Shiyan City | 1

Prefech je implementiran korišćenjem `IN` naredbe. Kada postoji više objekata u `QuerySet`-u, problem performansi, u zavisnosti od vrste db se može pojaviti.

[Sadržaj] (#sadržaj)

#### *lookups parameter

`Prefetch_related()` je korišćen u "Django < 1.7". Kao `select_related()`, `prefetch_related()` takodje podržava duboke upite, kao napr. provincije koje su posetili svi Zhang ljudi:

```py
zhangs = Person.objects.prefetch_related('visitation province')
.filter(first_name__iexact=u'Zhang')

>>> for i in zhangs:
for city in i.visitation.all():

print city.province
```

Generisani SQL upiti su:

```sql
SELECT
  QSOptimize_person.id,
  QSOptimize_person.firstname,
  QSOptimize_person.lastname,
  QSOptimize_person.hometown_id,
  QSOptimize_person.living_id 
FROM
  QSOptimize_person
WHERE
  QSOptimize_person.first name LIKE 'Zhang';

SELECT
  QSOptimize_person_visitation.person_id AS _prefetch_related_val,
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id 
FROM
  QSOptimize_city
INNER JOIN
  QSOptimize_person_visitation ON (QSOptimize_city.id = QSOptimize_person_visitation.city_id)
WHERE
  QSOptimize_person_visitation.person_id IN (1, 4);

SELECT
  QSOptimize_province.id,
  QSOptimize_province.name FROM
  QSOptimize_province
WHERE
  QSOptimize_province.id IN (1, 2);
```

A rezultujuće tabele su:

id | name      | hometown_id | living_id
---|-----------|-------------|----------
1  | Zhang San | 3           | 1
4  | Zhang San | 2           | 2

_prefetch_related_val | Id | Name           | province_id
----------------------|----|----------------|------------------
 1                    | 1  | Wuhan City     | 1
 1                    | 2  | Guangzhou City | 2
 4                    | 2  | Guangzhou City | 2
 1                    | 3  | Skiyan City    | 1

Id | Name
---|------------------
 1 | Hubei Province
 2 | Guangdong Province

Ulančavanje `prefetch_related` dodaje ove upite.

Primetite da kada koristimo `QuerySet`, pošto je db zahtev promenjen u ulančanoj operaciji, keširanje će biti ignorisano. To dovodi do db re-request za dobijanje podataka, što može izazvati probleme sa performansama.

Promena zahteva prema db može značiti potrebu za različitim `filters()`, `exclude()`, itd, tako da to veoma menja SQL kod. `all()` ne menja krajnji db zahtev, tako da on ne vodi ka ponovnom db zahtevu.

Na primer, da bi dobili gradove sa rečju "city" koje je svako posetio, to će voditi do brojnih SQL upita:

```py
plist = Person.objects.prefetch_related('visitation')
[p.visitation.filter(name__icontains=u "city") for p in plist]
```

Pošto su četiri osobe u db, rezultat je 2 + 4 SQL upita:

```sql
SELECT
  QSOptimize_person.id,
  QSOptimize_person.firstname,
  QSOptimize_person.lastname,
  QSOptimize_person.hometown_id,
  QSOptimize_person.living_id
 FROM
  QSOptimize_person;

SELECT
  QSOptimize_person_visitation.person_id AS _prefetch_related_val,
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
FROM
  QSOptimize_city
INNER JOIN
  QSOptimize_person_visitation ON (QSOptimize_city.id =
  QSOptimize_person_visitation.city_id)
WHERE
  QSOptimize_person_visitation.person_id IN (1, 2, 3, 4);

SELECT
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
FROM
  QSOptimize_city
INNER JOIN
  QSOptimize_person_visitation ON (QSOptimize_city.id =
  QSOptimize_person_visitation.city_id)
WHERE
  QSOptimize_person_visitation.person_id= '1' AND
  QSOptimize_city.name LIKE '% city%';

SELECT
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
FROM
  QSOptimize_city
INNER JOIN
  QSOptimize_person_visitation ON (QSOptimize_city.id =
  QSOptimize_person_visitation.city_id)
WHERE
  QSOptimize_person_visitation.person_id= '2' AND
  QSOptimize_city. name LIKE '% market%';

SELECT
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
FROM
  QSOptimize_city
INNER JOIN
  QSOptimize_person_visitation ON (QSOptimize_city.id = QSOptimize_person_visitation.  
    city_id)
WHERE
  QSOptimize_person_visitation. person_id = '3' AND 
    QSOptimize_city. name LIKE '% city%';

SELECT
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
FROM
  QSOptimize_city
INNER JOIN
  QSOptimize_person_visitation ON (QSOptimize_city.id =
    QSOptimize_person_visitation.city_id)
WHERE
  QSOptimize_person_visitation. person_id = '4' AND
    QSOptimize_city. name LIKE '% city%';
```

`QuerySet` izračunavanje je lenjo, i rezultati nisu dostupni dok se ne koriste. Kada
se pokrene druga linija Python koda, `for` petlja tretira "plist" kao iterator, koji
okida db upite. Prva dva `SQL` upita izazvana su sa `prefetch_related`.

Iako su sve zahtevane `city` informacije bile uključene u rezultate upita, jer je `Person.visitation` fitriran u petlji, ovo obično menja db zahtev. Zbog toga, ove operacije će zanemariti keširane podatke i ponovo uraditi `SQL` upit.

Ali šta ako je takav zahtev? U "Django >= 1.7", možete to uraditi kroz `Prefetch` objekat. Ako je "Django < 1.7", možete uraditi ovako:

```py
plist = Person.objects.prefetch_related('visitation')
[[city for city in p.visitation.all() if u"city" in city.name] for p in plist]
```

#### Prefetch objekat

U "Django >= 1.7", možete koristiti Prefetch objekat da kontrolišete ponašanje `prefetch_related` funkcije.

Karakteristike Prefetch objekta:

- `Prefetch` objekat može specifikovati samo jednu prefetch operaciju.
- `Prefetch` objekti specifikuju polja na isti način kao parametri u `prefetch_related`, lookup polja se prave duplom podcrtom.
- Možete manuelno specificirati QuerySet korišćen od `prefetch` kroz `queryset` parameter.
- Ime `prefetch` atributa može biti specficirano sa `to_attr` parametrom.
- `Prefetch` objekti i `lookup` parametri specificirani u formi stringa mogu se miksovati.

Nastavimo sa primerima da dobijemo gradove sa "Wu" i "Zhou" u svim gradovima koje je svako posetio:

```py
Wus = City.objects.filter(name icontains = u"wu")
Zhous = City.objects.filter(name icontains = u"state")

plist = Person.objects.prefetch_related(
  Prefetch('visitation', queryset = wus, to_attr = "wu_city"),
  Prefetch('visitation', queryset = zhous, to_attr = "zhou_city"),
)

[p.wu_city for p in plist]
[p.zhou_city for p in plist]
```

[Sadržaj](#sadržaj)

#### None

Možete isprazniti `prefetch_related` prosledjenjem `None`:

```py
>>> prefetch_cleared_qset = qset.prefetch_related(None)
```

[Sadržaj](#sadržaj)

#### Zaključak prefetch_related

- `Prefetch_related optimizuje` `jedan na više` i `više na više` veze.
- `Prefetch_related` optimizuje veze izmedju tabela vraćanjem njihovog sadržaja odvojeno i korišćenjem Python-a za procesiranje njihovih veza.
- Možete specificirati ime polja koje zahteva `prefetch_related` korišćenjem varijabilne dužine parametara, na isti način kao kod `select_related`.
- U "Django >= 1.7", kompleksni upiti mogu biti implementirani kroz `Prefetch` objekte, ali izgleda da u nižim verzijama Django-a to isto može biti implementirano.
- `Prefetch` objekti i stringovi mogu se mešati kao parametri `prefetch_related`.
- Ulanačni pozivi `prefetch_related` dodaju `prefetch` umesto da ga menjaju.
- `Prefetch_related` može biti očišćen prosledjenjem `None`.

[Sadržaj](#sadržaj)

### prefetch_related vs selected_related

Ako želimo da dobijemo ko je sve rodjen u Hubei province, najlakše je da dobijemo:

- Hubei province prvo,
- potom sve gradove u Hubei,
- i na kraju njihove žitelje:

```shell
>>> hb = Province.objects.get(name iexact = u"Hubei Province")
>>> people = []
>>> for city in hb.city_set.all():
... people.extend(city.birth.all())
```

Ovo će dovesti do 1 + (broj gradova u Hubei) SQL upita.

Pokušajmo sa prefetch_related():

```shell
>>> hb = Province.objects.prefetch_related("city_set birth")
  .objects.get(name iexact = u"Hubei Province")
>>> people = []
>>> for city in hb.city_set.all():
... people.extend(city.birth.all())
```

Ovo je prefetch dubine 2, biće tri SQL upita:

```sql
SELECT
  QSOptimize_province.id,
  QSOptimize_province.name
FROM
  QSOptimize_province
WHERE
  QSOptimize_province.name LIKE 'Hubei Province';

SELECT
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
FROM
  QSOptimize_city
WHERE
  QSOptimize_city.province_id IN (1);

SELECT
  QSOptimize_person.id,
  QSOptimize_person.firstname, QSOptimize_person.lastname,
  QSOptimize_person.hometown_id,
  QSOptimize_person.living_id
FROM
  QSOptimize_person
WHERE
  QSOptimize_person.hometown_id IN (1, 3);
```

Možda je reverzni upit jednostavniji?

```py
People = list(Person.objects.select_related("town__province")
.filter(town province name iexact = u"hubei__province")
```

```sql
SELECT
  QSOptimize_person.id,
  QSOptimize_person.firstname,
  QSOptimize_person.lastname,
  QSOptimize_person.hometown_id,
  QSOptimize_person.living_id,
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id,
  QSOptimize_province.id,
  QSOptimize_province.name
FROM
  QSOptimize_person
INNER JOIN
  QSOptimize_city ON (QSOptimize_person.hometown_id =
  QSOptimize_city.id)
INNER JOIN
  QSOptimize_province ON (QSOptimize_city.province_id = QSOptimize_province.id)
WHERE
  QSOptimize_province.name LIKE 'Hubei Province';
```

id | name      |hometown_id | living_id |id | name   | province_id | id | name
---|-----------|------------|-----------|---|--------|-------------|----|------
1  | Zhang San | 3          | 3         | 1 | Shiyan | 1           | 1  | Hubei
2  | Li        | 4          | 1         | 3 | Wuhan  | 1           | 1  | Hubei
3  | Wang Mazi | 3          | 2         | 3 | Shiyan | 1           | 1  | Hubei

`select_related()` je efikasniji od `prefetch_related()`. Zbog toga najbolje je koristi ga gde je moguće, kao što je `više na jedan` veza.

[Sadržaj](#sadržaj)

#### Zaključak prefetch_related vs selected_related

- Pošto `select_related()` uvek rešava problem u jednom SQL upitu i `prefetch_related()` pravi upite na svakoj povezanoj tabeli, efikasnost `select_related()` je obično veća.
- Razmišljajte o `prefetch_related()` samo ako `select_related()` ne može da reši problem.
- Možete koristiti i `select_related()` i `prefetch_related()` u jednom QuerySet-u da smanjite broj SQL upita.
- `select_related()` pre `prefetch_related()` je validno. Ako ide posle biće ignorisano.

[Sadržaj](#sadržaj)

### Kombinovano korišćenje

Za isti QuerySet, možete koristiti obe funkcije u isto vreme. Dodajmo model Order:

```py
class Order(models.Model):
  customer = models.ForeignKey(Person)
  orderinfo = models.CharField(max_length=50)
  time = models.DateTimeField(auto_now_add = True)

def unicode (self):
  return self.orderinfo
```

Ako imamo "order_id", treba da saznamo provinciju gde je customer ordera bio. Ako pokušamo sa "prefetch_related()":

```shell
>>> plist = Order.objects
  .prefetch_related('customer visitation province')
  .get(id=1)
>>> for city in plist.customer.visitation.all():
... print city.province.name
```

Pošto su četiri tabele uključene: "Order", "Person", "City", "Province", `prefetch_related()` će generisati četiri SQL upita:

```sql
SELECT
  QSOptimize_order.id,
  QSOptimize_order.customer_id,
  QSOptimize_order.orderinfo,
  QSOptimize_order.time
FROM
  QSOptimize_order
WHERE
  QSOptimize_order.id = 1;

SELECT
  QSOptimize_person.id,
  QSOptimize_person.firstname,
  QSOptimize_person.lastname,
  QSOptimize_person.hometown_id, QSOptimize_person.living_id
FROM
  QSOptimize_person
WHERE
  QSOptimize_person.id IN (1);

SELECT
  QSOptimize_person_visitation.person_id AS _prefetch_related_val,
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
 FROM
  QSOptimize_city
INNER JOIN
  QSOptimize_person_visitation ON (QSOptimize_city.id =
    QSOptimize_person_visitation.city_id)
WHERE QSOptimize_person_visitation.person_id IN (1);

SELECT
  QSOptimize_province.id,
  QSOptimize_province.name
FROM
  QSOptimize_province
WHERE
  QSOptimize_province.id IN (1, 2);
```

I rezultujuće tabele će biti:

 id | customer_id | orderinfo      | time
----|-------------|----------------|--------------------
  1 | 1           | Info of order  | 2014-08-10 17:05:48

 id | name      | hometown_id | living_id
----|-----------|-------------|----------
 1  | Zhang San | 3           | 1

prefetch_related_val | id | name           | province_id
---------------------|----|----------------|------------
 1                   |  1 | Wuhan City     | 1
 1                   |  2 | Guangzhou City | 2
 1                   |  3 | Shiyan City    | 1

id | name
---|-------
 1 | Hubei Province
 2 | Gunagding Province

Bolji način je pozvati select_related() jednom, potom prefetch_related():

```shell
>>> plist = Order.objects
  .select_related('customer')
  .prefetch_related('customer visitation province')
  .get(id=1)
>>> for city in plist.customer.visitation.all():
... print city.province.name
```

Rezultat je tri upita. Django će prvo pozvati `select_related`, a potom `prefetch_related` koristeći keširane podatke:

```sql
SELECT
  QSOptimize_order.id,
  QSOptimize_order.customer_id, QSOptimize_order.orderinfo,
  QSOptimize_order.time,
  QSOptimize_person.id,
  QSOptimize_person.firstname,
  QSOptimize_person.lastname,
  QSOptimize_person.hometown_id,
  QSOptimize_person.living_id
FROM
  QSOptimize_order
INNER JOIN
  QSOptimize_person ON (QSOptimize_order.customer_id = QSOptimize_person.id)
WHERE
  QSOptimize_order.id = '1';

SELECT
  QSOptimize_person_visitation.person_id AS _prefetch_related_val,
  QSOptimize_city.id,
  QSOptimize_city.name,
  QSOptimize_city.province_id
FROM
  QSOptimize_city
INNER JOIN
  QSOptimize_person_visitation ON (QSOptimize_city.id = 
    QSOptimize_person_visitation.city_id)
WHERE
  QSOptimize_person_visitation.person_id IN ('1', );

SELECT
  QSOptimize_province.id,
  QSOptimize_province.name
FROM
  QSOptimize_province
WHERE
  QSOptimize_province.id IN ('1', '2');
```

Rezultujuće tabele su:

id | customer_id | orderinfo | time       |id | name      | hometown_id |living_id
---|-------------|-----------|------------|---|-----------|-------------|----------
1  | 1           | Info of   | 2014-08-10 |1  | Zhang San | 3           | 1

prefetch_related_val | id | name           | province_id
---------------------|----|----------------|-----------
1                    | 1  | Wuhan City     | 1
1                    | 2  | Guangzhou City | 2
1                    | 3  | Shuiyan City   | 1

id |name
---|--------------------
1  | Hubei Province
2  | Guangdong Province

[Sadržaj](#sadržaj)

### Prefetching

Skoro sam radio prijavni sistem za jednu konferenciju. Bilo im je veoma važno da vide tabelu prijava uključujući kolonu sa listom imena programa. Ugrubo modeli izgledaju ovako:

```py
class Program(models.Model):
  name = models.CharField(max_length=20,)

class Price(models.Model):
  program = models.ForeignKey(Program,)
  from_date = models.DateTimeField()
  to_date = models.DateTimeField()

class Order(models.Model):
  state = models.CharField(max_length=20,)
  items = models.ManyToManyField(Price)
```

Ovde je:

- Program – jedna sesija, lektura ili konferencijski dan.
- Price – cena koja se može menjati u vremenu. Model reprezentuje cenu programa u odredjenom vremenskom trenutku.
- Order – prijava na jedan ili više programa. Svaka stavka u prijavi ima cenu u trenutku kada je prijava napravljena.

Kroz log mi ćemo pratiti upite izvršene od strane Djanga. Da bi ih logovali dodajmo sledeće u `settings.py`:

```py
LOGGING = {
  ...
  'loggers': {
    'django.db.backends': {'level': 'DEBUG',},
  },
}
```

[Sadržaj](#sadržaj)

#### Šta je problem?

Pokušajmo da dobijemo ime programa iz jedne prijave:

```shell
>>> o = Order.objects.filter(state='completed').first()
```

```sql
SELECT 
  ...
FROM 
  orders_order
WHERE 
  orders_order.state = 'completed'ORDER BY orders_order.id ASC 
LIMIT 1;
```

```shell
>>> [p.program.name for p in o.items.all()]
```

```sql
SELECT 
  ...
FROM 
  events_price
INNER 
  JOIN orders_order_items ON (events_price.id = orders_order_items.price_id)
WHERE 
 orders_order_items.order_id = '29';

SELECT 
  ...
FROM 
  events_program
WHERE 
  events_program.id = '8';
```

Za dobijanje kompletirane prijave potreban je jedan upit. Za dobijanje imena programa za svaku prijavu potrebana su dva upita. Ako trebamo dva upita za svaku prijavu, ukupan broj upita za 100 prijava je 1 + 100 * 2 = 201! Koristimo Django da smanjimo broj upita:

```shell
>>> o.items.values_list('program name')
```

```sql
SELECT 
  events_program.name
FROM 
  events_price
INNER 
  JOIN orders_order_items ON (events_price.id=orders_order_items.price_id)
INNER 
  JOIN events_program ON (events_price.program_id=events_program.id)
WHERE 
  orders_order_items.order_id = 29 
LIMIT 21;
```

Django je uradio povezivanje izmedju Price i Program i smanjo broj upita na jedan po prijavi. Znači umesto 201 upita sada imamo 101 upit.

#### Zašto ne možemo povezati tabele?

Ako imamo strani ključ možemo koristiti `select_related` ili 'snake case' da bi dobili povezana polja u jednom upitu. Na primer mi smo dobijali ime programa za listu cena u jednom upitu korišćenjem:

```py
values_list('program name')
```

Ovo smo mogli da uradimo jer je svaka cena bila povezana sa tačno jednim programom. Ako je veza izmedju dva modela više na više, ne možemo to uraditi. Svaka prijava ima jednu ili više povezanih cena, ako ih povežemo kao dosad dobićemo grešku duplih vrednosti.

```sql
SELECT 
  o.id AS order_id, p.id AS price_idFROM orders_order o
JOIN 
  orders_order_items op ON (o.id = op.order_id)
JOIN 
  events_price p ON (op.price_id = p.id)
ORDER BY 1, 2;
```

order_id | price_id
---------|---------
45       | 38
45       | 56
70       | 38
70       | 50
70       | 77
71       | 38

Prijave 70 i 45 imaju više stavki tako da se pojavljuju više puta u rezultatu. Django ne može upravljati sa ovim problemom!

[Sadržaj](#sadržaj)

#### prefetch_related 2

Django ima lep, ugradjen način rukovanja ovim problemom prefetch_related:

```shell
>>> o = Order.objects.filter(state='completed',)
.prefetch_related('items program',).first()
```

```sql
SELECT 
  ...
FROM 
  orders_order
WHERE 
  orders_order.state = 'completed'
ORDER BY 
  orders_order.id ASC 
LIMIT 1;

SELECT 
  (orders_order_items.order_id) AS _prefetch_related_val_order_id, events_price...
FROM 
  events_price
INNER JOIN 
  orders_order_items ON (events_price.id = orders_order_items.price_id)
WHERE 
  orders_order_items.order_id IN (29);

SELECT
  events_program.id,
  events_program.name
FROM 
  events_program
WHERE 
  events_program.id IN (8);
```

Mi smo rekli Djangu da nam je namera da dobijemo "items" program iz rezultujućeg seta. U drugom i trećem upitu možemo videti da Django dobija rezultat kroz tabelu "orders_order_items" i iz "events_program". Rezultat `prefetching`-a su keširani na objektima.

Šta se dešava kada mi pokušamo da dobijemo imena programa iz rezultata?

```shell
>>> [p.program.name for p in o.items.all()]
```

Nema dodatnih upita – tačno ono što smo želeli! Ovde je važno da kada koristimo prefetch, radimo sa objektima a ne sa queryset-om! Pokušaj da dobijemo imena programa sa upitom će proizvesti isti izlaz, queryset, ali će napraviti i dodatni upit:

```shell
>>> o.items.values_list('program name')
```

```sql
SELECT 
  events_program.name
FROM 
  events_price
INNER JOIN 
  orders_order_items ON (events_price.id = orders_order_items.price_id)
INNER JOIN 
  events_program ON (events_price.program_id = events_program.id)
WHERE 
  orders_order_items.order_id = 29 
LIMIT 21;
```

Na ovaj način imamo, za 100 prijava 3 upita.

[Sadržaj](#sadržaj)

### Uvod u Prefetch objekat

Od ver. 1.7 uveden je novi `Prefetch` objekat koji proširuje mogućnosti `prefetch_related`. Novi objekat pruža programeru mogućnost da nadjača upit koji bi koristio Django za prefetch povezanih objekata.

U našem prethodnom primeru Django koristi dva upita za prefetch – jedan kroz tabelu i jedan kroz program.

Šta ako kažemo Djangu da ih spoji?

```shell
>>> prices_and_programs = Price.objects.select_related('program')
>>> o = Order.objects.filter(state='completed')
        .prefetch_related(Prefetch('items', queryset=prices_and_programs))
        .first()
(0.01) 
```

```sql
SELECT 
  ...
FROM 
  orders_order
WHERE 
  orders_order.state = 'completed' 
ORDER BY 
  orders_order.id
ASC LIMIT 1;

SELECT (
  orders_order_items.order_id) AS _prefetch_related_val_order_id,
  events_price..., events_program...
INNER 
  JOIN events_program ON (events_price.program_id = events_program.id)
WHERE 
  orders_order_items.order_id IN (29);
```

Kreirali smo upit koji povezuje cene sa programima. Potom smo rekli Djangu da koristitaj upit za `prefetch` vrednosti. To je kao da smo rekli Djangu da nam je namera da dobijemo i items i programs za svaku prijavu.

Sada možemo da vratimo imena programa:

```shell
>>> [p.program.name for p in o.items.all()]
```

Nema dodatnih upita ! Idemo na sledeći nivo!

Kada smo govorili o modelima, rekli smo da su prices modelovane kao SCD tabela.
To znači da možemo da pravimo upit samo na aktivne cene.

Jedna cena je aktivna na odredjenom datumu ako je izmedju `from_date` i `end_date`:

```shell
>>> from django.utils import timezone
>>> now = timezone.now()
>>> active_prices = Price.objects.filter(from_date lte=now, to_date gt=now,)
```

Korišćenjem Prefetch objekta mi kažemo Djangu da smesti `prefetch` objekte u novi atribut rezultujućeg seta:

```shell
>>> from django.utils import timezone
>>> now = timezone.now()
>>> active_prices_and_programs = (Price.objects
      .filter(from_date lte=now,to_date gt=now,)
      .select_related('program'))
>>> o = Order.objects.filter(state='completed').prefetch_related(Prefetch('items',
      queryset=active_prices_and_programs, to_attr='active_prices',),).first()
```

```sql
SELECT 
  ...
FROM 
  orders_order
WHERE 
  orders_order.state = 'completed'
ORDER BY 
  orders_order.id
ASC LIMIT 1;

SELECT 
  ...
FROM 
  events_price
INNER 
  JOIN orders_order_items ON (events_price.id = orders_order_items.price_id)
INNER 
  JOIN events_program ON (events_price.program_id = events_program.id)
WHERE (orders_order_items.order_id IN (29) AND
      events_price.from_date <= '2017–04–29T07:53:00.210537+00:00' :: timestamptz AND
      events_price.to_date > '2017–04–29T07:53:00.210537+00:00' :: timestamptz);
```

Vidimo da Django izvršava dva upita, i prefetch upit sada sadrži prilagodjeni filter koji smo definisali. Da bi dobili aktivne cene možemo koristiti novi atribut `to_attr`:

```shell
>>> [p.program.name for p in o.active_prices]
```

Nema dodatnih upita !

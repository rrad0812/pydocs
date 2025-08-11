
# Django ORM

U ovim tutorijalima ćemo se detaljno pozabaviti Django ORM-om i kako ga efikasno koristiti za interakciju sa relacionim bazama podataka.

## Sadržaj

- [Uvod u Django ORM](#uvod-u-django-orm)  
  Podešavanje osnovnog projekta za sledeće tutorijale u ovom odeljku.

- [Relacija jedan na jedan](#relacija-jedan-na-jedan)  
  Kreiranje relacije jedan na jedan.

- [Relacija jedan na više](#relacija-jedan-na-više)  
  Korišćenje ForeignKey za kreiranje relacije jedan na više.

- [Relacija više na više](#relacija-više-na-više)  
  Kreiranje relacije više na više.

- [Relacija više na više sa dodatnim poljima](#relacija-više-na-više-sa-dodatnim-poljima)  
  Dodatna polja u spojnoj tabeli relacije „više na više“.

- [Metod order_by](#metod-order_by)  
  Kako koristiti metod order_by() za sortiranje rezultata koje vraća QuerySet ( ORDER BY ).

- [Ograničavanje broja vraćenih objekata u QuerySetu](#ograničavanje-broja-vraćenih-objekata-u-querysetu)  
  Kako koristiti isecanje za ograničenje broja objekata koje vraća QuerySet (LIMIT / OFFSET).

- [Operatori startswith, endswith i contains](#operatori-startswith-endswith-i-contains)  
  Odabir podataka na osnovu obrazaca podudaranja ( LIKE ).

- [Operatori IN i NOT IN](#operatori-in-i-not-in)  
  Provera da li se vrednost nalazi na listi vrednosti ( IN ).

- [Operator RANGE i NOT RANGE](#operator-range-i-not-range)
  Korišćenje Django opsega za proveru da li je vrednost unutar opsega inkluzivno ( BETWEEN ).

- [Metod exists](#metod-exists)  
  Vrati True ako upit sadrži bilo koji objekat ili False ako ne sadrži ( EXISTS ).

- [Metod isnull()](#metod-isnull)  
  Provera da li je vrednost NULL ( IS NULL ).

- [Agregati](#agregati)  
  Agregatne metode count, max, min i avg, sum.

- [Grupisanje i agregacija podataka](#grupisanje-i-agregacija-podataka)  
  Grupisanje objekata po kriterijumu u grupe i agregiraj podatke.

- [Dumpdata](#dumpdata)  
  Kako izvesti podatke u fajl sistem.

- [Loaddata](#loaddata)  
  Kako obezbediti početne podatke za modele.

## Uvod u Django ORM

ORM je skraćenica od `objektno-relaciono mapiranje`. ORM je tehnika koja vam omogućava da manipulišete podacima u relacionoj bazi podataka koristeći objektno orijentisano programiranje.

Django ORM vam omogućava da koristite isti Python API za interakciju sa različitim relacionim bazama podataka, uključujući `PostgreSQL`, `MySQL`, `Oracle` i `SQLite`. Pogledajte kompletnu listu podržanih baza podataka [ovde](django_baze_podataka.md).

Django ORM koristi obrazac `aktivnog zapisa`:

- Klasa se mapira na jednu tabelu u bazi podataka. Klasa se često naziva model klasa.
- Objekat klase se preslikava na jedan red u tabeli.

Kada definišete klasu modela, možete pristupiti unapred definisanim metodama za `kreiranje`, `čitanje`, `ažuriranje` i `brisanje` podataka.

Takođe, Django automatski generiše `administratorsku stranu` za upravljanje podacima modela.

Pogledajmo jednostavan primer da bismo videli kako Django ORM funkcioniše.

### Postavljanje osnovnog projekta

Postavićemo osnovni projekat sa novim virtuelnim okruženjem.

Kreiramo novo virtuelno okruženje i u njega instaliramo potrebne pakete:

Kreirajte novo virtuelno okruženje koristeći ugrađeni `venv` modul:

```shell
python -m venv venv
```

Aktivirajte virtuelno okruženje:

```shell
venv\scripts\activate
```

Instalirajte `django` i `django-extensions` paket:

  ```shell
  pip install django django-extensions
  ```

  Paket `django-extensions` pruža neka prilagođena proširenja za Django frejmvork. Koristićemo `django-extensions` paket za prikazivanje generisanog SQL-a pomoću Django ORM-a.

Kreirajte novi Django projekat pod nazivom `django_orm`:

  ```shell
  django-admin startproject django_orm
  ```

Kreirajte `HR` aplikaciju unutar `django_orm` projekta:

```shell
cd django_orm
python manage.py startapp hr
```

Registrujte `HR` i `django_extensions` u `INSTALLED_APPS` u `settings.py` projekta:

```py
INSTALLED_APPS = [
    # ...
    'django_extensions',
    'hr',
]
```

Potom ćemo da uradimo neka podešavanjaPostgreSQL servera baze podataka:

Instalirajte `PostgreSQL` server baze podataka na vaš lokalni računar.

Prijavite se na `PostgreSQL` server baze podataka. Biće vam zatražena lozinka korisnika `postgres`. Imajte na umu da koristite istu lozinku koju ste uneli za `postgres` korisnika tokom instalacije.

```shell
psql -U postgres
Password for user postgres:
```

Kreirajte novu bazu podataka sa imenom `hr` i unesite `exit` da biste zatvorili `psql` program:

```shell
postgres=# create database hr;
CREATE DATABASE 
postgres=# exit
```

Zatim podešavamo parametre veze sa PostgreSQL serverom baze podataka. Konfigurišite vezu sa bazom podataka u `settings.py` projekta `django_orm`:

```py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'hr',
        'USER': 'postgres',
        'PASSWORD': 'POSTGRES_PASSWORD',
        'HOST': 'localhost',
        'PORT': '',
    }
}
```

Imajte na umu da promenite `POSTGRES_PASSWORD` u svoju lozinku.

Instalirajte `psycopg2` paket da biste omogućili Django-u da se poveže sa `PostgreSQL` serverom baze podataka:

```shell
pip install psycopg2
```

Pokrenite Django razvojni server:

```shell
python manage.py runserver
```

Videćete podrazumevanu početnu stranicu Django-a.

### Definisanje modela

Definišite `Employee` klasu u `hr` aplikaciji koja ima dva polja `first_name` i `last_name`:

```py
from django.db import models

class Employee(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)

    def __str__(self):
        return f'{self.first_name} {self.last_name}'
```

Izvršite `migracije` pomoću `makemigrations` komande:

```shell
python manage.py makemigrations

Migrations for 'hr':
  hr\migrations\0001_initial.py
    - Create model Employee
```

Prosledite izmene u bazu podataka pomoću `migrate` komande:

```shell
python manage.py migrate
```

Django kreira mnogo tabela, uključujući i one za ugrađene modele kao što su `User` i `Group`. U ovom tutorijalu ćemo se fokusirati na `Employee` klasu.

Na osnovu `Employee` klase modela, Django ORM kreira tabelu `hr_employee` u bazi podataka.

Django kombinuje `ime aplikacije` i `ime klase` da bi generisao `ime tabele`:

```py
app_modelclass
```

U ovom primeru, naziv aplikacije je `hr`, a naziv klase modela je `Employee`. Stoga, Django kreira tabelu sa nazivom `hr_employee`.

> Imajte na umu da Django konvertuje naziv klase u mala slova pre nego što je doda nazivu aplikacije.

Klasa `Employee` modela ima dva polja `first_name` i `last_name`. Pošto klasa `Employee` nasleđuje od `models.Model` klase, Django automatski dodaje polje `id` kao polje za automatsko povećanje pod nazivom `id`. Stoga, `hr_employee` tabela ima tri kolone `id`, `first_name` i `last_name`.

Da biste interagovali sa `hr_employee` tabelom, možete pokrenuti `shell_plus` komandu koja dolazi iz `django-extensions` paketa.

Imajte na umu da vam Django pruža ugrađenu `shell` komandu. Međutim, `shell_plus` komanda je praktičnija za rad. Na primer, ona automatski učitava modele definisane u projektu i prikazuje generisani SQL.

Pokrenite `shell_plus` komandu sa `--print-sql` opcijom:

```shell
python manage.py shell_plus --print-sql
```

### Unošenje podataka

Kreirajte novi `Employee` objekat i pozovite `save()` metodu za umetanje novog reda u tabelu:

```shell
>>> e = Employee(first_name='John',last_name='Doe')
>>> e.save()
```

```sql  
INSERT INTO "hr_employee" ("first_name", "last_name")
VALUES ('John', 'Doe') RETURNING "hr_employee"."id"
```

U ovom primeru, ne morate da podešavate vrednost za kolonu `id`. Baza podataka automatski generiše njenu vrednost i Django će je prihvatiti kada ubacite novi red u tabelu.

Kao što je prikazano, Django koristi `INSERT ... VALUES ... RETURNING` naredbu da ubaci novi red sa dve kolone `first_name` i `last_name` u tabelu `hr_employee`.

Unesite drugog zaposlenog sa imenom Jane i prezimenom Doe:

```shell
>>> e = Employee(first_name='Jane',last_name='Doe')
>>> e.save()
```

```sql
INSERT INTO "hr_employee" ("first_name", "last_name")
VALUES ('Jane', 'Doe') RETURNING "hr_employee"."id"
```
  
Sada, `hr_employee` tabela ima dva reda sa `ID` vrednostima 1 i 2.

### Izbor podataka

Da biste izabrali sve redove iz `hr_employees` tabele, koristite `all()` metodu ovako:

```shell
>>> Employee.objects.all()
```
  
```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name"
  FROM "hr_employee"
 LIMIT 21
```
  
```shell
<QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
```

Kako ovo funkcioniše?

- Django koristi `SELECT` naredbu da bi izabrao redove iz `hr_employee` tabele.
- Django konvertuje redove u `Employee` objekte i vraća tip `QuerySet` koji sadrži `Employee` objekte.
- Obratite pažnju da je Django dodao `LIMIT` da bi vratio 21 zapis za prikazivanje na shell-u.

Da biste izabrali jedan red po `ID`-u, možete koristiti `get()` metodu. Na primer, sledeći kod vraća zaposlenog sa ID-om 1:

```shell
>>> e = Employee.objects.get(id=1)
```
  
```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name"
  FROM "hr_employee"
 WHERE "hr_employee"."id" = 1
 LIMIT 21
```
  
```shell
>>> e
<Employee: John Doe>
```

Za razliku od `all()` metoda, `get()` metod vraća `Employee` objekat umesto `QuerySet`.

Da biste pronašli zaposlene po njihovom imenu, možete koristiti `filter()` metod objekta `QuerySet`. Na primer, sledeći kod pronalazi zaposlene sa imenom Jane:

```shell
>>> Employee.objects.filter(first_name='Jane')
```
  
```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name"
  FROM "hr_employee"
 WHERE "hr_employee"."first_name" = 'Jane'
 LIMIT 21
```
  
```shell
<QuerySet [<Employee: Jane Doe>]>
```

### Ažuriranje podataka

Izaberite zaposlenog sa ID-om jednakim 2:

```shell
>>> e = Employee.objects.get(id=2)
```

```sql
SELECT "hr_employee"."id",
      "hr_employee"."first_name",
      "hr_employee"."last_name"
  FROM "hr_employee"
WHERE "hr_employee"."id" = 2
LIMIT 21
```
  
Ažurirajte prezime izabranog zaposlenog na Smith:

```shell
>>> e.last_name = 'Smith'
>>> e.save()
```

```sql
UPDATE "hr_employee"
  SET "first_name" = 'Jane',
      "last_name" = 'Smith'
WHERE "hr_employee"."id" = 2
```

```shell
>>> e
<Employee: Jane Smith>
```

### Brisanje podataka

Da biste obrisali instancu modela, koristite `delete()` metodu. Sledeći primer briše zaposlenog sa ID-om 2:

```shell
>>> e.delete()
```
  
```sql  
DELETE
  FROM "hr_employee"
 WHERE "hr_employee"."id" IN (2)
```
  
```shell
(1, {'hr.Employee': 1})
```

Da biste obrisali sve instance modela, koristite `all()` metodu za izbor svih zaposlenih i pozovite `delete()` metodu za brisanje svih izabranih zaposlenih:

```shell
>>> Employee.objects.all().delete()
```

```sql
DELETE
  FROM "hr_employee"
```
  
```shell
(1, {'hr.Employee': 1})
```

Django koristi `DELETE` naredbu bez `WHERE` klauzule da bi obrisao sve redove iz `hr_employee` tabele.

### Rezime uvoda u django-orm

- Django ORM vam omogućava interakciju sa relacionim bazama podataka koristeći Python API.
- Django ORM koristi obrazac aktivnog zapisa, u kome se klasa mapira na tabelu, a objekat na red.
- Koristite `all()` metodu da biste dobili sve redove iz tabele.
- Koristite `get()` metodu za izbor reda po ID-u.
- Koristite `filter()` metodu za filtriranje redova po jednom ili više polja.
- Koristite `save()` metodu za kreiranje novog reda ili ažuriranje postojećeg reda.
- Koristite `delete()` metodu za brisanje jednog ili više redova iz tabele.

[Sadržaj](#sadržaj)

## Relacija jedan na jedan

Relacija `jedan na jedan` definiše vezu između dve tabele, gde svakom redu u jednoj tabeli odgovara samo jedan red u drugoj tabeli.

Na primer, svaki zaposleni ima kontakt i svaki kontakt pripada jednom zaposlenom. Dakle, odnos između zaposlenih i kontakata je odnos `jedan na jedan`.

Da biste kreirali relaciju `jedan na jedan`, koristite `OneToOneField` klasu:

```py
OneToOneField(to, on_delete, parent_link=False, **options)
```

U ovoj sintaksi:

- `to` parametar definiše naziv modela.
- `on_delete` određuje akciju nad kontaktom kada se zaposleni obriše.

Sledeći primer koristi `OneToOneField` klasu za definisanje relacije `jedan na jedan` između `Contact` i `Employee` modela u `models.py`:

```py
from django.db import models

class Contact(models.Model):
    phone = models.CharField(max_length=50, unique=True)
    address = models.CharField(max_length=50)

    def __str__(self):
        return self.phone

class Employee(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    contact = models.OneToOneField(Contact, on_delete=models.CASCADE)

    def __str__(self):
        return f'{self.first_name} {self.last_name}'
```

Klasa `Employee` ima `contact` atribut koji referencira instancu `OneToOneField` klase.

U `OneToOneField`, navodimo `Contact` model i `on_delete` opciju koja definiše ponašanje kada se objekat Employee obriše.

Opcija `on_delete=models.CASCADE` znači da ako se `Employeese` objekat obriše, `Contact` objekat povezan sa njim takođe će biti automatski obrisan.

Imajte na umu da Django ne kreira ograničenje stranog ključa u bazi podataka bez `ON DELETE CASCADE` opcije. Umesto toga, Django ručno obrađuje brisanje u aplikaciji.

Imajte na umu da se ova interna implementacija može promeniti u budućnosti.

### Migriranje modela u bazu podataka

Pripremite migracije pomoću `makemigrations` komande:

```shell
python manage.py makemigrations

Migrations for 'hr':
  hr\migrations\0002_contact_employee_contact.py
    - Create model Contact
    - Add field contact to employee
```

Primenite migracije na bazu podataka pomoću `migrate` komande:

```shell
python manage.py migrate

Operations to perform:
  Apply all migrations: admin, auth, contenttypes, hr, sessions
Running migrations:
  Applying hr.0002_contact_employee_contact... OK
```

Iza kulisa, Django kreira dve tabele `hr_contact` i `hr_employee` u bazi podataka:

Tabela `hr_employee` ima `contact_id` kolonu koja je strani ključ koji se povezuje sa `ID`-om ( primarnim ključem ) tabele `hr_contact`.

### Povezivanje kontakta sa zaposlenim

Da biste interagovali modelima `Employee` i `Contact`, pokrećete `shell_plus` komandu sa `--print-sql` opcijom:

```shell
python manage.py shell_plus --print-sql
```

Opcija `--print-sql` prikazuje SQL komandu koju Django izvršava.

Kreirajte novi `Employee` objekat i sačuvajte ga u bazi podataka:

```shell
>>> e = Employee(first_name='John',last_name='Doe')
>>> e.save()
```

```sql
INSERT INTO "hr_employee" ("first_name", "last_name", "contact_id")
VALUES ('John', 'Doe', NULL) 
RETURNING "hr_employee"."id"
```

Kreirajte i sačuvajte novi kontakt u bazi podataka:

```shell
>>> c = Contact(phone='40812345678', address='101 N 1st Street, San Jose, CA')
>>> c.save()
```

```sql
INSERT INTO "hr_contact" ("phone", "address")
VALUES ('40812345678', '101 N 1st Street, San Jose, CA') 
RETURNING "hr_contact"."id"
```

Povežite kontakt sa zaposlenim:

```shell
>>> e.contact = c
>>> e.save()
```

Django ažurira vrednost `contact_id` kolone u `hr_employee` tabeli na vrednost `id` kolone u `hr_contact`tabeli.

```sql
UPDATE "hr_employee"
  SET "first_name" = 'John',
      "last_name" = 'Doe',
      "contact_id" = 1
WHERE "hr_employee"."id" = 3
```

### Dobijanje podataka iz odnosa jedan na jedan

Pronađite zaposlenog sa imenom John Doe:

```shell
>>> e = Employee.objects.filter(first_name='John',last_name='Doe').first()
<Employee: John Doe>
```

Django izvršava `SELECT` naredbu koja dobija red sa `first_name` 'John' and `last_name` 'Doe'.

Može `hr_employee` imati više zaposlenih sa istim imenom i prezimenom. Stoga, `filter()` vraća `QuerySet`.

Da bismo dobili prvi red u `QuerySet`, koristimo `first()` metodu. `first()` metoda vraća jednu instancu klase `Employee`.

  ```sql
  SELECT "hr_employee"."id",
        "hr_employee"."first_name",
        "hr_employee"."last_name",
        "hr_employee"."contact_id"
    FROM "hr_employee"
  WHERE ("hr_employee"."first_name" = 'John' AND "hr_employee"."last_name" = 'Doe')
  ORDER BY "hr_employee"."id" ASC
  LIMIT 1
  ```

Imajte na umu da upit ne dobija kontakt podatke iz `hr_contact` tabele. On dobija podatke samo iz `hr_employee` tabele.

Kada pristupite `contact` atributu zaposlenog:

```shell
>>> e.contact
```

... Django izvršava drugu `SELECT` naredbu da bi dobio podatke iz `hr_contact` tabele:

```sql
SELECT "hr_contact"."id",
       "hr_contact"."phone",
       "hr_contact"."address"
  FROM "hr_contact"
 WHERE "hr_contact"."id" = 1
 LIMIT 21
```

Sledeće upit dobija kontakt sa ID-om 1:

```shell
>>> c = Contact.objects.get(id=1)
```

Kada povezujete kontakt sa zaposlenim, možete pristupiti `employee` iz `contact` objekta:

```shell
>>> c.employee
<Employee: John Doe>
```

Django izvršava `SELECT` naredbu da bi dobio podatke iz `hr_employee` tabele:

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id"
  FROM "hr_employee"
 WHERE "hr_employee"."contact_id" = 1
 LIMIT 21
```

> [!Note]
>
> Imajte na umu da `Contact` klasa nema `employee` atribut. Međutim, možete mu pristupiti ako je kontakt povezan.

Hajde da napravimo još jedan kontakt koji se ne povezuje ni sa jednim employee objektom:

```shell
>>> c = Contact(phone='4081111111',address='202 N 1st Street, San Jose, CA')
>>> c.save()
```

Django će izvršiti sledeću `INSERT` naredbu:

```sql
INSERT INTO "hr_contact" ("phone", "address")
VALUES ('4081111111', '202 N 1st Street, San Jose, CA') 
RETURNING "hr_contact"."id"
```

Ako pronađete kontakt koji se ne povezuje ni sa jednim zaposlenim i pokušate da pristupite zaposlenom, dobićete `RelatedObjectDoesNotExist` izuzetak.

### Izbor povezanih objekata

Kreirajte novog zaposlenog:

```shell
>>> e = Employee(first_name='Jane',last_name='Doe')
>>> e.save()
```

```sql
INSERT INTO "hr_employee" ("first_name", "last_name", "contact_id")
VALUES ('Jane', 'Doe', NULL) RETURNING "hr_employee"."id"
Execution time: 0.003079s [Database: default]
```

Pokupite sve zaposlene:

```shell
>>> Employee.objects.all()
```

Django vraća dva zaposlena:

```shell
<QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
```

Ako morate da prikažete sve zaposlene, kao i njihove kontakte na istoj stranici, onda imate problem `N+1` upita:

- Potreban vam je jedan upit da biste dobili sve zaposlene (N zaposlenih).
- Potrebno vam je N upita da biste izabrali povezani kontakt svakog zaposlenog.

Da biste ovo izbegli, možete poslati upit za sve zaposlenima i kontakte pomoću jednog upita koristeći `select_related()` metodu:

```shell
>>> Employee.objects.select_related('contact').all()
```

U ovom slučaju, Django izvršava sledeću `LEFT JOIN` naredbu:

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_contact"."id",
       "hr_contact"."phone",
       "hr_contact"."address"
  FROM "hr_employee"
  LEFT OUTER JOIN "hr_contact"
    ON ("hr_employee"."contact_id" = "hr_contact"."id")
 LIMIT 21
 ```

Django koristi funkciju `LEFT OUTER JOIN` koja vraća sve zaposlene iz `hr_employee` tabele i kontakte povezane sa izabranim zaposlenima:

```shell
<QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
```

[Sadržaj](#sadržaj)

## Relacija jedan na više

U relaciji `jedan na više`, red u tabeli je povezan sa jednim ili više redova u drugoj tabeli. Na primer, depatment (odeljenje) može imati jednog ili više employees (zaposlenih) i svaki employee  pripada jednom departmentu.

Relacija između departmenta i employee je `jedan na više`. Suprotno tome, relacija između employee i departmenta je `više na jedan`.

Da biste kreirali relaciju `jedan na više` u Django-u, koristite `ForeignKey`. Na primer, sledeći kod koristi `ForeignKey` da bi kreirao relaciju `jedan na više` između `Department` i `Employee` modela:

```py
from django.db import models

class Contact(models.Model):
  phone = models.CharField(max_length=50, unique=True)
  address = models.CharField(max_length=50)

  def __str__(self):
    return self.phone

class Department(models.Model):
  name = models.CharField(max_length=255)
  description = models.TextField(null=True, blank=True)

  def __str__(self):
    return self.name

class Employee(models.Model):
  first_name = models.CharField(max_length=100)
  last_name = models.CharField(max_length=100)

  contact = models.OneToOneField(
    Contact,
    on_delete=models.CASCADE,
    null=True
  )

  department = models.ForeignKey(
    Department,
    on_delete=models.CASCADE
  )

  def __str__(self):
      return f'{self.first_name} {self.last_name}'
```

Kako ovo funkcioniše?

Definišite `Department` klasu modela.

Izmenite `Employee` klasu dodavanjem odnosa `jedan na više` koristeći `ForeignKey`:

```py
department = models.ForeignKey(
  Department,
  on_delete=models.CASCADE
)
```

U `ForeignKey`, prosleđujemo `Department` kao prvi argument i `on_delete` ključnu reč kao drugi argument. `on_delete=models.CASCADE` označava da ako se department obriše, svi zaposleni povezani sa tim odeljenjem takođe se brišu.

> [!Note]
>
> Imajte na umu da polje definišete sa `ForeignKey` na strani `"više"` relacije.

Izvršite migracije pomoću `makemigrations` komande:

```shell
python manage.py makemigrations
```

Django će izdati sledeću poruku:

> It is impossible to add a non-nullable field 'department' to employee without specifying a default. This is because the database needs something to populate existing rows.
>
> Please select a fix:
>
> 1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
> 2) Quit and manually define a default value in models.py.
>
> Select an option:

U `Employee` modelu, polje `department` definišemo kao polje koje se ne može nulirati. Stoga, Django-u je potrebna podrazumevana vrednost za ažuriranje `department` polja (ili `department_id` kolone) za postojeće redove u tabeli baze podataka.

Čak i ako `hr_employees` tabela nema redove, Django takođe zahteva da unesete podrazumevanu vrednost.

Kao što je jasno prikazano u poruci, Django vam pruža dve opcije. Potrebno je da unesete 1 ili 2 da biste izabrali odgovarajuću opciju.

Ako izaberete prvu opciju, Django zahteva jednokratnu podrazumevanu vrednost. Ako unesete 1, Django će prikazati komandnu liniju Python-a:

```shell
>>>
```

U ovom slučaju, možete koristiti bilo koju validnu vrednost u Pajtonu, npr `None`:

```shell
>>> None
```

Kada unesete podrazumevanu vrednost, Django će izvršiti migracije ovako:

```shell
Migrations for 'hr':
  hr\migrations\0002_department_employee_department.py
    - Create model Department
    - Add field department to employee
```

U bazi podataka, Django kreira `hr_department` i dodaje `department_id` kolonu u `hr_employee` tabelu. `department_id` kolona tabele `hr_employee` je povezana sa `id` kolonom tabele `hr_department`.

Ako izaberete drugu opciju unosom broja 2, Django će vam omogućiti da ručno definišete podrazumevanu vrednost za polje `department`. U ovom slučaju, možete dodati podrazumevanu vrednost polju department `models.py` na ovaj način:

```py
department = models.ForeignKey(
  Department,
  on_delete=models.CASCADE,
  default=None
)
```

Nakon toga, možete izvršiti migracije pomoću `makemigrations` komande:

```shelll
python manage.py makemigrations
```

Prikazaće sledeći izlaz:

```shell
Migrations for 'hr':
  hr\migrations\0002_department_employee_department.py
    - Create model Department
    - Add field department to employee
```

Ako `hr_employee` tabela ima redove, potrebno je da ih sve uklonite pre nego što migrirate nove migracije:

```shell
python manage.py shell_plus
>>> Employee.objects.all().delete()
```

Zatim možete izvršiti izmene u bazi podataka pomoću migrate komande:

```shell
python manage.py migrate

Operations to perform:
  Apply all migrations: admin, auth, contenttypes, hr, sessions
Running migrations:
  Applying hr.0002_department_employee_department... OK
```

Drugi način da se ovo reši je resetovanje migracija koje ćemo obraditi u tutorijalu za resetovanje migracija.

### Interakcija sa modelima

Pokrenite `shell_plus` komandu:

```shell
python manage.py shell_plus
```

Kreirajte novo odeljenje sa nazivom IT:

```shell
>>> d = Department(name='IT',description='Information Technology')
>>> d.save()
```

Kreirajte dva zaposlena i dodelite ih odeljenje IT:

```shell
>>> e = Employee(first_name='John',last_name='Doe',department=d)
>>> e.save()
>>> e = Employee(first_name='Jane',last_name='Doe',department=d)
>>> e.save()
```

Pristupite `department` objektu iz `employee` objekta:

```shell
>>> e.department 
<Department: IT>
>>> e.department.description
'Information Technology'
```

Dobijte sve zaposlene u odeljenju koristeći `employee_set` atribut ovako:

```shell
>>> d.employee_set.all()
<QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
```

Imajte na umu da nismo definisali `employee_set` svojstvo u `Department` modelu. Interno, Django je automatski dodao `employee_set` svojstvo modelu `Department` kada smo definisali odnos `jedan na više` koristeći `ForeignKey`.

Metod `all()` vraća `employee_set` rezultat u `QuerySet`-u koji sadrži sve zaposlene koji pripadaju IT odeljenju.

### Korišćenje select_related() za spajanje zaposlenog sa odeljenjem

Izađite iz `shell_plus` i ponovo ga izvršite. Ovaj put dodajemo `--print-sql` opciju za prikaz generisanog SQL-a koji će Django izvršiti nad bazom podataka:

```shell
python manage.py shell_plus --print-sql
```

Sledeće vraća prvog zaposlenog:

```shell
>>> e = Employee.objects.first()
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 ORDER BY "hr_employee"."id" ASC
 LIMIT 1
```

Da biste pristupili odeljenju prvog zaposlenog, koristite `department` atribut:

```shell
>>> e.department
```

```sql
SELECT "hr_department"."id",
       "hr_department"."name",
       "hr_department"."description"
  FROM "hr_department"
 WHERE "hr_department"."id" = 1
 LIMIT 21
```

```shell
<Department: IT>
```

U ovom slučaju, Django izvršava dva upita. Prvi upit bira prvog zaposlenog, a drugi upit bira odeljenje izabranog zaposlenog.

Ako izaberete N zaposlenih da biste ih prikazali na veb stranici, onda morate da izvršite N + 1 upit da biste dobili i zaposlene i njihova odeljenja. Prvi upit (1) bira N zaposlenih, a N upita bira N odeljenja za svakog zaposlenog. Ovaj problem je poznat kao "problem N + 1 upita".

Da biste rešili problem sa N + 1 upitom, možete koristiti `select_related()` metod za izbor i zaposlenih i odeljenja pomoću jednog upita. Na primer:

```shell
>>> Employee.objects.select_related('department').all()
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",   
       "hr_employee"."last_name",    
       "hr_employee"."contact_id",   
       "hr_employee"."department_id",
       "hr_department"."id",
       "hr_department"."name",
       "hr_department"."description"
  FROM "hr_employee"
 INNER JOIN "hr_department"
    ON ("hr_employee"."department_id" = "hr_department"."id")
 LIMIT 21
```

```shell
<QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
```

U ovom primeru, Django izvršava samo jedan upit koji spaja tabele `hr_employeeand` i `hr_department`.

### Rezime relacije jedan na više

- U relaciji `jedan na više`, red u tabeli je povezan sa jednim ili više redova u drugoj tabeli.
- Koristi se `ForeignKey` za uspostavljanje odnosa `jedan na više` između modela u Django-u.
- Definišite `ForeignKey` u modelu na strani `više` relacije.
- Koristite `select_related()` metodu za spajanje dve ili više tabela u relacijama `jedan na više`.

[Sadržaj](#sadržaj)

## Relacija više na više

U relaciji `više na više`, više redova u jednoj tabeli je povezano sa više redova u drugoj tabeli.

Na primer, zaposleni može imati više programa kompenzacije, a programu kompenzacije može pripadati više zaposlenih.

Stoga, više redova u tabeli zaposlenih povezano je sa više redova u tabeli kompenzacija. Dakle, odnos između zaposlenih i programa kompenzacija je relacija `više na više`.

Tipično, relacione baze podataka ne implementiraju direktan odnos `više na više` između dve tabele. Umesto toga, koriste treću tabelu, tabelu spajanja, da bi uspostavile dva odnosa `jedan na više` između dve tabele i tabele spajanja.

Tabela `hr_employee_compensations` je tabela spajanja. Ima dva strana ključa `employee_id` i `compensation_id`.

Strani `employee_id` ključ referencira na `id` tabele `hr_employee`, a strani ključ `compensation_id`  referencira na `id` u `hr_compensation` tabeli.

Obično vam nije potrebna `id` kolona u `hr_employee_compensations` tabeli kao primarni ključ i koristite `employee_id` i `compensation_id` kao složeni primarni ključ. Međutim, Django uvek kreira `id` kolonu kao primarni ključ za tabelu spajanja.

Takođe, Django kreira jedinstveno ograničenje koje uključuje kolone `employee_id` i `compensation_id`. Drugim rečima, u tabeli `hr_employee_compensations` neće biti duplih parova vrednosti `employee_id` i `compensation_id`.

Da biste kreirali relaciju `više na više` u Django-u, koristite `ManyToManyField`. Na primer, sledeći kod koristi `ManyToManyField` da bi kreirao relaciju `više na više` između `Employee` i `Compensation` modela:

```py
# ...
class Compensation(models.Model):
  name = models.CharField(max_length=255)

  def __str__(self):
      return self.name


class Employee(models.Model):
  first_name = models.CharField(max_length=100)
  last_name = models.CharField(max_length=100)

  contact = models.OneToOneField(
      Contact,
      on_delete=models.CASCADE,
      null=True
  )

  department = models.ForeignKey(
      Department,
      on_delete=models.CASCADE
  )

  compensations = models.ManyToManyField(Compensation)

  def __str__(self):
      return f'{self.first_name} {self.last_name}'
```

Kako ovo funkcioniše?

Definišimo novu `Compensation` klasu modela koja proširuje `models.Model` klasu.

Dodajemo `compensations` polje klasi `Employee`. `compensations` polje koristi `ManyToManyField` da bi uspostavilo odnos `više na više` između klasa `Employee` i `Compensation`.

Da bismo proširili izmene modela u bazu podataka, pokrećemo `makemigrations` komandu:

```shell
python manage.py makemigrations
```

U šelu će biti odštampano:

```shell
Migrations for 'hr':
  hr\migrations\0004_compensation_employee_compensations.py
    - Create model Compensation
    - Add field compensations to employee
```

Izvršimo `migrate` komandu:

```shell
python manage.py migrate

Operations to perform:
  Apply all migrations: admin, auth, contenttypes, hr, sessions
Running migrations:
  Applying hr.0004_compensation_employee_compensations... OK  
```

Django je kreirao dve nove tabele `hr_compensation` i tabelu spajanja `hr_employee_compensations`.

### Kreiranje podataka

Pokrenimo `shell_plus` komandu:

```shell
python manage.py shell_plus
```

Kreirajmo tri programa kompenzacije, uključujući "Stock", "Bonuses" i "Profit Sharing":

  ```shell
  >>> c1 = Compensation(name='Stock')
  >>> c1.save()
  >>> c2 = Compensation(name='Bonuses') 
  >>> c2.save()
  >>> c3 = Compensation(name='Profit Sharing')  
  >>> c3.save()
  >>> Compensation.objects.all()
  <QuerySet [<Compensation: Stock>, <Compensation: Bonuses>, <Compensation: Profit Sharing>]>
  ```

Nabavimo zaposlenog sa imenom John i prezimenom Doe:

```shell
>>> e = Employee.objects.filter(first_name='John',last_name='Doe').first()
>>> e
<Employee: John Doe>
```

### Dodavanje kompenzacija zaposlenima

Upišimo John Doe u programe kompenzacije "stock" ( c1 ) i "bonuses" ( c2 ) koristeći `add()` metod atributa `compensations` i `save()` metod objekta `Employee`:

  ```shell
  >>> e.compensations.add(c1)
  >>> e.compensations.add(c2) 
  >>> e.save()
  ```

Pristupimo svim `compensations` programima za  "John Doe" koristeći `all()` metod atributa `compensations`:

```shell
>>> e.compensations.all()
<QuerySet [<Compensation: Stock>, <Compensation: Bonuses>]>
```

Kao što je jasno prikazano na izlazu, "John Doe" ima dva programa kompenzacije.

Upišimo "Jane Doe" u tri programa kompenzacije, uključujući akcije, bonuse i podelu dobiti:

```shell
>>> e = Employee.objects.filter(first_name='Jane',last_name='Doe').first()
>>> e 
<Employee: Jane Doe>
>>> e.compensations.add(c1)
>>> e.compensations.add(c2) 
>>> e.compensations.add(c3) 
>>> e.save()
>>> e.compensations.all()
```

```shell
<QuerySet [<Compensation: Stock>, <Compensation: Bonuses>, <Compensation: Profit Sharing>]>
```

Interno, Django je ubacio identifikacione brojeve zaposlenih i kompenzacija u tabelu spajanja:

| id  | employee_id | compensation_id |
|-----|-------------|-----------------|
|  1  | 5           | 1               |
|  2  | 5           | 2               |
|  3  | 6           | 1               |
|  4  | 6           | 2               |
|  5  | 6           | 3               |

(5 rows)

Pronađimo sve zaposlene koji su bili uključeni u plan nadoknade akcijama koristeći `employee_set` atribut objekta `Compensation`:

  ```shell
  >>> c1
  <Compensation: Stock>
  >>> c1.employee_set.all()
  <QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
  ```

Vraćeno je dva zaposlena kako je i očekivano.

Možemo koristiti `employee_set` atribut da pronađemo sve zaposlene koji imaju program kompenzacije za učešće u dobiti:

```shell
>>> c3
<Compensation: Profit Sharing>
>>> c3.employee_set.all()
<QuerySet [<Employee: Jane Doe>]>
```

Vraćen je jedan zaposleni.

Django vam omogućava da vršite upite kroz celu relaciju. Na primer, možete pronaći sve zaposlene koji imaju kompenzaciju sa ID-om 1:

```shell
>>> Employee.objects.filter(compensations__id=1) 
<QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
```

Ili sa imenom kompenzacije "Profit Sharing":

```shell
>>> Employee.objects.filter(compensations__name="Profit Sharing") 
<QuerySet [<Employee: Jane Doe>]>
```

### Ukidanje kompenzacija zaposlenima

Da biste uklonili program kompenzacije zaposlenom, koristite `remove()` metod atributa `compensations` objekta Employee. Na primer:

Pronađimo zaposlenog čije je ime Jane Doe:

```shell
>>> e = Employee.objects.filter(first_name='Jane',last_name='Doe').first()
<Employee: Jane Doe>
```

Uklonimo "profit sharing" kompenzaciju ( c3 ) iz za Jane Doe i sačuvajte izmene:

```shell
>>> e.compensations.remove(c3)
>>> e.save()
```

Nabavimo sve programe kompenzacije od Jane Doe:

```shell
>>> e.compensations.all()
<QuerySet [<Compensation: Stock>, <Compensation: Bonuses>]>
```

Sada su Jane Doe preostala dva programa kompenzacije.

### Rezime relacije više na više

- U relaciji `više na više`, više redova u jednoj tabeli je povezano sa više redova u drugoj tabeli.
- Relacione baze podataka koriste tabelu za spajanje da bi uspostavile odnos `više na više` između dve tabele.
- Koristite `ManyToManyField` za modeliranje relacije `više na više` između modela u Django-u.

[Sadržaj](#sadržaj)

## Relacija više na više sa dodatnim poljima

U relaciji `više na više`, više redova u jednoj tabeli je povezano sa više redova u drugoj tabeli. Da bi se uspostavio odnos `više na više`, relacione baze podataka koriste treću tabelu koja se naziva tabela spajanja i kreiraju se dva odnosa `jedan na više` sa izvornim tabelama.

Tipično, tabela spajanja sadrži vrednosti identifikatora izvornih tabela tako da redovi u jednoj tabeli mogu biti povezani sa redovima u drugoj tabeli.

Ponekad ćete možda želeti da dodate dodatna polja u tabelu spajanja. Na primer, svaki zaposleni može imati više poslova tokom svoje karijere.

Da biste pratili kada zaposleni preuzima posao, možete dodati polja `begin_date` i `end_date` u tabelu spajanja.

Da biste to uradili u Django-u, koristite za `ManyToManyField` argument  `through`.

Na primer, sledeće pokazuje kako povezati `Employee` sa više `Job` putem `Assigment` modela:

```py
class Employee(models.Model):
  # ...

class Job(models.Model):
  title = models.CharField(max_length=255)
  employees = models.ManyToManyField(Employee, through='Assignment')

  def __str__(self):
    return self.title

class Assignment(models.Model):
  employee = models.ForeignKey(Employee, on_delete=models.CASCADE)
  position = models.ForeignKey(Job, on_delete=models.CASCADE)
  begin_date = models.DateField()
  end_date = models.DateField(default=date(9999, 12, 31))
```

Kako to funkcioniše?

Definišite `Job` model, dodajte `employees` atribut `Job` modelu koji koristi `ManyToManyField` i prosledite mu `Assignment`, i `through` kao argument.

Definišite `Assignment` klasu koja ima dva strana ključa, jedan je povezan sa `Employee` modelom, a drugi je povezan sa `Job` modelom. Takođe, dodajte dodatne atribute `begin_date` i `end_date` modelu `Assignment`.

Pokrenite `makemigrations` da biste napravili nove migracije:

```shell
python manage.py makemigrations

Migrations for 'hr':
  hr\migrations\0005_assignment_job_assignment_job.py
    - Create model Assignment
    - Create model Job
    - Add field job to assignment
```

Izvršite `migrate` komandu da biste primenili promene na bazu podataka:

```shell
python manage.py migrate

Operations to perform:
  Apply all migrations: admin, auth, contenttypes, hr, sessions
Running migrations:
  Applying hr.0005_assignment_job_assignment_job... OK
```

Iza kulisa, Django kreira tabele `hr_job` i `hr_assignment` u bazi podataka:

`hr_assignment` je tabela spajanja. Pored polja `employee_id` i `position_id`, ona ima `begin_date` i `end_date` dodatna polja.

### Kreiranje novih radnih mesta

Pokrenimo `shell_plus` komandu:

```shell
python manage.py shell_plus
```

Kreirajte tri nove pozicije (posla):

```shell
>>> j1 = Job(title='Software Engineer I')
>>> j1.save()
>>> j2 = Job(title='Software Engineer II') 
>>> j2.save() 
>>> j3 = Job(title='Software Engineer III')
>>> j3.save()
```

```shell
>>> Job.objects.all()
<QuerySet [<Job: Software Engineer I>, <Job: Software Engineer II>, <Job: Software Engineer III>]>
```

### Kreiranje instanci za spojni model

Pronađimo zaposlene sa imenom "John Doe" i "Jane Doe":

```shell
>>> e1 = Employee.objects.filter(first_name='John',last_name='Doe').first()
>>> e1
<Employee: John Doe>
>>> e2 = Employee.objects.filter(first_name='Jane', last_name='Doe').first()
>>> e2
<Employee: Jane Doe>
```

Kreirajmo instance spojnog modela ( `Assignment` ):

```shell
>>> from datetime import date
>>> a1 = Assignment(employee=e1,job=j1, begin_date=date(2019,1,1), end_date=date(2021,12,31))
>>> a1.save()
>>> a2 = Assignment(employee=e1,job=j2, begin_date=date(2022,1,1))
>>> a2.save()
>>> a3 = Assignment(employee=e2, job=j1, begin_date=date(2019, 3, 1))
>>> a3.save()
```

Pronađite zaposlene koji zauzimaju "Software Engineer I" poziciju ( p1 ):

```shell
>>> j1.employees.all()
<QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
```

Iza kulisa, Django izvršava sledeći upit:

```sql
SELECT
  "hr_employee"."id",
  "hr_employee"."first_name",
  "hr_employee"."last_name",
  "hr_employee"."contact_id",
  "hr_employee"."department_id"
FROM "hr_employee"
INNER JOIN "hr_assignment"
  ON ("hr_employee"."id" = "hr_assignment"."employee_id")
WHERE "hr_assignment"."job_id" = 1
```

Slično tome, možete pronaći sve zaposlene koji zauzimaju "Software Engineer II" poziciju ( p2 ):

```shell
>>> j2.employees.all()
<QuerySet [<Employee: John Doe>]>
```

### Uklanjanje instanci spojnog modela

Uklonite "Jane Doe" ( e2 ) iz "Software Engineer II" posla koristeći `remove()` metodu:

```shell
>>> j2.employees.remove(e2) 
```

Uklonite sve zaposlene sa "Software Engineer I" posla koristeći `clear()` metodu:

```shell
>>> j1.employees.clear() 
```

Posao "j1" sada ne bi trebalo da ima zaposlene:

```shell
>>> j1.employees.all() 
<QuerySet []>
```

### Rezime relacije više na više sa dodatnim poljima

Koristite `through` argument u `ManyToManyField` da biste napravili realciju `više na više` sa dodatnim poljima.

[Sadržaj](#sadržaj)

## Metod order_by

Prilikom definisanja modela, možete odrediti podrazumevani redosled `QuerySet` rezultata koje vraća model korišćenjem `ordering` opcije u klasi modela `Meta`.

Koristićemo model `Employee` za demonstraciju. Model `Employee` se mapira na tabelu `hr_employee` u bazi podataka:

Na primer, da biste sortirali zaposlenog po "first_name" i "last_name" po abecednom redu, možete da navedete opciju `first_name` i `last_name` u polju `ordering` u  `Meta` klasi na sledeći način:

Da biste sortirali vrednosti u opadajućem redosledu, dodajete `-` znak ispred imena polja. Na primer, sledeći primer koristi `ordering` opciju za sortiranje zaposlenih po "first_name" u opadajućem redosledu i "last_name" u rastućem redosledu:

```py
class Employee(models.Model):
    # ...

    class Meta:
        ordering = ['-first_name', 'last_name']
```

Takođe, možete koristiti izraz upita `F()` da biste učinili da se NULL vrednosti pojave prve ili poslednje u sortiranom rezultatu:

```py
from django.db.models import F

class Employee(models.Model):
    # ...

    class Meta:
        ordering = [F('first_name').asc(nulls_last=True)]
```

Da biste poništili podrazumevani redosled sortiranja, koristite `order_by()` metod `QuerySet`:

```py
order_by(*fields)
```

Sledeći primer koristi `order_by()` za sortiranje zaposlenih po imenu u rastućem redosledu:

```shell
>>> Employee.objects.all().order_by('first_name')
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 ORDER BY "hr_employee"."first_name" ASC
 LIMIT 21
```

```shell
<QuerySet [<Employee: Aaron Pearson>, <Employee: Adam Crane>, <Employee: Adam Stewart>, <Employee: Adrienne Green>, <Employee: Alan Johnson>, <Employee: Alexa West>, <Employee: Alicia Wyatt>, <Employee: Amanda Benson>, <Employee: Amber Brown>, <Employee: Amy Lopez>, <Employee: Amy Lee>, <Employee: Andre Perez>, <Employee: 
Andrea Mcintosh>, <Employee: Andrew Guerra>, <Employee: Andrew Dixon>, <Employee: Ann Chang>, <Employee: Anne Odom>, <Employee: Anthony Welch>, <Employee: Anthony Fuentes>, <Employee: Ashley Brown>, '...(remaining elements truncated)...']>
```

Kao i kod `ordering` opcije, možete koristiti `order_by()` metodu za sortiranje zaposlenog po imenu u opadajućem redosledu:

```shell
>>> Employee.objects.all().order_by('-first_name')
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 ORDER BY "hr_employee"."first_name" DESC
 LIMIT 21
```

```shell
<QuerySet [<Employee: William Wise>, <Employee: Wendy Reilly>, <Employee: Victoria Forbes>, <Employee: Victoria Schneider>, <Employee: Vicki Baker>, <Employee: Veronica Blackburn>, <Employee: Vanessa Allen>, <Employee: Valerie Nguyen>, <Employee: Tyler Coleman>, <Employee: Tyler Briggs>, <Employee: Troy Ashley>, <Employee: Travis Goodwin>, <Employee: Tony Jordan>, <Employee: Todd Evans>, <Employee: Timothy Dillon>, <Employee: Timothy Mckay>, <Employee: Timothy Williams>, <Employee: Timothy Lewis>, <Employee: Tiffany Holt>, <Employee: Tiffany Jackson>, '...(remaining elements truncated)...']>
```

Takođe `order_by()` vam omogućava da sortirate rezultate po više polja. Na primer, sledeći primer koristi `order_by()` za sortiranje zaposlenih po imenu i prezimenu:

```shell
>>> Employee.objects.all().order_by('first_name','last_name')
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 ORDER BY "hr_employee"."first_name" ASC,
          "hr_employee"."last_name" ASC
 LIMIT 21
```

```shell
<QuerySet [<Employee: Aaron Pearson>, <Employee: Adam Crane>, <Employee: Adam Stewart>, <Employee: Adrienne Green>, <Employee: Alan Johnson>, <Employee: Alexa West>, <Employee: Alicia Wyatt>, <Employee: Amanda Benson>, <Employee: Amber Brown>, <Employee: Amy Lee>, <Employee: Amy Lopez>, <Employee: Andre Perez>, <Employee: 
Andrea Mcintosh>, <Employee: Andrew Dixon>, <Employee: Andrew Guerra>, <Employee: Ann Chang>, <Employee: Anne Odom>, <Employee: Anthony Fuentes>, <Employee: Anthony Welch>, <Employee: Ashley Brown>, '...(remaining elements truncated)...']>
```

Da biste nasumično poređali, možete koristiti znak pitanja `?` ovako:

```shell
>>> Employee.objects.all().order_by('?')
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 ORDER BY RANDOM() ASC
 LIMIT 21
```

```shell
<QuerySet [<Employee: Daniel Meyer>, <Employee: Todd Evans>, <Employee: Roger Robinson>, <Employee: Dwayne Williams>, <Employee: Michael Murphy>, <Employee: Daniel Friedman>, <Employee: Claudia Aguilar>, <Employee: Craig Hunter>, <Employee: Amanda Benson>, <Employee: Renee Wright>, <Employee: Wendy Reilly>, <Employee: Jamie Jackson>, <Employee: Philip Jones>, <Employee: Kelly Stewart>, <Employee: Barbara Vincent>, <Employee: Drew Gonzalez>, <Employee: Derek Owens>, <Employee: Lauren Mcdonald>, <Employee: Perry Rodriguez>, <Employee: Matthew Hernandez>, '...(remaining elements truncated)...']>
```

Operacija `order_by('?')` može biti spora i skupa, u zavisnosti od baze podataka koju koristite. Gore navedeno je SQL iz PostgreSQL-a. Druge baze podataka mogu imati drugačije implementacije.

### Ređanje po srodnim modelima

`order_by()` vam omogućava da sortirate po polju u povezanom modelu. Sintaksa sortiranja za povezani model je:

```py
Entity.objects.order_by(related_model__field_name)
```

Za demonstraciju ćemo koristiti modele `Employee` i `Department`. Modeli `Employee` i `Department` se mapiraju na tabele `Employee` i `hr_employee` i `hr_department`:

Sledeći primer koristi `order_by()` za sortiranje zaposlenih po nazivima njihovih odeljenja:

```shell
>>> Employee.objects.all().order_by('department__name')
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 INNER JOIN "hr_department"
    ON ("hr_employee"."department_id" = "hr_department"."id")
 ORDER BY "hr_department"."name" ASC
 LIMIT 21
```

```sql
<QuerySet [<Employee: Brandy Morris>, <Employee: Jay Carlson>, <Employee: Jessica Lewis>, <Employee: Amanda Benson>, <Employee: Jacqueline Weaver>, <Employee: Patrick Griffith>, <Employee: Adam Stewart>, <Employee: Tiffany Holt>, <Employee: Amber Brown>, <Employee: Martin Raymond>, <Employee: Kyle Pratt>, <Employee: Cheryl Thomas>, <Employee: Linda Garcia>, <Employee: Jeanette Hendrix>, <Employee: Kimberly Gallagher>, <Employee: Kelly Stewart>, <Employee: Alan Johnson>, <Employee: 
William Wise>, <Employee: Debra Webb>, <Employee: Ryan Garcia>, '...(remaining elements truncated)...']>
```

Generisani SQL pokazuje da prilikom uređivanja po polju u povezanom modelu, Django koristi spajanje koje spaja tabelu trenutnog modela ( Employee) sa tabelom povezanog modela ( Department).

### Rezime order_by

- Koristite Django `order_by()` za sortiranje podataka po jednom ili više polja u rastućem ili opadajućem redosledu.

[Sadržaj](#sadržaj)

## Ograničavanje broja vraćenih objekata u QuerySetu

U praksi, retko dobijate sve redove iz jedne ili više tabela u bazi podataka. Umesto toga, dobijate podskup redova za prikazivanje na veb stranici.

Django koristi sintaksu isecanja da bi ograničio `QuerySet` na određeni broj objekata. Iza kulisa, Django izvršava SQL `SELECT` izraz sa `LIMIT` i `OFFSET` klauzulama.

Pretpostavimo da želite da pristupite prvih 10 redova u tabeli, možete koristiti sledeće isecanje:

```py
Entity.objects.all()[:10]
```

Pošto tabela čuva redove u neodređenom redosledu, "prvih 10 objekata" postaje nepredvidivo.

Stoga, uvek bi trebalo da sortirate rezultate pre nego što raščlanite upit. Na primer, sledeći kod dobija prvih 10 redova poređanih po field_name:

```py
Entity.objects.all().order_by(field_name)[:10]
```

Da biste dobili redove od 10 do 20, možete koristiti sledeće isecanje:

```py
Entity.objects.all().order_by(field_name)[10:20]
```

Generalno, sintaksa za ograničavanje rezultata je sledeća:

```py
Entity.objects.all()[Offset:Offset+Limit]
```

U ovoj sintaksi,

- `Offset` je broj redova koje želite da preskočite,
- `Limit` je broj redova koje želite da preuzmete.

Imajte na umu da Django ne podržava negativno indeksiranje kao što je:

```py
Entity.objects.all().order_by(field_name)[-10]
```

Takođe, kada isecate `QuerySet`, Django vraća novi `QuerySet`.

Da biste preuzeli prvi ili poslednji red, trebalo bi da koristite metod `first()` `last()` jer ako je `QuerySet` prazan i koristite isec:

```py
Entity.objects.all().order_by(field_name)[0]
```

...onda ćete dobiti IndexErrorizuzetak.

### Primer Limit&Offset ograničenja

Koristićemo `Employee` model iz `HR` aplikacije za demonstraciju. `Employee` model se mapira u `hr_employee` tabelu:

Sledeći primer prikazuje prvih 10 zaposlenih poređanih po imenu:

```shell
>>> Employee.objects.all().order_by('first_name')[:10] 
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 ORDER BY "hr_employee"."first_name" ASC
 LIMIT 10
```

```sql
<QuerySet [<Employee: Aaron Pearson>, <Employee: Adam Crane>, <Employee: Adam Stewart>, <Employee: Adrienne Green>, <Employee: Alan Johnson>, <Employee: Alexa West>, <Employee: Alicia Wyatt>, <Employee: Amanda Benson>, <Employee: Amber Brown>, <Employee: Amy Lopez>]>
```

Sledeći primer preskače prvih 10 redova i dobija sledećih 10 redova iz `hr_employee` tabele:

```shell
>>> Employee.objects.order_by('first_name')[10:20] 
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 ORDER BY "hr_employee"."first_name" ASC
 LIMIT 10 
 OFFSET 10
```

```sql
<QuerySet [<Employee: Amy Lee>, <Employee: Andre Perez>, <Employee: Andrea Mcintosh>, <Employee: Andrew Dixon>, <Employee: Andrew Guerra>, <Employee: Ann Chang>, 
<Employee: Anne Odom>, <Employee: Anthony Fuentes>, <Employee: Anthony Welch>, <Employee: Ashley Brown>]>
```

### Rezime Limit & Offset ograničenje

- Django koristi sintaksu isecanja nizova da bi ograničio broj objekata koje vraća `QuerySet`.

[Sadržaj](#sadržaj)

## Operatori startswith, endswith i contains

Za demonstraciju ćemo koristiti model `Employee` iz `HR` aplikacije. `Employee` model se mapira na `hr_employee` tabelu u bazi podataka:

### startswith i istartswith

Ponekad želite da proverite da li string počinje podstringom. Na primer, možda želite da pronađete zaposlene čije ime počinje sa Je.

Da biste to uradili u SQL-u, koristite LIKE operator ovako:

```sql
SELECT *
FROM hr_employee
WHERE first_name LIKE 'Je%';
```

Ovde je `%` džoker znak koji odgovara bilo kom broju znakova. 'Je%' odgovara stringovima koji počinju sa `Je` i praćeni su sa nula ili više znakova.

Da biste dobili podatke iz Django ORM-a koristeći kao LIKE operator, koristite `startswith` tako što ga dodajete nazivu polja:

```py
field_name__startswith
```

Na primer, sledeći primer koristi `filter()` metodu za pronalaženje zaposlenih čija imena počinju sa Je:

```shell
>>> Employee.objects.filter(first_name__startswith='Je') 
```

```sql
SELECT "hr_employee"."id",        
       "hr_employee"."first_name",
       "hr_employee"."last_name", 
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE "hr_employee"."first_name"::text LIKE 'Je%'
 LIMIT 21
```

```shell
<QuerySet [<Employee: Jennifer Thompson>, <Employee: Jerry Cunningham>, <Employee: Jesus Reilly>, <Employee: Jessica Lewis>, <Employee: Jeanette Hendrix>, <Employee: Jeffrey Castro>, <Employee: Jessica Jackson>, <Employee: Jennifer Bender>, <Employee: Jennifer Moyer>]>
```

Ako želite da pronađete zaposlene čija imena počinju sa "Je" bez razlikovanja velikih i malih slova, možete koristiti `istartswith`:

```shell
>>> Employee.objects.filter(first_name__istartswith='je') 
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE UPPER("hr_employee"."first_name"::text) LIKE UPPER('je%')
 LIMIT 21
```

```shell
<QuerySet [<Employee: Jennifer Thompson>, <Employee: Jerry Cunningham>, <Employee: Jesus Reilly>, <Employee: Jessica Lewis>, <Employee: Jeanette Hendrix>, <Employee: Jeffrey Castro>, <Employee: Jessica Jackson>, <Employee: Jennifer Bender>, <Employee: Jennifer Moyer>]>
```

U ovom slučaju, `__istartswith` koristi verziju vrednosti napisanu velikim slovima za podudaranje.

### endswith i iendswith

Operatori `endswith` i `iendswith` vraćaju `True` ako se vrednost završava podstringomm. `endswith` je ekvivalentan sledećem LIKE operatoru:

```sql
LIKE '%substring'
```

Na primer, sledeći kod koristi `endswith` da pronađe zaposlene čija se imena završavaju sa "er":

```shell
>>> Employee.objects.filter(first_name__endswith='er')
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE "hr_employee"."first_name"::text LIKE '%er'
 LIMIT 21
```

```shell
<QuerySet [<Employee: Jennifer Thompson>, <Employee: Tyler Briggs>, <Employee: Spencer Riggs>, <Employee: Roger Robinson>, <Employee: Hunter Boyd>, <Employee: Amber Brown>, <Employee: Tyler Coleman>, <Employee: Jennifer Bender>, <Employee: Jennifer Moyer>]>
```

Upit je vratio zaposlene sa imenima "Jennifer", "Tyler", "Spencer", "Roger", itd.

`iendswith` je verzija od koja ne razlikuje velika i mala slova kao `endswith`. Na primer:

```shell
>>> Employee.objects.filter(first_name__iendswith='ER') 
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE UPPER("hr_employee"."first_name"::text) LIKE UPPER('%ER')
 LIMIT 21
```

```shell
<QuerySet [<Employee: Jennifer Thompson>, <Employee: Tyler Briggs>, <Employee: Spencer Riggs>, <Employee: Roger Robinson>, <Employee: Hunter Boyd>, <Employee: Amber Brown>, <Employee: Tyler Coleman>, <Employee: Jennifer Bender>, <Employee: Jennifer Moyer>]>
```

### contains i icontains

`contains` vam omogućava da proverite da li string sadrži podstring. Ekvivalentno je sledećem LIKE operatoru:

```sql
LIKE '%substring%'
```

Na primer, sledeći kod pronalazi zaposlene čije ime sadrži podstring ff:

```shell
>>> Employee.objects.filter(first_name__contains='ff')
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE "hr_employee"."first_name"::text LIKE '%ff%'
 LIMIT 21
```

```shell
<QuerySet [<Employee: Tiffany Jackson>, <Employee: Tiffany Holt>, <Employee: Jeffrey Castro>]>
```

Upit vraća zaposlene sa imenima Tiffani i DŽeffri.

`icontains` je verzija `contains` koja ne razlikuje velika i mala slova. Dakle, možete koristiti `icontains` da proverite da li string sadrži podstring bez razlikovanja velikih i malih slova:

```shell
>>> Employee.objects.filter(first_name__icontains='ff') 
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE UPPER("hr_employee"."first_name"::text) LIKE UPPER('%ff%')
 LIMIT 21
```

```sql
<QuerySet [<Employee: Tiffany Jackson>, <Employee: Tiffany Holt>, <Employee: Jeffrey Castro>]>
```

### Rezime LIKE operatori

Django ORM vs SQL LIKE operatori:

|Django ORM                           |SQL Like                                    |
|-------------------------------------|--------------------------------------------|
|field_name__startswith = 'substring' | field_name LIKE '%substring'               |
|field_name__istartswith = 'substring'| UPPER(field_name) LIKE UPPER('%substring') |
|field_name__endswith = 'substring'   | field_name LIKE 'substring%'               |
|field_name__iendswith = 'substring'  | UPPER(field_name) LIKE UPPER('substring%') |
|field_name__contains = 'substring'   | field_name LIKE '%substring%'              |
|field_name__icontains = 'substring'  | UPPER(field_name) LIKE UPPER('%substring%')|

[Sadržaj](#sadržaj)

## Operatori IN i NOT IN

### Operator IN

Koristićemo `Employee` model u `HR` aplikaciji za demonstraciju. `Employee` model se mapira na `hr_employee` tabelu u bazi podataka:

`SQL` `IN` operator vraća vrednost "True" ako se vrednost nalazi u skupu vrednosti:

```sql
field_name IN (v1, v2, ...)
```

Na primer, možete koristiti `IN` operator da biste upitali za redove iz `hr_employee` tabele čiji `department_id` je na listi, ovako:

```sql
SELECT *
FROM hr_employee
WHERE department_id IN (1,2,3)
```

U Django-u koristite `in` operator ovako:

```shell
>>> Employee.objects.filter(department_id__in=(1,2,3)) 
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE "hr_employee"."department_id" IN (1, 2, 3)
```

Obično se koristi podupit sa `IN` operatorom, a ne lista doslovnih vrednosti. Na primer, sve zaposlene u odeljenjima `Sales` i `Marketing` pronalazite na sledeći način:

```shell
>>> departments = Department.objects.filter(Q(name='Sales') | Q(name='Marketing')) 
```

```shell
>>> Employee.objects.filter(department__in=departments)
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE "hr_employee"."department_id" IN (
        SELECT U0."id"
          FROM "hr_department" U0
         WHERE (U0."name" = 'Sales' OR U0."name" = 'Marketing')
       )
```

Kako to funkcioniše?

Nabavite odeljenja sa nazivima Sales ili Marketing:
Ovde koristimo <a name="Q-objects"></a> [Q() objekte](F_izrazi_i_Q_objekti.md) Qjango ORM-a.

```py
departments = Department.objects.filter(Q(name='Sales') | Q(name='Marketing'))
```

Prosledite department `QuerySet` operatoru `IN`:

```py
Employee.objects.filter(department__in=departments)
```

Iza kulisa, Django izvršava upit sa `IN` operatorom koji upoređuje ID odeljenja sa listom ID-ova odeljenja sa liste:

```sql
SELECT "hr_employee"."id",
      "hr_employee"."first_name",
      "hr_employee"."last_name",
      "hr_employee"."contact_id",
      "hr_employee"."department_id"
  FROM "hr_employee"
WHERE "hr_employee"."department_id" IN (
        SELECT U0."id"
          FROM "hr_department" U0
        WHERE (U0."name" = 'Sales' OR U0."name" = 'Marketing')
      )
```

### Operator NOT IN

Operator `NOT` negira `IN` operator. `NOT INO` perator vraća vrednost `True` ako vrednost nije u listi vrednosti:

```sql
field_name NOT IN (v1, v2, ...)
```

Da biste izvršili `NOT IN` u Django ORM-u možete koristiti `Q` objekat i operator `~`:

```py
~Q(field_name__in=(v1,v2,..))
```

Na primer, sledeći kod pronalazi zaposlene čiji ID odeljenja nije 1, 2 ili 3:

```shell
>>> Employee.objects.filter(~Q(department_id__in=(1,2,3))) 
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE NOT ("hr_employee"."department_id" IN (1, 2, 3))
```

Alternativno, možete koristiti `exclude()` metod umesto `filter()` metode:

```shell
>>> Employee.objects.exclude(department_id__in=(1,2,3))
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE NOT ("hr_employee"."department_id" IN (1, 2, 3))
```

### Rezime IN i NOT IN operatora

- Koristite Django `in` da proverite da li se vrednost nalazi u listi vrednosti.

| Django ORM                                    | SQL                   |
|-----------------------------------------------|-----------------------|
| Entity.objects.filter(id__in = (v1,v2,v3))    | id IN (v1,v2,v3)      |
| Entity.objects.filter(~Q(id__in = (v1,v2,v3)))| NOT (id IN (v1,v2,v3))|
| Entity.objects.exclude(id__in = (v1,v2,v3))   | NOT (id IN (v1,v2,v3))|

[Sadržaj](#sadržaj)

## Operator RANGE i NOT RANGE

### Operator RANGE

U SQL-u, koristite BETWEEN operator da biste proverili da li se vrednost nalazi između dve vrednosti:

```sql
field_name BETWEEN low_value AND high_value
```

Operator BETWEEN vraća vrednost `True` ako je vrednost  `field_name` između `low_value` i `high_value` . U suprotnom, vraća vrednost `False`.

#### Korišćenje RANGE sa brojevima

Djangov ekvivalent operatora `BETWEEN` je `__range`:

```py
Entity.objects.filter(field_name__range=(low_value,high_value))
```

Na primer, možete pronaći zaposlene čiji je ID između 1 i 5 koristeći opseg kao što je ovaj:

```shell
>>> Employee.objects.filter(id__range=(1,5))
```

Iza kulisa, Django izvršava sledeći upit:

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE "hr_employee"."id" BETWEEN 1 AND 5
```

#### Korišćenje Django opsega sa datumima

Pored brojeva i stringova, opseg funkcioniše i sa datumima. Na primer, sledeći kod vraća sve dodele poslova počev od "January 1, 2020", do "March 31, 2020":

```shell
>>> Assignment.objects.filter(begin_date__range=(start_date,end_date))
```

```sql
SELECT "hr_assignment"."id",
       "hr_assignment"."employee_id",
       "hr_assignment"."job_id",
       "hr_assignment"."begin_date",
       "hr_assignment"."end_date"
  FROM "hr_assignment"
 WHERE "hr_assignment"."begin_date" BETWEEN '2020-01-01'::date AND '2020-03-31'::date
```

### Operator NOT RANGE

U SQL operator NOT negira operator BETWEEN:

```sql
field_name NOT BETWEEN (low_value, high_value)
```

Drugim rečima, NOT BETWEEN vraća vrednost `True` ako vrednost nije u opsegu vrednosti.

U Django-u, možete koristiti `Q` objekat sa opsegom da biste proverili da li vrednost nije u opsegu:

```py
Entity.objects.filter(~Q(field_name__range=(low_value,high_value)))
```

Na primer, možete pronaći zaposlene čiji ID nije u opsegu (1,5):

```shell
>>> Employee.objects.filter(~Q(id__range=(1,5)))
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE NOT ("hr_employee"."id" BETWEEN 1 AND 5)
```

### Rezime operatora range i not range

- Koristite Django `range` da proverite da li je vrednost u opsegu vrednosti.

[Sadržaj](#sadržaj)

## Metod exists()

Ponekad želite da proverite da li upit sadrži bilo kakve redove. Da biste to uradili, koristite `exists()` metod objekta `QuerySet`.

Metoda `exists()` vraća vrednost `True` ako `QuerySet` sadrži bilo kakve redove ili `False` ako je prazan.

Iza kulisa, `exists()` će pokušat da izvrši upit na najbrži mogući način.Međutim, upit će biti gotovo identičan običnom `QuerySet` upitu.

Pretpostavimo da imate `QuerySet` i želite da proverite da li ima neke objekte. Umesto da uradite ovo:

```py
if query_set:
   print('the queryset has at least one object')
```

...trebalo bi da koristite `exists()` jer je malo brži:

```py
if query_set.exists():
   print('the queryset has at least one object')
```

`Django 4.1` je dodao `aexists()`, što je asinhrona verzija `exists()`.

### Primer metode exists()

Koristićemo `Employee` model za demonstraciju. `Employee` model se mapira na `hr_employee` tabelu u bazi podataka:

Pokrenimo `shell_plus` komandu:

```py
python manage.py shell_plus
```

Pronađimo zaposlene čija imena počinju slovom "J":

```shell
>>> Employee.objects.filter(first_name__startswith='J').exists()
```

```sql
SELECT 1 AS "a"
  FROM "hr_employee"
 WHERE "hr_employee"."first_name"::text LIKE 'J%'
 LIMIT 1
```

```shell
True
```

> [!Note]
>
> Imajte na umu da je Django generisao SQL na osnovu PostgreSQL-a. Ako koristite druge baze podataka, možete videti malo drugačiji SQL izraz.

U ovom primeru, `exists()` metod vraća vrednost `True`. On bira samo prvi red da bi utvrdio da li `QuerySet` sadrži bilo koji red.

Ako ne koristite `exists()` metod, u `QuerySet` biće svi redovi iz `hr_employee` tabele koji zadovoljavaju dati uslov:

```shell
>>> qs = Employee.objects.filter(first_name__startswith='J') 
>>> print(qs.query)
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
FROM "hr_employee"
WHERE "hr_employee"."first_name"::text LIKE J%
ORDER BY "hr_employee"."first_name" ASC,
         "hr_employee"."last_name" ASC
```

### Rezime operatora exists

- Koristite Django `exists()` metodu da proverite da li `QuerySet` sadrži bilo kakve redove.

[Sadržaj](#sadržaj)

## Metod isnull()

Za demonstraciju ćemo koristiti modele `Employee` i `Contact` iz `HR` aplikacije. Modeli `Emloyee` i `Contact` se mapiraju na tabele `hr_employee` i `hr_contact` baze podataka.

U relacionim bazama podataka, `NULL` označava nedostajuće informacije. Na primer, ako ne znate kontakt zaposlenog, možete koristiti `NULL` za kontakt zaposlenog u trenutku snimanja podataka.

`NULL` je posebna vrednost jer ne možete koristiti operator = da biste uporedili vrednost sa njom. Umesto toga, koristite `IS NULL` operator.

Na primer, da biste proverili da li je vrednost u contact_id `NULL` ili ne, koristite `IS NULL` operator na sledeći način:

```sql
contact_id IS NULL
```

`IS NULL` operator vraća vrednost `True`“ ako je vrednost `contact_id` nevažeća, tj `NULL` ili `False` u suprotnom.

Da biste negirali `IS NULL` operator, možete koristiti `NOT` operator ovako:

```sql
contact_id IS NOT NULL
```

Django koristi `__isnull` da proveri da li je vrednost `NULL` ili ne:

```py
Entity.objects.filter(field_name__isnull=True)
```

Metoda `filter()` vraća sve instance sa `field_name` jednako `NULL`. Da biste negirali `is isnull`, uporedite ga sa `False`:

```py
Entity.objects.filter(field_name__isnull=False)
```

U ovom slučaju, `filter()` vraća sve instance entiteta čiji `field_name` nije `NULL`.

### isnull

Sledeći primer koristi `isnull` da bi se dobili zaposleni koji nemaju kontakte:

```shell
>>> Employee.objects.filter(contact_id__isnull=True)
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE "hr_employee"."contact_id" IS NULL
```

Generisani upit koristi `IS NULL` operator za upoređivanje `contact_id` sa `NULL`.

Sledeći primer koristi `isnull` da bi dobio sve zaposlene koji imaju kontakte:

```shell
>>> Employee.objects.filter(contact_id__isnull=False) 
```

```sql
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 WHERE "hr_employee"."contact_id" IS NOT NULL
```

U ovom slučaju, generisani upit koristi `IS NOT NULL` da bi uporedio vrednosti u `contact_id` koloni sa `NULL`.

### Rezime operator isnull

- Koristimo `isnull` da proverimo da li je vrednost `NULL` ili nije.

[Sadržaj](#sadržaj)

## Agregati

U ovom tutorijalu ćete naučiti kako da koristite Django da biste dobili agregatne vrednosti iz baze podataka, uključujući `count`, `min`, `max`, `sum` i `avg`.

### Priprema podataka

Za demonstraciju ćemo koristiti modele `Employee` i `Department` iz `HR` aplikacije. Modeli `Employee` i `Department` se mapiraju na tabele `hr_employee` i `hr_department`.

Podatke za primere možete dobiti [ovde](https://www.pythontutorial.net/wp-content/uploads/2023/01/django_orm_7_1.zip).

Dodajmo `salary` polje modelu `Employee`:

```py
class Employee(models.Model):

    salary = models.DecimalField(max_digits=15, decimal_places=2)
    # ...
```

Izvršimo migracije pomoću `makemigrations` komande:

```shell
python manage.py makemigrations

Migrations for 'hr':
  hr\migrations\0005_employee_salary.py
    - Add field salary to employee
```

Proširimo izmene u bazu podataka pokretanjem `migrate` komande:

```shell
python manage.py migrate

Operations to perform:
  Apply all migrations: admin, auth, contenttypes, hr, sessions
Running migrations:
  Applying hr.0005_employee_salary... OK
```

Konačno, popunimo vrednosti u `salary` kolonu podacima iz `data.json` jedinice:

```shell
python manage.py loaddata data.json
```

### Uvod u agregate

Agregatna funkcija prihvata listu vrednosti i vraća jednu vrednost. Uobičajeno korišćene agregatne funkcije su `count`, `max`, `min`, `avg` i `sum`.

#### Count

Objekat `QuerySet` vam pruža `count()` metodu koja vraća broj objekata koje sadrži. Na primer, možete koristiti `count()` metodu da biste dobili broj zaposlenih:

```shell
>>> Employee.objects.count()
```

```sql
SELECT COUNT(*) AS "__count"
  FROM "hr_employee"        
```

```shell
220
```

Metoda `count()` koristi `SQL` `COUNT(*)` funkciju da vrati broj redova u `hr_employee` tabeli.

Da biste dobili broj zaposlenih čija imena počinju slovom "J", možete koristiti obe `filter()` metode `count()` objekta, `QuerySet` kao što je ovaj:

```shell
>>> Employee.objects.filter(first_name__startswith='J').count()
```

```sql
SELECT COUNT(*) AS "__count"
  FROM "hr_employee"
 WHERE "hr_employee"."first_name"::text LIKE 'J%'
```

```shell
29
```

U ovom slučaju, `filter()` metoda formira `WHERE` klauzulu dok `count()` metoda formira `COUNT()` funkciju.

#### Max

`Max()` vraća maksimalnu vrednost u skupu vrednosti. Prihvata kolonu u kojoj želite da dobijete najveću vrednost.

Na primer, sledeći izraz koristi `Max()` da bi vratio najvišu platu:

```shell
>>> Employee.objects.aggregate(Max('salary'))
```

```sql
SELECT MAX("hr_employee"."salary") AS "salary__max"
  FROM "hr_employee"
```

```shell
{'salary__max': Decimal('248312.00')}
```

`Max()` izvršava SQL MAX() na koloni `salary` u `hr_employee` tabeli i vraća najvišu platu.

#### Min

`Min()` vraća  minimalnu vrednost u skupu vrednosti. Kao i `Max()`, prihvata kolonu u kojoj želite da dobijete najnižu vrednost.

Sledeći primer koristi `Min()` da bi vratio najnižu platu zaposlenih:

```shell
>>> Employee.objects.aggregate(Min('salary')) 
```

```sql
SELECT MIN("hr_employee"."salary") AS "salary__min"
  FROM "hr_employee"
```

```shell
{'salary__min': Decimal('40543.00')}
```

Funkcija `Min()` izvršava `SQL` `MIN()` funkciju koja vraća minimalnu vrednost u koloni plata.

#### AVG

`Avg()` vraća  prosečnu vrednost u skupu vrednosti. Prihvata naziv kolone i vraća prosečnu vrednost svih vrednosti u toj koloni:

```shell
>>> Employee.objects.aggregate(Avg('salary'))
```

```sql
SELECT AVG("hr_employee"."salary") AS "salary__avg"
  FROM "hr_employee"
```

```shell
{'salary__avg': Decimal('137100.490909090909')}
```

Iza kulisa, `Avg()` izvršava `SQL` `AVG()` funkciju na koloni sa platama `hr_employee` i vraća prosečnu platu.

#### Sum

`Sum()` vraća zbir vrednosti. Na primer, možete koristiti `Sum()` da biste izračunali ukupnu platu kompanije:

```shell
>>> Employee.objects.aggregate(Sum('salary')) 
```

```sql
SELECT SUM("hr_employee"."salary") AS "salary__sum"
  FROM "hr_employee"
```

```shell
{'salary__sum': Decimal('30162108.00')}
```

`Sum()` izvršava `SQL` `SUM()` funkciju i vraća ukupnu vrednost svih vrednosti u koloni plata u `hr_employee` tabeli.

### Rezime agregati

- Koristite `count()` metodu da biste dobili broj objekata u `QuerySet`.
- Koristite `Max()` da biste dobili maksimalnu vrednost u skupu vrednosti.
- Koristite `Min()` da biste dobili minimalnu vrednost u skupu vrednosti.
- Koristite `Avg()` da biste dobili prosečnu vrednost u skupu vrednosti.
- Koristite `Sum()` da biste dobili ukupnu vrednost seta.

[Sadržaj](#sadržaj)

## Grupisanje i agregacija podataka

`SQL` `GROUP BY` klauzula grupiše redove koje vraća upit. Obično se koriste agregatne funkcije kao što su `count`, `min`, `max`, `avg` i `sum` sa `GROUP BY` klauzulom da bi se vratila agregirana vrednost za svaku grupu.

Evo osnovne upotrebe klauzule `GROUP BY` u `SELECT` izjavi:

```sql
SELECT column_1, AGGREGATE(column_2)
FROM table_name
GROUP BY column1;
```

U Djangu, možete koristiti `annotate()` metodu sa `values()` da biste primenili agregaciju na grupe poput ove:

```py
(Entity.objects
  .values('column_2')
  .annotate(value=AGGREGATE('column_1'))
)
```

U ovoj sintaksi;

- `values('column_2')` – prosledite metodi `values()` kolonu koju želite da grupišete.
- `annotate(value=AGGREGATE('column_1'))` – navedite šta treba agregirati u `annotate()` metodi.

Obratite pažnju da je redosled pozivanja `values()` i `annotates()` važan. Ako ne pozovete `values()` metodu prvo i `annotate()` zatim, izraz neće proizvesti agregatne rezultate.

### GROUP BY primeri

Za demonstraciju ćemo koristiti modele `Employee` i `Department` iz aplikacije `HR`. Modeli `Emloyee` i `Department` se mapiraju na tabele `hr_employee` i `hr_department` u bazi podataka:

#### Primer GROUP BY sa Count

Sledeći primer koristi metodu `values()` `annotate()` da bi se dobio broj zaposlenih po odeljenju:

```shell
>>> (Employee.objects
...     .values('department')
...     .annotate(head_count=Count('department'))
...     .order_by('department')
...  )
```

```sql
SELECT "hr_employee"."department_id",
       COUNT("hr_employee"."department_id") AS "head_count"
  FROM "hr_employee"
 GROUP BY "hr_employee"."department_id"
 ORDER BY "hr_employee"."department_id" ASC
 LIMIT 21
```

```shell
<QuerySet [
  {'department': 1, 'head_count': 30}, 
  {'department': 2, 'head_count': 40}, 
  {'department': 3, 'head_count': 28}, 
  {'department': 4, 'head_count': 29}, 
  {'department': 5, 'head_count': 29}, 
  {'department': 6, 'head_count': 30}, 
  {'department': 7, 'head_count': 34}
]>
```

Kako to funkcioniše?

Grupišemo zaposlene po odeljenjima koristeći `values()` metodu:

```py
values('department')
```

Primenimo `Count()` na svaku grupu:

```py
annotate(head_count=Count('department'))
```

Sortiramo objekte `QuerySet`-a po `department` koloni:

```py
order_by('department')
```

Iza kulisa, Django izvršava `SELECT` sa `GROUP BY` klauzulom:

```sql
SELECT "hr_employee"."department_id",
       COUNT("hr_employee"."department_id") AS "head_count"
  FROM "hr_employee"
 GROUP BY "hr_employee"."department_id"
 ORDER BY "hr_employee"."department_id" ASC
 LIMIT 21
```

#### Primer GROUP BY sa Sum

Slično tome, možete koristiti `Sum()` agregat za izračunavanje ukupne plate zaposlenih u svakom odeljenju:

```py
>>> (Employee.objects
...     .values('department')
...     .annotate(total_salary=Sum('salary'))
...     .order_by('department')
...  )
```

```sql
SELECT "hr_employee"."department_id",
       SUM("hr_employee"."salary") AS "total_salary"
  FROM "hr_employee"
 GROUP BY "hr_employee"."department_id"
 ORDER BY "hr_employee"."department_id" ASC
 LIMIT 21
```

```shell
<QuerySet [
  {'department': 1, 'total_salary': Decimal('3615341.00')}, 
  {'department': 2, 'total_salary': Decimal('5141611.00')}, 
  {'department': 3, 'total_salary': Decimal('3728988.00')}, 
  {'department': 4, 'total_salary': Decimal('3955669.00')}, 
  {'department': 5, 'total_salary': Decimal('4385784.00')}, 
  {'department': 6, 'total_salary': Decimal('4735927.00')}, 
  {'department': 7, 'total_salary': Decimal('4598788.00')}
]>
```

#### Primer GROUP BY sa min, max i avg

Sledeći primer primenjuje više agregatnih funkcija na grupe da bi se dobila najniža, prosečna i najviša plata zaposlenih u svakom odeljenju:

```py
>>> (Employee.objects
...     .values('department')
...     .annotate(
...         min_salary=Min('salary'),
...         max_salary=Max('salary'),
...         avg_salary=Avg('salary')
...     )
...     .order_by('department')
...  )
```

```sql
SELECT "hr_employee"."department_id",
       MIN("hr_employee"."salary") AS "min_salary",
       MAX("hr_employee"."salary") AS "max_salary",
       AVG("hr_employee"."salary") AS "avg_salary"
  FROM "hr_employee"
 GROUP BY "hr_employee"."department_id"
 ORDER BY "hr_employee"."department_id" ASC
 LIMIT 21
```

```shell
<QuerySet [
  {'department': 1, 'min_salary': Decimal('45427.00'), 'max_salary': Decimal('149830.00'), 'avg_salary': Decimal('120511.366666666667')}, 
  {'department': 2, 'min_salary': Decimal('46637.00'), 'max_salary': Decimal('243462.00'), 'avg_salary': Decimal('128540.275000000000')}, 
  {'department': 3, 'min_salary': Decimal('40762.00'), 'max_salary': Decimal('248265.00'), 'avg_salary': Decimal('133178.142857142857')}, 
  {'department': 4, 'min_salary': Decimal('43000.00'), 'max_salary': Decimal('238016.00'), 'avg_salary': Decimal('136402.379310344828')}, 
  {'department': 5, 'min_salary': Decimal('42080.00'), 'max_salary': Decimal('246403.00'), 'avg_salary': Decimal('151233.931034482759')}, 
  {'department': 6, 'min_salary': Decimal('58356.00'), 'max_salary': Decimal('248312.00'), 'avg_salary': Decimal('157864.233333333333')}, 
  {'department': 7, 'min_salary': Decimal('40543.00'), 'max_salary': Decimal('238892.00'), 'avg_salary': Decimal('135258.470588235294')}]>
```

#### Primer GROUP BY pomoću JOIN-a

Sledeći primer koristi metode `values()` i `annotate()` da bi se dobio broj zaposlenih po odeljenju:

```py
>>> (Department.objects
...     .values('name')
...     .annotate(
...         head_count=Count('employee')
...     )
...  )
```

```sql
SELECT "hr_department"."name",
       COUNT("hr_employee"."id") AS "head_count"
  FROM "hr_department"
  LEFT OUTER JOIN "hr_employee"
    ON ("hr_department"."id" = "hr_employee"."department_id")
 GROUP BY "hr_department"."name"
 LIMIT 21
```

```shell
<QuerySet [
  {'name': 'Marketing', 'head_count': 28}, 
  {'name': 'Finance', 'head_count': 29}, 
  {'name': 'SCM', 'head_count': 29}, 
  {'name': 'GA', 'head_count': 30}, 
  {'name': 'Sales', 'head_count': 40}, 
  {'name': 'IT', 'head_count': 30}, 
  {'name': 'HR', 'head_count': 34}
]>
```

Kako to funkcioniše?

- `values('name')` – grupiše odeljenje po `name`.
- `annotate(headcount=Count('employee'))` – broji zaposlene u svakom odeljenju.

Iza kulisa, Django koristi `LEFT JOIN` da spoji `hr_department` tabelu sa `hr_employee` tablom i primeni `COUNT()` funkciju na svaku grupu.

#### GROUP BY sasa HAVING

Da biste primenili uslov na grupe, koristite `filter()` metodu. Na primer, sledeći primer koristi `filter()` metodu da bi dobio odeljenje sa brojem zaposlenih većim od 30:

```shell
>>> (Department.objects
...     .values('name')
...     .annotate(
...         head_count=Count('employee')
...     )
...     .filter(head_count__gt=30)
...  )
```

```sql
SELECT "hr_department"."name",
       COUNT("hr_employee"."id") AS "head_count"
  FROM "hr_department"
  LEFT OUTER JOIN "hr_employee"
    ON ("hr_department"."id" = "hr_employee"."department_id")
 GROUP BY "hr_department"."name"
HAVING COUNT("hr_employee"."id") > 30
 LIMIT 21
```

```shell
<QuerySet [{'name': 'Sales', 'head_count': 40}, {'name': 'HR', 'head_count': 34}]>
```

Iza kulisa, Django koristi `HAVING` klauzulu za filtriranje grupe na osnovu uslova koji prosleđujemo `filter()` metodi:

```sql
SELECT "hr_department"."name",
       COUNT("hr_employee"."id") AS "head_count"
  FROM "hr_department"
  LEFT OUTER JOIN "hr_employee"
    ON ("hr_department"."id" = "hr_employee"."department_id")
 GROUP BY "hr_department"."name"
HAVING COUNT("hr_employee"."id") > 30
```

### Rezime group by

- Koristite metodu `values()` i `annotate()` za grupisanje redova u grupe.
- Koristite `filter()` za dodavanje filterskog uslova grupama.

[Sadržaj](#sadržaj)

## Dumpdata

Ponekad želite da premestite neke uobičajene podatke iz test baze podataka u produkcijsku bazu podataka.

Da biste to uradili, koristite `dumpdata` komandu za izvoz podataka iz test baze podataka u datoteku i uvoz u proizvodnu bazu podataka pomoću `loaddata` komande.

Komanda `dumpdata` ima mnogo opcija koje vam omogućavaju da:

- Izvezete sve instance modela svih aplikacija (celove baze podataka) u datoteku.
- Izvezete sve instance modela jedne aplikacije u datoteku.
- Izvezete neke instance modela (neke tabele u bazi podataka) u datoteku.

Format izlazne datoteke može biti `xml`, `json`, `jsonl`, ili `yaml`. Sledeći kod vrši damp cele baze podataka u `data.json` datoteku:

```shell
python manage.py dumpdata > data.json
```

Ako otvorite `data.json` datoteku, videćete mnoštvo podataka. Na primer, sledeći primer prikazuje instancu Employee:

```json
...
{
    "model": "hr.employee",
    "pk": 5,
    "fields": {
      "first_name": "John",
      "last_name": "Doe",
      "contact": null,
      "department": 1,
      "compensations": [1, 2]
    }
  },
...
```

Uzorak modela sadrži sledeće informacije:

- Naziv modela ( `hr.employee` ).
- Vrednost primarnog ključa ( `pk` ).
- Polja ( `first_name`, `last_name`, `contact`, `department`, i `compensations`) modela `Employee`.

Ako želite da izvezete celu bazu podataka u drugi format kao što je `XML`, potrebno je da navedete opciju formata:

```shell
python manage.py dumpdata > filename --format file_format
```

`file_format` može biti `json`, `jsonl`, `xml`, i `yaml`.

Na primer, sledeća komanda eksportuje celu bazu podataka u `XML` datoteku:

```shell
python manage.py dumpdata > data.xml --format xml
```

### Izvoz podataka iz određene aplikacije

Da biste izvezli podatke određene aplikacije, navedite naziv aplikacije:

```shell
python manage.py dumpdata app_name > filename.json
```

Na primer, sledeća komanda izvozi instance modela aplikacije `hr`:

```shell
python manage.py dumpdata hr > hr.json
```

### Izvoz podataka iz određenog modela

Da biste ispisali podatke određene tabele, navedite naziv aplikacije i naziv modela na sledeći način:

```shell
python manage.py app_name.model_name > filename
```

Na primer, sledeća komanda ispisuje sve instance tabele Employee u HRaplikaciji:

```shell
python manage.py dumpdata hr.employee > hr_employee.json
```

### Izvoz podataka isključivanjem jednog ili više

Ponekad želite da izvezete podatke iz svih modela osim jednog ili više modela. U tom slučaju možete koristiti `--exclude` opciju:

```shell
python manage.py --exclude app_name.model_name > filename
```

Imajte na umu da komanda može da sadrži više `--exclude` opcija tako da možete da isključite više modela.

Na primer, sledeći izvoz podataka iz cele baze podataka osim modela `Contact`:

```shell
python manage.py --exclude hr.contact > data.json
```

Sledeća komanda izvozi podatke iz cele baze podataka osim modela `Contact` i `Department`:

```shell
python manage.py --exclude hr.contact --exclude department > data.json
```

### Rezime dumpdata

- Koristite `dumpdata` komandu za izvoz podataka jednog ili više modela.

[Sadržaj](#sadržaj)

## Loaddata

Komanda `loaddata` vam omogućava da učitate podatke iz datoteke u bazu podataka. Obično se `dumpdata` komanda koristi za izvoz podataka iz baze podataka a komanda `loaddata` za uvoz podataka iz datoteke u istu ili drugu bazu podataka.

```shell
python manage.py loaddata fixture_name
```

Fiksčer ( `Fixture` ) je kolekcija datoteka sa podacima koje će se koristiti za uvoz u bazu podataka.

Po konvenciji, Django će tražiti uređaje u `fixtures` direktorijumu ispod svake aplikacije i uvoziti podatke iz njih.

Na primer, sledeće prikazuje `fixtures` direktorijum i `hr.json` fiksčer datoteku u `hr` aplikaciji projekta:

```shell
├── admin.py
├── apps.py
├── fixtures
|  └── hr.json
...
```

Sledeći primer prikazuje odlomak iz `hr.json` datoteke:

```json
[
  {
    "model": "hr.contact",
    "pk": 1,
    "fields": {
      "phone": "40812345678",
      "address": "101 N 1st Street, San Jose, CA"
    }
  },
  {
    "model": "hr.contact",
    "pk": 2,
    "fields": {
      "phone": "4081111111",
      "address": "202 N 1st Street, San Jose, CA"
    }
  },
...
```

Da biste učitali podatke `hr.json` u bazu podataka, koristite sledeću `loaddata` komandu:

```shell
python manage.py loaddata hr.json
```

### Podešavanja direktorijuma uređaja

Podrazumevano, Django pronalazi datoteke sa podacima u `fixtures` direktorijumu unutar svake aplikacije. Da biste naveli dodatne direktorijume koji sadrže fiksčer datoteke, možete ih postaviti na `FIXTURE_DIR` Slistu u `settings.py` datoteci projekta:

```py
FIXTURE_DIRS = ['path/to/fixtures/dir', 'path/to/fixtures/dir2']
```

### Učitavanje primera HR podataka pomoću loaddata

Koristićemo `loaddata` komandu za učitavanje podataka iz fikstura za `HR aplikaciju Django projekta.

- Neka direktorijum aplikacije `hr/fixtures` sadrži `data.json` datoteku koja sadrži primere `HR` podataka.

- Pokrenite `loaddata` komandu za učitavanje podataka iz `data.json` datoteke:

```shell
python manage.py loaddata data.json
```

Trebalo bi da izlaz bude nešto ovako:

```shell
Installed 471 object(s) from 1 fixture(s)
```

### Rezime loaddata

- Koristite `loaddata` komandu da učitate podatke iz uređaja u bazu podataka.

[Sadržaj](#sadržaj)

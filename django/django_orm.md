
# Django ORM

U ovom odeljku ćemo se detaljno pozabaviti Django ORM-om i kako ga efikasno koristiti za interakciju sa relacionim bazama podataka.

## Sadržaj

- [Uvod u Django ORM](#uvod-u-django-orm)

  Podešavanje osnovnog projekta za sledeće tutorijale u ovom odeljku.</br></br>

- [Relacija "jedan-na-jedan"](#relacija-jedan-na-jedan)

  Kreiranje relacije "jedan-na-jedan".</br></br>

- [Relacija "jedan-na-više"](#relacija-jedan-na-više)

  Korišćenje ForeignKey za kreiranje relacije jedan-na-više.</br></br>

- [Relacija "više-na-više"](#relacija-više-na-više)

  Kreiranje relacije "više-na-više".</br></br>

- [Dodavanje dodatnih polja u relaciju "više na više"]().
  
  Kako dodati dodatna polja u relaciju „više na više“. </br></br>

- [Limit/Offset]()
  
  Kako koristiti isecanje za ograničenje broja objekata koje vraća QuerySet. </br></br>

- [order_by]()
  
  Kako koristiti metod order_by() za sortiranje rezultata koje vraća QuerySet. ( ORDER BY ) </br></br>

- [Startswith, endswith i contains]()
  
  Odabir podataka na osnovu podudaranja obrazaca ( LIKE ). </br></br>

- [In]()

  Provera da li se vrednost nalazi na listi vrednosti ( IN ). </br></br>

- [Range]()

  Korišćenje Django opsega za proveru da li je vrednost unutar opsega inkluzivno ( BETWEEN ). </br></br>

- [Isnull]() 

  Provera da li je vrednost NULL ( IS NULL ). </br></br>

- [Exist]()

  Vrati True ako upit sadrži bilo koji objekat ili False ako ne sadrži. </br></br>

- [Aggregate]()
  
  Kako izvršavati agregatne funkcije kao što su avg, count, max, min i sum. </br></br>

- [Group by]() 

  Grupiši objekte po kriterijumu u grupe. </br></br>

## Uvod u Django ORM

[00]: https://docs.djangoproject.com/en/4.2/ref/databases/

ORM je skraćenica od `objektno-relaciono mapiranje`. ORM je tehnika koja vam omogućava da manipulišete podacima u relacionoj bazi podataka koristeći objektno orijentisano programiranje.

Django ORM vam omogućava da koristite isti Python API za interakciju sa različitim relacionim bazama podataka, uključujući `PostgreSQL`, `MySQL`, `Oracle` i `SQLite`. Pogledajte kompletnu listu podržanih baza podataka [ovde][00].

Django ORM koristi `obrazac aktivnog zapisa`:

- Klasa se mapira na jednu tabelu u bazi podataka. Klasa se često naziva model klasa.
- Objekat klase se preslikava na jedan red u tabeli.

Kada definišete klasu modela, možete pristupiti unapred definisanim metodama za `kreiranje`, `čitanje`, `ažuriranje` i `brisanje` podataka.

Takođe, Django automatski generiše `administratorsku stranu` za upravljanje podacima modela.

Pogledajmo jednostavan primer da bismo videli kako Django ORM funkcioniše.

### Postavljanje osnovnog projekta

Postavićemo osnovni projekat sa novim virtuelnim okruženjem.

#### Kreiranje novog virtuelnog okruženja

- Kreirajte novo virtuelno okruženje koristeći ugrađeni `venv` modul:

  ```shell
  python -m venv venv
  ```

- Aktivirajte virtuelno okruženje:

  ```shell
  venv\scripts\activate
  ```

- Instalirajte `django` i `django-extensions` paket:

  ```shell
  pip install django django-extensions
  ```

  Paket `django-extensions` pruža neka prilagođena proširenja za Django frejmvork. Koristićemo `django-extensions` paket za prikazivanje generisanog SQL-a pomoću Django ORM-a.

#### Kreiranje novog projekta

- Kreirajte novi Django projekat pod nazivom `django_orm`:

  ```shell
  django-admin startproject django_orm
  ```

- Kreirajte `HR` aplikaciju unutar `django_orm` projekta:

  ```shell
  cd django_orm
  python manage.py startapp hr
  ```

- Registrujte `HR` i `django_extensions` u `INSTALLED_APPS` u `settings.py` projekta:

  ```py
  INSTALLED_APPS = [
      # ...
      'django_extensions',
      'hr',
  ]
  ```

#### Podešavanje PostgreSQL servera baze podataka

- Instalirajte `PostgreSQL` server baze podataka na vaš lokalni računar.

- Drugo, prijavite se na `PostgreSQL` server baze podataka. Biće vam zatražena lozinka korisnika `postgres`. Imajte na umu da koristite istu lozinku koju ste uneli za `postgres` korisnika tokom instalacije.

  ```shell
  psql -U postgres
  Password for user postgres:
  ```

- Kreirajte novu bazu podataka sa imenom `hr` i unesite `exit` da biste zatvorili `psql` program:

```shell
postgres=# create database hr;
CREATE DATABASE 
postgres=# exit
```

#### Povezivanje sa PostgreSQL-om

- Konfigurišite vezu sa bazom podataka u `settings.py` projekta `django_orm`:

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

- Instalirajte `psycopg2` paket da biste omogućili Django-u da se poveže sa `PostgreSQL` serverom baze podataka:

  ```shell
  pip install psycopg2
  ```

- Pokrenite Django razvojni server:

  ```shell
  python manage.py runserver
  ```

  Videćete podrazumevanu početnu stranicu Django-a.

### Definisanje modela

- Definišite `Employee` klasu u `hr` aplikaciji koja ima dva polja `first_name` i `last_name`:

```py
from django.db import models

class Employee(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)

    def __str__(self):
        return f'{self.first_name} {self.last_name}'
```

- Izvršite `migracije` pomoću `makemigrations` komande:

  ```shell
  python manage.py makemigrations
  ```

  Izlaz:

  ```shell
  Migrations for 'hr':
    hr\migrations\0001_initial.py
      - Create model Employee
  ```

- Prosledite izmene u bazu podataka pomoću `migrate` komande:

  ```shell
  python manage.py migrate
  ```

Django kreira mnogo tabela, uključujući i one za ugrađene modele kao što su `User` i `Group`. U ovom tutorijalu ćemo se fokusirati na `Employee` klasu.

Na osnovu `Employee` klase modela, Django ORM kreira tabelu `hr_employee` u bazi podataka.

Django kombinuje `ime aplikacije` i `ime klase` da bi generisao `ime tabele`:

```py
app_modelclass
```

U ovom primeru, naziv aplikacije je `hr`, a naziv klase je `Employee`. Stoga, Django kreira tabelu sa nazivom `hr_employee`. Imajte na umu da Django konvertuje naziv klase u mala slova pre nego što je doda nazivu aplikacije.

Klasa `Employee` modela ima dva polja `first_name` i `last_name`. Pošto klasa `Employee` nasleđuje od `models.Modelklase`, Django automatski dodaje polje `id` kao polje za automatsko povećanje pod nazivom `id`. Stoga, `hr_employee` tabela ima tri kolone `id`, `first_name` i `last_name`.

Da biste interagovali sa `hr_employee` tabelom, možete pokrenuti `shell_plus` komandu koja dolazi iz `django-extensions` paketa.

Imajte na umu da vam Django pruža ugrađenu `shell` komandu. Međutim, `shell_plus` komanda je praktičnija za rad. Na primer, ona automatski učitava modele definisane u projektu i prikazuje generisani SQL.

Pokrenite `shell_plus` komandu sa `--print-sql` opcijom:

```py
python manage.py shell_plus --print-sql
```

#### Unošenje podataka

- Kreirajte novi `Employee` objekat i pozovite `save()` metodu za umetanje novog reda u tabelu:

  ```shell
  >>> e = Employee(first_name='John',last_name='Doe')
  >>> e.save()
  INSERT INTO "hr_employee" ("first_name", "last_name")
  VALUES ('John', 'Doe') RETURNING "hr_employee"."id"
  Execution time: 0.003234s [Database: default]
  ```

  U ovom primeru, ne morate da podešavate vrednost za kolonu `id`. Baza podataka automatski generiše njenu vrednost i Django će je prihvatiti kada ubacite novi red u tabelu.

  Kao što je prikazano na izlazu, Django koristi `INSERT` naredbu da ubaci novi red sa dve kolone `first_name` i `last_name` u tabelu `hr_employee`.

- Unesite drugog zaposlenog sa imenom Jane i prezimenom Doe:

  ```shell
  >>> e = Employee(first_name='Jane',last_name='Doe')
  >>> e.save()
  INSERT INTO "hr_employee" ("first_name", "last_name")
  VALUES ('Jane', 'Doe') RETURNING "hr_employee"."id"
  Execution time: 0.002195s [Database: default]
  ```

  Sada, `hr_employee` tabela ima dva reda sa `ID` vrednostima 1 i 2.

#### Izbor podataka

Da biste izabrali sve redove iz `hr_employees` tabele, koristite `all()` metodu ovako:

```shell
>>> Employee.objects.all()
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name"
  FROM "hr_employee"
 LIMIT 21
Execution time: 0.001856s [Database: default]
<QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
```

Kako ovo funkcioniše.

- Prvo, Django koristi `SELECT` naredbu da bi izabrao redove iz `hr_employee` tabele.
- Drugo, Django konvertuje redove u `Employee` objekte i vraća tip `QuerySet` koji sadrži `Employee` objekte.

Obratite pažnju da je Django dodao `LIMIT` da bi vratio 21 zapis za prikazivanje na shell-u.

Da biste izabrali jedan red po `ID`, možete koristiti `get()` metodu. Na primer, sledeći kod vraća zaposlenog sa ID-om 1:

```shell
>>> e = Employee.objects.get(id=1)
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name"
  FROM "hr_employee"
 WHERE "hr_employee"."id" = 1
 LIMIT 21
Execution time: 0.001003s [Database: default]
>>> e
<Employee: John Doe>
```

Za razliku od `all()` metode, `get()` metod vraća `Employee` objekat umesto `QuerySet`.

Da biste pronašli zaposlene po njihovom imenu, možete koristiti `filter()` metod objekta `QuerySet`. Na primer, sledeći kod pronalazi zaposlene sa imenom Jane:

```shell
>>> Employee.objects.filter(first_name='Jane')
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name"
  FROM "hr_employee"
 WHERE "hr_employee"."first_name" = 'Jane'
 LIMIT 21
Execution time: 0.000000s [Database: default]
<QuerySet [<Employee: Jane Doe>]>
```

#### Ažuriranje podataka

- Izaberite zaposlenog sa ID-om 2:

  ```shell
  >>> e = Employee.objects.get(id=2)
  SELECT "hr_employee"."id",
        "hr_employee"."first_name",
        "hr_employee"."last_name"
    FROM "hr_employee"
  WHERE "hr_employee"."id" = 2
  LIMIT 21
  Execution time: 0.001022s [Database: default]
  ```

- Ažurirajte prezime izabranog zaposlenog na Smith:

  ```shell
  >>> e.last_name = 'Smith'
  >>> e.save()
  UPDATE "hr_employee"
    SET "first_name" = 'Jane',
        "last_name" = 'Smith'
  WHERE "hr_employee"."id" = 2
  Execution time: 0.004019s [Database: default]
  >>> e
  <Employee: Jane Smith>
  ```

#### Brisanje podataka

Da biste obrisali instancu modela, koristite `delete()` metodu. Sledeći primer briše zaposlenog sa ID-om 2:

```shell
>>> e.delete()
DELETE
  FROM "hr_employee"
 WHERE "hr_employee"."id" IN (2)
Execution time: 0.002001s [Database: default]
(1, {'hr.Employee': 1})
```

Da biste obrisali sve instance modela, koristite `all()` metodu za izbor svih zaposlenih i pozovite `delete()` metodu za brisanje svih izabranih zaposlenih:

```shell
>>> Employee.objects.all().delete()
DELETE
  FROM "hr_employee"
Execution time: 0.001076s [Database: default]
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

### Uvod u relaciju jedan na jedan

Relacija `jedan-na-jedan` definiše vezu između dve tabele, gde svakom redu u jednoj tabeli odgovara samo jedan red u drugoj tabeli.

Na primer, svaki zaposleni ima kontakt i svaki kontakt pripada jednom zaposlenom. Dakle, odnos između zaposlenih i kontakata je odnos `jedan-na-jedan`.

Da biste kreirali relaciju `jedan-na-jedan`, koristite `OneToOneField` klasu:

```py
OneToOneField(to, on_delete, parent_link=False, **options)
```

U ovoj sintaksi:

- `to` parametar definiše naziv modela.
- `on_delete` određuje akciju nad kontaktom kada se zaposleni obriše.

Sledeći primer koristi `OneToOneField` klasu za definisanje relacije `jedan-na-jedan` između `Contact` i `Employee` modela u `models.py`:

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

Imajte na umu da Django kreira ograničenje stranog ključa u bazi podataka bez `ON DELETE CASCADE` opcije. Umesto toga, Django ručno obrađuje brisanje u aplikaciji. Imajte na umu da se ova interna implementacija može promeniti u budućnosti.

### Migriranje modela u bazu podataka

- Izvršite migracije pomoću `makemigrations` komande:

  ```shell
  python manage.py makemigrations

  Migrations for 'hr':
    hr\migrations\0002_contact_employee_contact.py
      - Create model Contact
      - Add field contact to employee
  ```

- Primenite migracije na bazu podataka pomoću `migrate` komande:

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

- Kreirajte novi `Employee` objekat i sačuvajte ga u bazi podataka:

  ```shell
  >>> e = Employee(first_name='John',last_name='Doe')
  >>> e.save()
  ```

  Django izvršava sledeću SQL komandu:

  ```sql
  INSERT INTO "hr_employee" ("first_name", "last_name", "contact_id")
  VALUES ('John', 'Doe', NULL) 
  RETURNING "hr_employee"."id"
  ```

- Kreirajte i sačuvajte novi kontakt u bazi podataka:

  ```shell
  >>> c = Contact(phone='40812345678', address='101 N 1st Street, San Jose, CA')
  >>> c.save()
  ```

  Django takođe izvršava sledeću INSERTkomandu:

    ```sql
    INSERT INTO "hr_contact" ("phone", "address")
    VALUES ('40812345678', '101 N 1st Street, San Jose, CA') 
    RETURNING "hr_contact"."id"
  ```

- Povežite kontakt sa zaposlenim:

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

### Dobijanje podataka iz odnosa jedan-na-jedan

- Pronađite zaposlenog sa imenom John Doe:

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
<Contact: 40812345678>
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

Imajte na umu da `Contact` klasa nema `employee` atribut. Međutim, možete mu pristupiti ako je kontakt povezan.

Hajde da napravimo još jedan `contact` koji se ne povezuje ni sa jednim employee objektom:

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

- Kreirajte novog zaposlenog:

```shell
>>> e = Employee(first_name='Jane',last_name='Doe')
>>> e.save()
INSERT INTO "hr_employee" ("first_name", "last_name", "contact_id")
VALUES ('Jane', 'Doe', NULL) RETURNING "hr_employee"."id"
Execution time: 0.003079s [Database: default]
```

- Pokupite sve zaposlene:

```shell
>>> Employee.objects.all()
```

DŽango vraća dva zaposlena:

```shell
<QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
```

Ako morate da prikažete sve zaposlene, kao i njihove kontakte na istoj stranici, onda imate problem `N+1` upita:

- Prvo, potreban vam je jedan upit da biste dobili sve zaposlene (N zaposlenih).
- Drugo, potrebno vam je N upita da biste izabrali povezani kontakt svakog zaposlenog.

Da biste ovo izbegli, možete pošaljiti upit svim zaposlenima i kontaktima pomoću jednog upita koristeći `select_related()` metodu:

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

## Relacija jedan-na-više

### Uvod u relacije jedan-na-više

U relaciji `jedan-na-više`, red u tabeli je povezan sa jednim ili više redova u drugoj tabeli. Na primer, depatment (odeljenje) može imati jednog ili više employees (zaposlenih) i svaki employee  pripada jednom departmentu.

Relacija između departmenta i employee je `jedan-na-više`. Suprotno tome, relacija između employee i departmenta je `više-na-jedan`.

Da biste kreirali relaciju `jedan-na-više` u Django-u, koristite `ForeignKey`. Na primer, sledeći kod koristi `ForeignKey` da bi kreirao relaciju `jedan-na-više` između `Department` i `Employee` modela:

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

- Definišite `Department` klasu modela.

- Izmenite `Employee` klasu dodavanjem odnosa `jedan-na-više` koristeći `ForeignKey`:

  ```py
  department = models.ForeignKey(
    Department,
    on_delete=models.CASCADE
  )
  ```

  U `ForeignKey`, prosleđujemo `Department` kao prvi argument i `on_delete` ključnu reč kao drugi argument. `on_delete=models.CASCADE` označava da ako se department obriše, svi zaposleni povezani sa tim odeljenjem takođe se brišu.

  > Imajte na umu da polje definišete sa `ForeignKey` na strani `"više"` relacije.

- Izvršite migracije pomoću `makemigrations` komande:

  ```shell
  python manage.py makemigrations
  ```

  Django će izdati sledeću poruku:

  > It is impossible to add a non-nullable field 'department' to employee without specifying a default. This is because the database needs something to populate existing rows. </br>
  Please select a fix:
  > 1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
  > 2) Quit and manually define a default value in `models.py`.
  >
  > Select an option:

    U `Employee` modelu, polje `department` definišemo kao polje koje se ne može nulirati. Stoga, Django-u je potrebna podrazumevana vrednost za ažuriranje `department` polja (ili `department_id` kolone) za postojeće redove u tabeli baze podataka.

    Čak i ako `hr_employees` tabela nema redove, Django takođe zahteva da unesete podrazumevanu vrednost.

    Kao što je jasno prikazano u poruci, Django vam pruža dve opcije. Potrebno je da unesete 1 ili 2 da biste izabrali odgovarajuću opciju.

  - Ako izaberete prvu opciju, Django zahteva jednokratnu podrazumevanu vrednost. Ako unesete 1, Django će prikazati komandnu liniju Python-a:

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

  - Ako izaberete drugu opciju unosom broja 2, Django će vam omogućiti da ručno definišete podrazumevanu vrednost za polje `department`. U ovom slučaju, možete dodati podrazumevanu vrednost polju department `models.py` na ovaj način:

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
  ```
  
  Izlaz:

  ```shell
  Operations to perform:
    Apply all migrations: admin, auth, contenttypes, hr, sessions
  Running migrations:
    Applying hr.0002_department_employee_department... OK
  ```

  Drugi način da se ovo reši je resetovanje migracija koje ćemo obraditi u tutorijalu za resetovanje migracija.

### Interakcija sa modelima

- Pokrenite `shell_plus` komandu:

  ```shell
  python manage.py shell_plus
  ```

- Kreirajte novo odeljenje sa nazivom IT:

  ```shell
  >>> d = Department(name='IT',description='Information Technology')
  >>> d.save()
  ```

- Kreirajte dva zaposlena i dodelite ih odeljenje IT:

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

- Dobijte sve zaposlene u odeljenju koristeći `employee_set` atribut ovako:

  ```shell
  >>> d.employee_set.all()
  <QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
  ```

  Imajte na umu da nismo definisali `employee_set` svojstvo u `Department` modelu. Interno, Django je automatski dodao `employee_set` svojstvo modelu `Department` kada smo definisali odnos `jedan-na-više` koristeći `ForeignKey`.

  Metod `all()` vraća `employee_set` rezultat u `QuerySet`-u koji sadrži sve zaposlene koji pripadaju IT odeljenju.

### Korišćenje select_related() za spajanje zaposlenog sa odeljenjem

Izađite iz `shell_plus` i ponovo ga izvršite. Ovaj put dodajemo `--print-sql` opciju za prikaz generisanog SQL-a koji će Django izvršiti nad bazom podataka:

```shell
python manage.py shell_plus --print-sql
```

Sledeće vraća prvog zaposlenog:

```shell
>>> e = Employee.objects.first()
SELECT "hr_employee"."id",
       "hr_employee"."first_name",
       "hr_employee"."last_name",
       "hr_employee"."contact_id",
       "hr_employee"."department_id"
  FROM "hr_employee"
 ORDER BY "hr_employee"."id" ASC
 LIMIT 1
Execution time: 0.003000s [Database: default]
```

Da biste pristupili odeljenju prvog zaposlenog, koristite `department` atribut:

```shell
>>> e.department 
SELECT "hr_department"."id",
       "hr_department"."name",
       "hr_department"."description"
  FROM "hr_department"
 WHERE "hr_department"."id" = 1
 LIMIT 21
Execution time: 0.013211s [Database: default]
<Department: IT>
```

U ovom slučaju, Django izvršava dva upita. Prvi upit bira prvog zaposlenog, a drugi upit bira odeljenje izabranog zaposlenog.

Ako izaberete N zaposlenih da biste ih prikazali na veb stranici, onda morate da izvršite N + 1 upit da biste dobili i zaposlene i njihova odeljenja. Prvi upit (1) bira N zaposlenih, a N upita bira N odeljenja za svakog zaposlenog. Ovaj problem je poznat kao "problem N + 1 upita".

Da biste rešili problem sa N + 1 upitom, možete koristiti `select_related()` metod za izbor i zaposlenih i odeljenja pomoću jednog upita. Na primer:

```shell
>>> Employee.objects.select_related('department').all()
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
Execution time: 0.012124s [Database: default]
<QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
```

U ovom primeru, Django izvršava samo jedan upit koji spaja tabele ` hr_employeeand` i `hr_department`.

### Rezime

- U relaciji `jedan-na-više`, red u tabeli je povezan sa jednim ili više redova u drugoj tabeli.
- Koristi se `ForeignKey` za uspostavljanje odnosa `jedan-na-više` između modela u Django-u.
- Definišite `ForeignKey` umodelu na strani `"više"` relacije.
- Koristite `select_related()` metodu za spajanje dve ili više tabela u relacijama `jedan-na-više`.

[Sadržaj](#sadržaj)

## Relacija "više-na-više"

### Uvod u relaciju "više-na-više"

U relaciji `"više-na-više"`, više redova u jednoj tabeli je povezano sa više redova u drugoj tabeli.

Na primer, zaposleni može imati više programa kompenzacije, a programu kompenzacije može pripadati više zaposlenih.

Stoga, više redova u tabeli zaposlenih povezano je sa više redova u tabeli kompenzacija. Dakle, odnos između zaposlenih i programa kompenzacija je relacija `"više-na-više"`.

Tipično, relacione baze podataka ne implementiraju direktan odnos „više-na-više“ između dve tabele. Umesto toga, koriste treću tabelu, tabelu za spajanje, da bi uspostavile dva odnosa `"jedan-na-više"` između dve tabele i tabele za spajanje.

Tabela `hr_employee_compensations` je tabela za spajanje. Ima dva strana ključa `employee_id` i `compensation_id`.

Strani `employee_id` ključ referencira na `id` tabele `hr_employee`, a strani ključ `compensation_id`  referencira na `id` u `hr_compensation` tabeli.

Obično vam nije potrebna `id` kolona u `hr_employee_compensations` tabeli kao primarni ključ i koristite `employee_id` i `compensation_id` kao složeni primarni ključ. Međutim, Django uvek kreira `id` kolonu kao primarni ključ za tabelu koja se spaja.

Takođe, Django kreira jedinstveno ograničenje koje uključuje kolone `employee_id` i `compensation_id`. Drugim rečima, u tabeli `hr_employee_compensations` neće biti duplih parova vrednosti `employee_id` i `compensation_id`.

Da biste kreirali relaciju `"više-na-više"` u Django-u, koristite `ManyToManyField`. Na primer, sledeći kod koristi `ManyToManyField` da bi kreirao relaciju `"više-na-više"` između `Employee` i `Compensation` modela:

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

- Definišite novu `Compensation` klasu modela koja proširuje `models.Model` klasu.

- Dodajte `compensations` polje klasi `Employee`. `compensations` polje koristi `ManyToManyField` da bi uspostavilo odnos `"više-na-više"` između klasa `Employee` i `Compensation`.

- Da biste proširili izmene modela u bazu podataka, pokrećete `makemigrations` komandu:

  ```shell
  python manage.py makemigrations
  ```

  Izbaciće nešto ovako:

  ```shell
  Migrations for 'hr':
    hr\migrations\0004_compensation_employee_compensations.py
      - Create model Compensation
      - Add field compensations to employee
  ```

- I izvršite `migrate` komandu:

  ```shell
  python manage.py migrate
  ```

  Izlaz:

  ```shell
  Operations to perform:
    Apply all migrations: admin, auth, contenttypes, hr, sessions
  Running migrations:
    Applying hr.0004_compensation_employee_compensations... OK  
  ```

Django je kreirao dve nove tabele `hr_compensation` i jednu tabelu za spajanje `hr_employee_compensations`.

### Kreiranje podataka

- Pokrenite `shell_plus` komandu:

  ```shell
  python manage.py shell_plus
  ```

- Kreirajte tri programa kompenzacije, uključujući Stock, Bonuses i Profit Sharing:

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

- Nabavite zaposlenog sa imenom John i prezimenom Doe:

```shell
>>> e = Employee.objects.filter(first_name='John',last_name='Doe').first()
>>> e
<Employee: John Doe>
```

### Dodavanje kompenzacija zaposlenima

- Upišite John Doe u programe kompenzacije stock( c1 ) i bonuses ( c2 ) koristeći add() metod atributa compensationsi save()metod objekta Employee:

  ```shell
  >>> e.compensations.add(c1)
  >>> e.compensations.add(c2) 
  >>> e.save()
  ```

- Pristupite svim `compensations` programima John Doe koristeći `all()` metod atributa compensations:

  ```shell
  >>> e.compensations.all()
  <QuerySet [<Compensation: Stock>, <Compensation: Bonuses>]>
  ```

  Kao što je jasno prikazano na izlazu, John Doe ima dva programa kompenzacije.

- U upišite se Jane Doe u tri programa kompenzacije, uključujući akcije, bonuse i podelu dobiti:

  ```shell
  >>> e = Employee.objects.filter(first_name='Jane',last_name='Doe').first()
  >>> e 
  <Employee: Jane Doe>
  >>> e.compensations.add(c1)
  >>> e.compensations.add(c2) 
  >>> e.compensations.add(c3) 
  >>> e.save()
  >>> e.compensations.all()
  <QuerySet [<Compensation: Stock>, <Compensation: Bonuses>, <Compensation: Profit Sharing>]>
  ```

  Interno, Django je ubacio identifikacione brojeve zaposlenih i kompenzacija u tabelu pridruživanja:

  ```shell
  id | employee_id | compensation_id
  ----+-------------+-----------------
    1 |           5 |               1
    2 |           5 |               2
    3 |           6 |               1
    4 |           6 |               2
    5 |           6 |               3
  (5 rows)
  ```

- Pronađite sve zaposlene koji su bili uključeni u plan nadoknade akcijama koristeći `employee_set` atribut objekta `Compensation`:

  ```shell
  >>> c1
  <Compensation: Stock>
  >>> c1.employee_set.all()
  <QuerySet [<Employee: John Doe>, <Employee: Jane Doe>]>
  ```

  Vraćeno je dva zaposlena kako je i očekivano.

- Možete koristiti `employee_set` atribut da pronađete sve zaposlene koji imaju program kompenzacije za učešće u dobiti:

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

```
>>> Employee.objects.filter(compensations__name="Profit Sharing") 
<QuerySet [<Employee: Jane Doe>]>
```

### Ukidanje kompenzacija zaposlenima

Da biste uklonili program kompenzacije zaposlenom, koristite `remove()` metod atributa `compensations` objekta Employee. Na primer:

- Pronađite zaposlenog čije je ime Jane Doe:

  ```shell
  >>> e = Employee.objects.filter(first_name='Jane',last_name='Doe').first()
  <Employee: Jane Doe>
  ```

- Uklonite "profit sharing" kompenzaciju ( c3 ) iz za Jane Doe i sačuvajte izmene:

  ```shell
  >>> e.compensations.remove(c3)
  >>> e.save()
  ```

- Nabavite sve programe kompenzacije od Jane Doe:

  ```shell
  >>> e.compensations.all()
  <QuerySet [<Compensation: Stock>, <Compensation: Bonuses>]>
  ```

Sada su Jane Doe preostala dva programa kompenzacije.

### Rezime relacije "više na više"

- U relaciji `"više-prema-više"`, više redova u jednoj tabeli je povezano sa više redova u drugoj tabeli.
- Relacione baze podataka koriste tabelu za spajanje da bi uspostavile odnos `"više-prema-više"` između dve tabele.
- Koristite `ManyToManyField` za modeliranje relacije `"više-prema-više"` između modela u Django-u.

[Sadržaj](#sadržaj)

## ManyToManyField through

U relaciji `"više-na-više"`, više redova u jednoj tabeli je povezano sa više redova u drugoj tabeli. Da bi se uspostavio odnos `"više-na-više"`, relacione baze podataka koriste treću tabelu koja se naziva tabela za spajanje i kreiraju dva odnosa `"jedan-na-više"` iz izvornih tabela.

Tipično, tabela spajanja sadrži vrednosti identifikatora izvornih tabela tako da redovi u jednoj tabeli mogu biti povezani sa redovima u drugoj tabeli.

Ponekad ćete možda želeti da dodate dodatna polja u tabelu za pridruživanje. Na primer, svaki zaposleni može imati više poslova tokom svoje karijere.

Da biste pratili kada zaposleni preuzima posao, možete dodati polja `begin_date` i `end_date` u tabelu za pridruživanje.

Da biste to uradili u Django-u, koristite argument `ManyToManyField` `through`.

Na primer, sledeće pokazuje kako povezati zaposlenog sa više poslova putem dodela:

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

- Definišite `Job` model, dodate `employees` atribut koji koristi `ManyToManyField` i prosledite ga `Assignment` kao `through` argument.
- Definišite `Assignment` klasu koja ima dva strana ključa, jedan je povezan sa `Employee` modelom, a drugi je povezan sa `Job` modelom. Takođe, dodajte atribute `begin_date` i `end_date` modelu `Assignment`.
- Pokrenite `makemigrations` da biste napravili nove migracije:

  ```py
  python manage.py makemigrations
  ```

  Izlaz:

  ```shell
  Migrations for 'hr':
    hr\migrations\0005_assignment_job_assignment_job.py
      - Create model Assignment
      - Create model Job
      - Add field job to assignment
  ```

- Izvršite `migrate` komandu da biste primenili promene na bazu podataka:

  ```shell
  python manage.py migrate
  ```

  Izlaz:

  ```shell
  Operations to perform:
    Apply all migrations: admin, auth, contenttypes, hr, sessions
  Running migrations:
    Applying hr.0005_assignment_job_assignment_job... OK
  ```

Iza kulisa, Django kreira tabele `hr_job` i `hr_assignment` u bazi podataka:
`hr_assignment` je tabela spajanja. Pored polja `employee_id` i `position_id`, ona ima `begin_date`i `end_date` polja.

### Stvaranje novih radnih mesta

- Pokrenite `shell_plus` komandu:

  ```shell
  python manage.py shell_plus
  ```

- Kreirajte tri nove pozicije:

  ```shell
  >>> j1 = Job(title='Software Engineer I')
  >>> j1.save()
  >>> j2 = Job(title='Software Engineer II') 
  >>> j2.save() 
  >>> j3 = Job(title='Software Engineer III')
  >>> j3.save()
  >>> Job.objects.all()
  <QuerySet [<Job: Software Engineer I>, <Job: Software Engineer II>, <Job: Software Engineer III>]>
  ```

### Kreiranje instanci za spojni model

- Pronađite zaposlenog sa imenom "John Doe" i "Jane Doe":

  ```shell
  >>> e1 = Employee.objects.filter(first_name='John',last_name='Doe').first()
  >>> e1
  <Employee: John Doe>
  >>> e2 = Employee.objects.filter(first_name='Jane', last_name='Doe').first()
  >>> e2
  <Employee: Jane Doe>
  ```

- Kreirajte instance srednjeg modela ( `Assignment` ):

  ```shell
  >>> from datetime import date
  >>> a1 = Assignment(employee=e1,job=j1, begin_date=date(2019,1,1), end_date=date(2021,12,31))
  >>> a1.save()
  >>> a2 = Assignment(employee=e1,job=j2, begin_date=date(2022,1,1))
  >>> a2.save()
  >>> a3 = Assignment(employee=e2, job=j1, begin_date=date(2019, 3, 1))
  >>> a3.save()
  ```

- Pronađite zaposlene koji zauzimaju "Software Engineer I" poziciju ( p1 ):

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

Slično tome, možete pronaći sve zaposlene koji zauzimaju tu "Software Engineer II" poziciju:

```shell
>>> j2.employees.all()
<QuerySet [<Employee: John Doe>]>
Kodni jezik:  Pajton  ( python )
```

### Uklanjanje instanci spojnog modela

- Uklonite "Jane Doe" ( e2 ) iz "Software Engineer II" posla koristeći `remove()` metodu:

  ```shell
  >>> j2.employees.remove(e2) 
  ```

- Uklonite sve zaposlene sa "Software Engineer I" posla koristeći sledeću `clear()` metodu:

  ```shell
  >>> j1.employees.clear() 
  ```

Posao "j1" sada ne bi trebalo da ima zaposlene:

```shell
>>> j1.employees.all() 
<QuerySet []>
```

### Rezime through

- Koristite `through` argument u `ManyToManyField` da biste dodali dodatna polja u relaciju `"više-na-više"`.

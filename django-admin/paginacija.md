
# Efikasna paginacija

## Sadržaj

[Nazad](sadrzaj.md)

- [Razumevanje naivne paginacije](#razumevanje-naivne-paginacije)
- [Predstava naivne paginacije](#predstava-naivne-paginacije)
- [Rukovanje paginacijom u Djangu](#rukovanje-paginacijom-u-djangu)
  - [Uklanjanje Count upita](#uklanjanje-count-upita)
  - [Približno COUNT](#približno-count)
  - [Paginacija sa setovanjem ključa](#paginacija-sa-setovanjem-ključa)
- [Korišćenje dodatka `dj-pagination za Django`](#korišćenje-dodatka-dj-pagination)
- [Zaključak paginacija](#zaključak-paginacija)

Moglo bi se reći da većina veb okvira koristi naivan pristup paginaciji. Korišćenjem  PostgreSQL `COUNT`, `LIMIT` i `OFFSET` radi dobro za većinu veb aplikacija, ali ako imate tabele sa milionima zapisa ili više, degradira performanse brzo.

Django je odličan okvir za izradu veb aplikacija, ali njegova podrazumevana metoda paginacije u velikoj meri pada u ovu zamku. U ovom članku ću vam pomoći da razumete ograničenja Django-ove stranice i ponuditi tri alternativne metode koje će poboljšati performanse vaše aplikacije. Usput ćete videti kompromise i slučajeve upotrebe za svaku metodu tako da možete odlučiti koja najbolje odgovara vašoj aplikaciji.

### Razumevanje naivne paginacije

Pogledajmo primer vrste kontrole paginacije koju veb aplikacija može da koristi:

- U ovoj kontroli korisnik može da ode na prethodnu i sledeću stranicu ili da pređe direktno na određenu stranicu. Upit da biste dobili desetu stranicu koristeći Postgres-ov `LIMIT` i `OFFSET` pristup mogao bi izgledati ovako:

```sql
SELECT *
  FROM users
  ORDER BY created_at DESC
  LIMIT 10 OFFSET 100;
```

Uočite da za dobijanje ispravnog `OFFSET` broja stranice morate pomnožiti broj stranice x `LIMIT`.

Potreban je još jedan upit za prikaz naše kontrole paginacije. Morate znati broj zapisa u tabeli. Bez tih informacija nećete znati koliko stranica morate da pretražite.

```sql
SELECT count(*) 
  FROM users
```

Možda ćete videti kako ovaj pristup vrlo brzo može postati pravi problem performansi.

[Sadržaj](#sadržaj)

### Predstava naivne paginacije

Da biste bolje razumeli uska grla performansi `LIMIT` i `OFFSET`, možete uvesti neke testne podatke i isprobati ih.

Prvo, napravite tabelu dovoljno veliku da naiđe na usporavanja:

```sql
CREATE TABLE USERS ( id serial, name varchar(50) );

INSERT INTO users SELECT
  --- Ten million records generate_series(1,10000000) AS id,
  --- Example: "e6f2c6842d146c518185e1e47add9532"
  substr(md5(random()::text), 0, 50) AS name;
```

Kada pokrenete upit da biste dobili desetu stranicu rezultata, odgovor je gotovo trenutan. Na svom Macbook Pro računaru iz 2018. sa najnovijom verzijom Postgresa vidim podatke u 89 ms.

Međutim, za upite dalje, vreme čekanja se povećava. Uz `LIMIT` od 10 i `OFFSET` od     5.000.000, odgovor traje 2,62 sekunde.

Na kraju, možete pogledati `COUNT` upit koji se izvodi u svih 10 miliona redova. Taj upit sada traje letargičnih 4,45 sekundi.

Spora performansa u ovim primerima je izazvana na način na koji `OFFSET` i `COUNT` rade. Da biste sa `OFFSET` došli do navedene stranice, potrebno je da baza podataka pređe svaki indeks do stranice koju želite. Zbog toga se performanse degradiraju što dalje zavirite u tabelu.

Naivni pristup paginaciji koja koristi `COUNT`, `LIMIT` i `OFFSET` je održivo rešenje za tabele ispod milion redova. U velikim tabelama možete očekivati spore upite i loše korisničko iskustvo.

[Sadržaj](#sadržaj)

### Rukovanje paginacijom u Djangu

Sada kada imate osnovno znanje o performansama upita za paginaciju u Postgresu, možete početi da shvatate zašto paginacija usporava u Djangu. Da bih pokazao kako Django upravlja paginacijom, napravio sam novu aplikaciju sa modelom korisnika i ubacio 10 miliona zapisa prilagođavajući naš raniji upit.

`{project}/users/models.py`

```py
from django.db import models

class User(models.Model):
    name = models.CharField(max_length=50)
```

Koristio sam administratorsku stranicu da testiram brzinu paginacije.

`{project}/users/admin.py`

```py
from django.contrib import admin from .models import User

admin.site.register(User)
```

Sada kada je User model predstavljen na admin panelu, mogu videti tabelu sa 10 miliona zapisa.

Koristeći `django-debug-toolbar` mogu zaviriti u SQL upite koje Django generiše u realnom vremenu. Za generisanje ovog korisničkog interfejsa koriste se dva upita:

```sql
-- Count the total number of records - 2.43 seconds
SELECT COUNT(*) AS " count" 
  FROM "users_user"

-- Get first page of items - 2ms
SELECT "users_user"."id","users_user"."name" 
  FROM "users_user"
  ORDER BY "users_user"."id" DESC 
  LIMIT 100
```

Ovi upiti bi trebali izgledati poznato jer su gotovo identični gornjim upitima naivne paginacije. Čudno, upit za brojanje se pokreće dva puta, što znači da kada učitate Django admin panel, morate čekati da baza podataka dva puta prebroji svaki red tabele.

Kada kliknete na stranicu 99,999, Django će ponovo pokrenuti dva upita za brojanje i još jedan upit za paginaciju koristeći LIMIT i OFFSET:

```sql
-- Get the 99,999 page (100 results per page) - 13.34 seconds
SELECT "users_user"."id", "users_user"."name" FROM "users_user"
  ORDER BY "users_user"."id" DESC
  LIMIT 100 OFFSET 9999900
```

Ovom upitu je potrebno 13 sekundi da se završi!

Jasno je da je naivan pristup paginaciji u Djangu spor za velike tabele. Vremenom će vaše tablice baze podataka verovatno rasti, a kako dostignu desetine miliona zapisa, vi i vaši klijenti ćete početi da primećujete ovo užasno vreme učitavanja. Šta možete učiniti u vezi s tim?

U sledećim odeljcima pokazaću vam tri opcije za poboljšanje performansi paginacije u aplikaciji Django.

[Sadržaj](#sadržaj)

#### Uklanjanje COUNT upita

`COUNT` upit dominira u vremenu učitavanja za prvu stranicu rezultata. Prilikom preskakanja na kasnije stranice, `OFFSET` upit je najsporiji, ali fokusiraću se na poboljšanje `COUNT` u ovoj prvoj opciji.

Ovo može iznenaditi, ali jedno rešenje je potpuno uklanjanje upita za brojanje. Zar to neće slomiti korisnički interfejs !?

Nekako ... U ovom slučaju, moglo bi biti razumno ne znati koliko stranica ima u tabeli "Users". Ne dešava se često da se korisnici nađu na pet milionitoj stranici tabele zapisa. Navigacija do sledeće i prethodne stranice obično je dovoljna kontrola. Korišćenje okvira za pretragu ili filtera je verovatno bolji metod za pronalaženje zapisa koji želite.

Pogledajte Django-ova polja za pretraživanje za informacije o omogućavanju pretraživanja i filtriranja na admin tabli i slobodno pročitajte pganalize članak o potpunom pretraživanju teksta u Djangu.

Najpoznatiji primer ove vrste paginacije je stranica sa rezultatima Google pretrage. Pri dnu stranice, skraćena kontrola paginacije prikazuje direktne veze samo na prvih 10 stranica.

To ne znači da vas Google sprečava da vidite ostale milijarde rezultata. Jednostavno vam govori da bi precizniji termin za pretragu bio bolji način da dođete do tih rezultata od paginacije.

Da se oslobodite `COUNT` smisla u svojoj aplikaciji, Django olakšava skrivanje. Prvo prepišite svojstvo `count` podrazumevanog Paginator -a :

```py
# {project}/users/paginator.py
from django.core.paginator import Paginator
from django.utils.functional import cached_property

class UserPaginator(Paginator):
    @cached_property
    def count(self):
        return 9999999999
```

Primetite da je vrednost čuvara mesta broj koji je mnogo veći nego što očekujete da ćete imati rezultate.

Django reaguje na ovo prilagođavanje tako što prikazuje prvih nekoliko stranica u komponenti paginacije, kao što biste očekivali. Međutim, poslednjih nekoliko stranica će biti lažni. Kada kliknete na stranicu koja ne postoji, Django će vas odvesti na poslednju stranicu bez obzira na to koliko zapisa imate.

Nakon što zamenite podrazumevani paginator, uvezite ga u svoj administratorski model:

`{project}/users/admin.py`

```py
from django.contrib import admin from .models import User
from .paginator import UserPaginator

@admin.register(User)
class UserTableAdmin(admin.ModelAdmin):
    show_full_result_count = False
    paginator = UserPaginator
```

Imajte na umu da sam takođe postavio `show_full_result_count` na `False`. Ovo će isključiti drugi upit za brojanje koji sam ranije primetio.

Nakon ažuriranja aplikacije ovim promenama, smanjio sam vreme za prvu stranicu sa ~5 sekundi na 8ms. Imajte na umu da ova tabela i dalje pati od sporih `OFFSET` upita. Prelazak na stranicu 50,0000 trajao je 18 sekundi. Pre nego što vam pokažem kako da rešite `OFFSET` problem, pokazaću vam još jedan način za poboljšanje `COUNT` upita.

[Sadržaj](#sadržaj)

### Približno COUNT

Drugi način da se smanji vreme provedeno na COUNT upitu korišćenjem nekih ugrađenih Postgres funkcija za procenu ukupnog broja zapisa kada brojanje traje predugo. Možete videti temeljnu implementaciju pristupa koja preopterećuje count metodu u Djangoovoj `Paginator` klasi.

Prva stvar da se uradi je da se postavi `statement_timeout` na upit i rezervnu opciju procenjenih zapisa. Možete koristiti atomske transakcije da postavite vremensko ograničenje na 150 ms.

```py
try:
  with transaction.atomic(), connection.cursor() as cursor:
    # Limit to 150 ms
    cursor.execute('SET LOCAL statement_timeout TO 150;')
    return super().count
except OperationalError:
  pass
...
```

Ako `count` metoda vraća podatke pre isteka vremenskog ograničenja, onda se koristi stvarna vrednost. Međutim, ako upit potraje duže, Django će se vratiti na približnu vrednost uskladištenu u `pg_class` metapodacima. Metapodaci se ažuriraju kada se izvršavaju komande kao `VACUUM`, `ANALYZE` i `CREATE INDEX`, ili se radi `autovacuum`  na tabeli.

```py
...
with transaction.atomic(), connection.cursor() as cursor:
  # Obtain estimated values (only valid with PostgreSQL)
  cursor.execute("SELECT reltuples FROM pg_class WHERE relname = %s",
    [self.object_list.query.model._meta.db_table])
  estimate = int(cursor.fetchone()[0])
  return estimate
  ...
```

Nakon primene ove metode, maksimalno vreme koje ćete potrošiti na učitavanje COUNT je 150 ms.

[Sadržaj](#sadržaj)

#### Paginacija sa setovanjem ključa

Da biste rešili spor `OFFSET` problem, možete ga zameniti paginacijom skupa ključeva (ili pretrage). U paginaciji skupa ključeva, svaku stranicu preuzima poređano polje poput `id` ili `created_at` datuma. Umesto ponavljanja brojanja stranica, kao što se `OFFSET` to dešava, paginacija ključeva filtrira direktno poredano polje.

**Primer korišćenja paginacije skupova ključeva**:

```sql
SELECT *
  FROM user
  WHERE id < 60 -- The last item in the previous page
  ORDER BY id DESC
  LIMIT 10
```

Ovaj pristup se malo razlikuje od `OFFSET` zbog toga što morate znati vrednost na kojoj počinjete. Zamislite da ste na četvrtoj strani tabele, a znate prvu `id` na petoj. Umesto da prebrojavate sve zapise, možete ići direktno na taj `id` i vratiti sledećih deset stavki.

Ovo funkcioniše jer indeksi mogu efikasno podržati ovakav upit. Traženje indeksa B-stabla da vrati 10 id unosa pre određenog id zahteva samo učitavanje 10 unosa indeksa. Uporedite to sa upotrebom `OFFSET`, gde se moraju učitati svi unosi do pomaka, a zatim i navedena granica, što čini velike pomake veoma skupim.

Kada koristite paginaciju ključeva zajedno sa pravim indeksom, videćete značajno povećanje performansi. Kompleksnost vremena da nađe neki zapis u bazi podataka je konstantna. Na primer, traženje poslednje stranice velike tabele generisane gore traje samo 78 ms.

Ova metoda takođe štiti od oskudnih podataka. Ako je korisnik izbrisan i redosled više nije uzastopan u tabeli, to ne utiče na paginaciju skupova ključeva; bez problema će preskočiti nedostajuću vrednost.

[Sadržaj](#sadržaj)

#### Kompromisi paginacije skupova ključeva

Paginacija ključeva dolazi sa nekoliko kompromisa. Bez `OFFSET`-a, ne znate tačno koliko stranica ima u tabeli niti na kom se broju stranice trenutno nalazite. Osim toga, za paginaciju skupova ključeva potrebno je sortirati polje na vašem modelu. Sekvencijalni ID-ovi i polja datuma dobro funkcionišu.

[Sadržaj](#sadržaj)

### Korišćenje dodatka dj-pagination

Nažalost, nisam uspeo da pronađem biblioteku koja proširuje Djangov osnovni `Paginator` za dodavanje paginacije skupova ključeva. Admin tabela zahteva paginator tog tipa, pa ću iste generisane podatke demonstrirati u novom prikazu aplikacije pomoću dodatka `dj-pagination`.

Dodajte aplikaciju u svoj `INSTALLED_APPS` i middleware - dokumenti dobro objašnjavaju. Zatim kreirajte prikaz u aplikaciji Users:

`{project}/users/views.py`

```py
from django.shortcuts import render from .models import User

def index(request):
  context = {
    'users': User.objects.order_by('id').all()
  }
  return render(request, 'users/index.html', context)
```

Ovaj kod omogućava korisnicima da budu QuerySet dostupni u kontekstu prikaza i kaže mu da prikaže datoteku šablona. Zatim dodajte direktorijum šablona u svoj `TEMPLATES` objekat podešavanja i dodajte novu datoteku šablona:

`{project}/templates/users/index.html`

```html
{% load pagination_tags %}
{% autopaginate users %}
{% for user in users %}
{{ user.name }}
{% endfor %}
{% paginate %}
```

U stvarnoj aplikaciji, dodali biste dodatno označavanje i oblikovanje kako biste prikazali listu korisnika, ali to pokazuje oznake koje vam `dj-pagination` postaju dostupne.

Sada kada imate prikaz i šablon, napravite `urls.py` datoteku za usmeravanje zahteva do prikaza:

`{project}/users/urls.py`

```py
from django.urls import path 
from . import views

app_name = 'users' urlpatterns = [
  # ex: /users/
  path('', views.index, name='index'),
]
```

Na kraju, dodajte ga u svoje URL adrese:

```py
// various imports... 
urlpatterns = [
  // routes...
  path('users/', include('users.urls')),
]
```

Sada, kada usmerite pregledač na `/users stranicu`, videćete listu korisničkih imena sa jednostavnim kontrolama paginacije. Generisani upit vraća rezultate za manje od 100 ms, bez obzira na koju stranicu pređem.

Ako tražite način da imate veću kontrolu nad paginacijom skupova ključeva, `django-infinite-scroll-pagination` bi možda bilo vredno pogledati.

[Sadržaj](#sadržaj)

#### Zaključak paginacija

U ovom članku ste saznali kako funkcioniše paginacija u Djangu. Iako naivna paginacija dobro funkcioniše za male tabele, ova metoda brzo smanjuje performanse kako vaša tabela raste na milione redova. Štaviše, skok do zapisa duboko u tabeli biće veoma spor u upitu koji koristi `OFFSET`.

Dobra vest je da možete ubrzati stvari promenom `COUNT` upita. Pored toga, prelazak na paginaciju skupova ključeva poboljšaće performanse pretraživanja stranica i učiniće ih da rade u realnom vremenu. Django olakšava promenu podrazumevane konfiguracije, dajući vam moć da napravite efikasno rešenje za paginaciju u Djangu.

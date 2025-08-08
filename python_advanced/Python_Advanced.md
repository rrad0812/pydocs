
# Python tutorial - Advanced

## Property

### Uvod u properties

Sledeći kod definiše `Person` klasu koja ima dva atributa `name` i `age`, i kreira novu instancu klase `Person`:

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

john = Person('John', 18)
```

Pošto je `age` atribut instance klase `Person`, možete mu dodeliti novu vrednost ovako:

```py
john.age = 19
```

Sledeći zadatak je takođe tehnički validan:

```py
john.age = -1
```

Međutim, `age` je semantički netačan. Da biste bili sigurni da `age` nije nula ili negativna vrednost, koristite `if` naredbu za dodavanje provere na sledeći način:

```py
age = -1
if age <= 0:
    raise ValueError('The age must be positive')
else:
    john.age = age
```

I to morate da uradite svaki put kada želite da dodelite vrednost atributu `age`. Ovo je ponavljajuće i teško za održavanje. Da biste izbegli ovo ponavljanje, možete definisati par metoda koje se zovu `getter` i `setter`.

### Getter i seter

Metode `getter` i `setter` pružaju interfejs za pristup atributu instance:

- `Getter` vraća vrednost atributa
- `Setter` postavlja novu vrednost atributa

U našem primeru, možete učiniti `age` atribut privatnim (po konvenciji) i definisati metodu za dobijanje i postavljanje da biste manipulisali `age` atributom.

Sledeći kod prikazuje novu `Person` klasu sa metodama za dobijanje i postavljanje `age` atributa:

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self.set_age(age)

    def set_age(self, age):
        if age <= 0:
            raise ValueError('The age must be positive')
        self._age = age

    def get_age(self):
        return self._age
```

Kako ovo funkcioniše.

U `Person` klasi, `set_age()` je `seter`, a `get_age()` je `getter`. Po konvenciji, `getter` i `seter` imaju imena: `get_<attribute>()` i `set_<attribute>()`.

U `set_age()` metodi, podižemo `ValueError` ako je `age` manje ili jednako nuli. U suprotnom, dodeljujemo `age` argument atributu `_age`:

```py
def set_age(self, age):
    if age <= 0:
        raise ValueError('The age must be positive')
    self._age = age
```

Metoda `get_age()` vraća vrednost atributa `_age`:

```py
def get_age(self):
    return self._age
```

U `__init__()` metodi, pozivamo `setter` - `set_age()` metodu da bismo inicijalizovali `_age` atribut:

```py
def __init__(self, name, age):
    self.name = name
    self.set_age(age)
```

Sledeći pokušaji dodeljivanja nevažeće vrednosti atributu `age`:

```py
john = Person('John', 18)
john.set_age(-19)
```

I Pajton je izdao `ValueError` kao što se i očekivalo.

> `ValueError: The age must be positive`

Ovaj kod radi sasvim dobro. Ali ima problem sa kompatibilnošću unazad.

Pretpostavimo da ste objavili `Person` klasu pre nekog vremena i da je drugi programeri već koriste. A sada kada dodate `getter` i `seter`, sav kod koji koristi `Person` više neće raditi.

Da biste definisali `getter` i `setter` metode, a istovremeno postigli kompatibilnost sa prethodnim verzijama, možete koristiti `property()` klasu.

### Klasa property

Klasa `property` vraća `property` objekat. `property()` klasa ima sledeću sintaksu:

```py
property(fget=None, fset=None, fdel=None, doc=None)
```

`property()` ima sledeće parametre:

- `fget` je funkcija ili meoda za dobijanje vrednosti atributa.
- `fset` je funkcija ili metoda za postavljanje vrednosti atributa.
- `fdel` je funkcija za brisanje atributa.
- `doc` je dokumentacioni string, tj. komentar.

Sledeći primer koristi `property()` funkciju za definisanje `age` svojstva `Person` klase.

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def set_age(self, age):
        if age <= 0:
            raise ValueError('The age must be positive')
        self._age = age

    def get_age(self):
        return self._age

    age = property(fget=get_age, fset=set_age)
```

U `Person` klasi kreiramo novi objekat tipa `property` pozivanjem `property()` i dodeljujemo objekat svojstva atributa `age`. Imajte na umu da je `age` atribut klase, a ne atribut instance.

Sledeće pokazuje da `Person.age` je `property` objekat:

```py
print(Person.age)
```

Izlaz:

```py
<property object at 0x000001F5F5149180>
```

Sledeći kod kreira novu instancu klase `Personi` i pristupa `age` atributu:

```py
john = Person('John', 18)
```

`john.__dict__` čuva atribute instance objekta `john`. Sledeća kod prikazuje sadržaj `john.__dict__`:

```py
pprint(john.__dict__)
```

Izlaz:

```py
{'_age': 18, 'name': 'John'}
```

Kao što jasno možete videti iz izlaza, `john.__dict__` nema `age` atribut.

Sledeća komanda dodeljuje vrednost atributu `age` objekta `john`:

```py
john.age = 19
```

U ovom slučaju, Pajton prvo traži atribut `age` u `john.__dict__`. Pošto Pajton ne pronalazi `age` atribut u `john.__dict__`, onda će ga pronaći `age` u `Person.__dict__`.

`Person.__dict__` čuva atribute klase `Person`. Sledeći kod prikazuje sadržaj `Person.__dict__`:

```py
pprint(Person.__dict__)
```

Izlaz:

```py
mappingproxy({'__dict__': <attribute '__dict__' of 'Person' objects>,
              '__doc__': None,
              '__init__': <function Person.__init__ at 0x000002242F5B2670>,
              '__module__': '__main__',
              '__weakref__': <attribute '__weakref__' of 'Person' objects>,
              'age': <property object at 0x000002242EE39180>,
              'get_age': <function Person.get_age at 0x000002242F5B2790>,
              'set_age': <function Person.set_age at 0x000002242F5B2700>})
```

Pošto Pajton pronalazi `age` atribut u `Person.__dict__`, pozvaće `age` objekat svojstva.

Kada dodelite vrednost objektu `age`:

```py
john.age = 19
```

Pajton će pozvati funkciju dodeljenu argumentu `fset`, a to je `set_age()`.

Slično tome, kada čitate iz `age` objekta svojstva, Pajton će izvršiti funkciju dodeljenu argumentu `fget`, a to je `get_age()` metoda.

Korišćenjem `property()` klase, možemo dodati svojstvo klasi uz očuvanje kompatibilnosti sa verzijama unazad. U praksi, prvo ćete definisati atribute. Kasnije, možete dodati svojstvo klasi ako je potrebno.

### Spajanje svega zajedno

```py
from pprint import pprint

class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def set_age(self, age):
        if age <= 0:
            raise ValueError('The age must be positive')
        self._age = age

    def get_age(self):
        return self._age

    age = property(fget=get_age, fset=set_age)

print(Person.age)

john = Person('John', 18)
pprint(john.__dict__)

john.age = 19
pprint(Person.__dict__)
```

### Rezime uvoda u property klasu

Koristite Python `property()` klasu da definišete svojstvo za klasu.

### Dekorator @property

``Rezime``: u ovom tutorijalu ćete naučiti o dekoratoru svojstava ( @property ) u Pajtonu i, što je još važnije, kako on funkcioniše.

#### Uvod u dekorator @property

U prethodnom tutorijalu ste naučili kako da koristite klasu `property` da biste dodali svojstvo klasi. Evo sintakse klase property:

```py
class property(fget=None, fset=None, fdel=None, doc=None)
```

Sledeće definiše `Person` klasu sa dva atributa `name` i `age`:

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Da biste definisali metodu za dobijanje `age` atributa, koristite `property` klasu ovako:

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age

    def get_age(self):
        return self._age

    age = property(fget=get_age)
```

`property` prihvata metodu za dobijanje i vraća objekat svojstva.

Sledeća kod kreira instancu klase `Person` i dobija vrednost svojstva `age` preko instance:

```py
john = Person('John', 25)
print(john.age)
```

Izlaz:

```py
25
```

Takođe, možete direktno pozvati `get_age()` metod objekta `Person` ovako:

```py
print(john.get_age())
```

Dakle, da biste dobili vrednost `age` objekta `Person`, možete koristiti ili `age` svojstvo ili `get_age()` metod.

Ovo stvara nepotrebnu redundantnost. Da biste izbegli ovu redundantnost, možete preimenovati get_age() metodu u age() metodu ovako:

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age

    def age(self):
        return self._age

    age = property(fget=age)
```

`property()` prihvata `age` metodu i vraća `age` `property` objekat - što je dekorator. Stoga, možete koristiti `@property` dekorator za dekorisanje `age()` metode na sledeći način:

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age

    @property
    def age(self):
        return self._age
```

Dakle, korišćenjem `@property` dekoratora možete pojednostaviti definiciju svojstva klase.

#### Dekorateri setter-a

Sledeći kod dodaje `setter` metodu ( set_age ) da bi se dodelila vrednost `_age` atributu `Person` klase:

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age

    @property
    def age(self):
        return self._age

    def set_age(self, value):
        if value <= 0:
            raise ValueError('The age must be positive')
        self._age = value 
```

Da biste dodelili `set_age` objektu `fset` svojstva `age`, pozovite `setter` metod age objekta svojstva na sledeći način:

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age

    @property
    def age(self):
        return self._age

    def set_age(self, value):
        if value <= 0:
            raise ValueError('The age must be positive')
        self._age = value

    age = age.setter(set_age)
```

Metoda setter prihvata `set_age` funkciju i vraća drugu funkciju ( `property` objekat ). Stoga, dekorator za metodu property možete koristiti ovako: `@age.setter(set_age)`

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def set_age(self, value):
        if value <= 0:
            raise ValueError('The age must be positive')
        self._age = value
```

Sada možete promeniti `set_age()` metod u `age()` metod i koristiti `age` svojstvo u `__init__()` metodi:

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if value <= 0:
            raise ValueError('The age must be positive')
        self._age = value
```

Ukratko, možete koristiti dekoratore da biste kreirali svojstvo koristeći sledeći obrazac:

```py
class MyClass:
    def __init__(self, attr):
        self.prop = attr

    @property
    def prop(self):
        return self.__attr

    @prop.setter
    def prop(self, value):
        self.__attr = value
```

U ovom obrascu, `__attr` je privatni atribut, a `prop` je naziv svojstva.

Sledeći primer koristi `@property` dekoratore za kreiranje svojstava `name` and `age` u `Person` klasi:

```py
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if value <= 0:
            raise ValueError('The age must be positive')
        self._age = value

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        if value.strip() == '':
            raise ValueError('The name cannot be empty')
        self._name = value
```

#### Rezime @property dekoratora

- Koristite `@property` dekorator da biste kreirali svojstvo klase.

### Read-only property

``Rezime``: u ovom tutorijalu ćete naučiti kako da definišete svojstvo samo za čitanje u Pajtonu i kako da ga koristite za definisanje izračunatih svojstava.

#### Uvod u svojstvo samo za čitanje

Da biste definisali svojstvo samo za čitanje ( readonly ), potrebno je da kreirate svojstvo samo sa funkcijom za dobijanje ( getter ). Međutim, ono nije zaista samo za čitanje jer uvek možete pristupiti osnovnom atributu i promeniti ga.

Svojstva samo za čitanje su korisna u nekim slučajevima, kao što su izračunata svojstva.

Sledeći primer definiše klasu pod nazivom `Circle` koja ima `radius` atribut i `area()` metod:

```py
import math

class Circle:
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return math.pi ` self.radius `` 2
```

A sledeći kod kreira novi `Circle` eobjekat i vraća njegovu površinu:

```py
c = Circle(10)
print(c.area())
```

Izlaz:

```py
314.1592653589793
```

Evo kompletnog scenarija:

```py
import math

class Circle:
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return math.pi ` self.radius `` 2
    
c = Circle(10)
print(c.area())    
```

Izlaz:

```py
314.1592653589793
```

Ovaj kod radi savršeno dobro.

Ali bi bilo prirodnije da je površina svojstvo objekta `Circle`, a ne metoda. Da biste napravili `area()` metod kao svojstvo klase `Circle`, možete koristiti `@property` dekorator na sledeći način:

```py
import math

class Circle:
    def __init__(self, radius):
        self.radius = radius

    @property
    def area(self):
        return math.pi ` self.radius `` 2


c = Circle(10)
print(c.area)
```

Površina se izračunava iz radius atributa. Stoga se često naziva izračunato ili kompletirano svojstvo.

#### Keširanje izračunatih svojstava

Pretpostavimo da kreirate novi objekat kruga i pristupate svojstvu površine više puta. Svaki put, površina mora biti ponovo izračunata, što nije efikasno.

Da biste poboljšali efikasnost, potrebno je ponovo izračunati površinu kruga samo kada se poluprečnik promeni. Ako se poluprečnik ne menja, možete ponovo koristiti prethodno izračunatu površinu.

Da biste to uradili, možete koristiti tehniku keširanja:

- Prvo, izračunajte površinu i sačuvajte je u keš memoriji.
- Drugo, ako se radijus promeni, resetujte površinu. U suprotnom, vratite površinu direktno iz keša bez ponovnog izračunavanja.

Sledeće definiše novu `Circle` klasu sa keširanim `area` svojstvom:

```py
import math

class Circle:
    def __init__(self, radius):
        self._radius = radius
        self._area = None

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError('Radius must be positive')

        if value != self._radius:
            self._radius = value
            self._area = None

    @property
    def area(self):
        if self._area is None:
            self._area = math.pi ` self.radius `` 2

        return self._area
```

Kako to funkcioniše.

- Prvo, postavite `_area` na `None` u `__init__` metodi. Atribut `_area` je keš memorija koja čuva izračunatu površinu.

- Drugo, ako se radijus promeni (u setteru), resetujte _area na None.

- Treće, definišite `area` izračunato svojstvo. `area` svojstvo vraća `_area` ako nije `None`. U suprotnom, izračunajte površinu, sačuvajte je u `_area` i vratite je.

#### Rezime readonly property-ja

- Definišite samo `getter` metodu da bi svojstvo bilo samo za čitanje
- Koristite izračunato svojstvo da biste svojstvo klase učinili prirodnijim
- Koristite keširanje izračunatih svojstava da biste poboljšali performanse.

### Deletter property

``Rezime``: u ovom tutorijalu ćete naučiti kako da koristite klasu `property()` za brisanje svojstva objekta.

Da biste kreirali svojstvo klase, koristite dekorator `@property`. Ispod, `@property` dekorator koristi `property` klasu koja ima tri metode: `setter`, `getter` i `deleter`.

Korišćenjem metode za brisanje možete obrisati svojstvo objekta. Obratite pažnju da `deleter()` metoda briše svojstvo objekta, a ne klasu.

Sledeće definiše `Person` klasu sa `name` svojstvom:

```py
from pprint import pprint

class Person:
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        if value.strip() == '':
            raise ValueError('name cannot be empty')
        self._name = value

    @name.deleter
    def name(self):
        del self._name
```

U `Person` klasi koristimo `@name.deleter` dekorator. Unutar deletera koristimo `del` ključnu reč za brisanje `_name` atributa instance Person.

Sledeće prikazuje deo `__dict__` klase Person:

```py
pprint(Person.__dict__)
```

Izlaz:

```py
mappingproxy({'__dict__': <attribute '__dict__' of 'Person' objects>,
              '__doc__': None,
              '__init__': <function Person.__init__ at 0x000001DC41D62670>,
              '__module__': '__main__',
              '__weakref__': <attribute '__weakref__' of 'Person' objects>,
              'name': <property object at 0x000001DC41C89130>})
```

Klasa `Person.__dict__` ima `name` promenljivu. Sledeća instrukcija kreira novu instancu klase `Person`:

```py
person = Person('John')
```

Ima person.__dict__atribut _name:

```py
pprint(person.__dict__)
```

Izlaz:

```py
{'_name': 'John'}
```

Sledeća komanda koristi `del` ključnu reč za brisanje `name` svojstva:

```py
del person.name
```

Interno, Pajton će izvršiti `deleter` metodu koja briše `_name` atribut iz `person` objekta. `person.__dict__` biće prazno ovako:

```py
{}
```

A ako ponovo pokušate da pristupite `name` svojstvu, dobićete grešku:

```py
print(person.name)
```

```py
Error:
AttributeError: 'Person' object has no attribute '_name'
```

#### Rezime deletter property-ja

- Koristite dekorator `deleter` da biste obrisali svojstvo instance klase.

## Deskriptori

Pretpostavimo da imate klasu `Person` sa dva atributa instance `first_name` i `last_name`:

```py
class Person:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name
```

I želite da atributi `first_name` i `last_name` budu neprazni stringovi. Ovi obični atributi ne mogu to garantovati.

Da biste obezbedili validnost podataka, možete koristiti svojstvo sa `getter` i `setter` metodama, kao što je ovo:

```py
class Person:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name

    @property
    def first_name(self):
        return self._first_name

    @first_name.setter
    def first_name(self, value):
        if not isinstance(value, str):
            raise ValueError('The first name must a string')

        if len(value) == 0:
            raise ValueError('The first name cannot be empty')

        self._first_name = value

    @property
    def last_name(self):
        return self._last_name

    @last_name.setter
    def last_name(self, value):
        if not isinstance(value, str):
            raise ValueError('The last name must a string')

        if len(value) == 0:
            raise ValueError('The last name cannot be empty')

        self._last_name = value
```

U ovoj `Person` klasi, `getter` vraća vrednost atributa dok je `setter` validira pre nego što je dodeli atributu.

Ovaj kod radi savršeno dobro. Međutim, suvišan je jer logika validacije potvrđuje da li su ime i prezime validni na isti način.

Takođe, ako klasa ima više atributa koji zahtevaju string koji nije prazan, potrebno je da duplirate ovu logiku u drugim svojstvima. Drugim rečima, ova logika validacije se ne može ponovo koristiti.

Da biste izbegli dupliranje logike, možete imati metodu koja validira podatke i ponovo koristiti ovu metodu u drugim svojstvima. Ovaj pristup će omogućiti ponovnu upotrebu. Međutim, Pajton ima bolji način da ovo reši korišćenjem `deskriptora`.

Prvo, definišite klasu `deskriptora` koja implementira tri metode `__set_name__`, `__get__` i `__set__`:

```py
class RequiredString:
    def __set_name__(self, owner, name):
        self.property_name = name

    def __get__(self, instance, owner):
        if instance is None:
            return self

        return instance.__dict__[self.property_name] or None

    def __set__(self, instance, value):
        if not isinstance(value, str):
            raise ValueError(f'The {self.property_name} must be a string')

        if len(value) == 0:
            raise ValueError(f'The {self.property_name} cannot be empty')

        instance.__dict__[self.property_name] = value
```

Drugo, koristite `RequiredString` klasu u `Person` klasi:

```py
class Person:
    first_name = RequiredString()
    last_name = RequiredString()
```

Ako dodelite prazan string ili vrednost koja nije string atributu `first_name` or `last_name` klase `Person`, dobićete grešku.

Na primer, sledeći pokušaj dodeljivanja praznog stringa atributu `first_name`:

```py
try:
    person = Person()
    person.first_name = ''
except ValueError as e:
    print(e)
```

```py
Error:
The first_name must be a string
```

Takođe, klasu `RequiredString` možete koristiti u bilo kojoj klasi sa atributima koji zahtevaju vrednost stringa koja nije prazna.

Pored `RequiredString`, možete definisati i druge deskriptore da biste primenili druge tipove podataka kao što su `age`, `email` i `phone`. A ovo je samo jednostavna primena deskriptora.

Hajde da razumemo kako funkcionišu deskriptori.

### Protokol deskriptor

U Pajtonu, `protokol deskriptor` se sastoji od tri metode:

- `__get__` dobija vrednost atributa
- `__set__` postavlja vrednost atributa
- `__delete__` briše atribut

Opciono, deskriptor može imati `__set_name__` metodu koja postavlja atribut na instanci klase na novu vrednost.

### Šta je deskriptor

> Deskriptor je objekat klase koji implementira jednu od metoda navedenih u protokolu deskriptora.

Deskriptora ima dve vrste:

- Deskriptor podataka je objekat klase koji implementira metod `__set__` i/ili `_delete__` i
- Deskriptor koji nije podatak je objekat koji implementira samo metodu `__get__`.

Tip deskriptora određuje rezoluciju pretrage svojstva koju ćemo obraditi u sledećem tutorijalu.

### Kako funkcionišu deskriptori

Sledeće menja `RequiredString` klasu da bi uključilo `print` naredbe koje ispisuju argumente.

```py
class RequiredString:
    def __set_name__(self, owner, name):
        print(f'__set_name__ was called with owner={owner} and name={name}')
        self.property_name = name

    def __get__(self, instance, owner):
        print(f'__get__ was called with instance={instance} and owner={owner}')
        if instance is None:
            return self

        return instance.__dict__[self.property_name] or None

    def __set__(self, instance, value):
        print(f'__set__ was called with instance={instance} and value={value}')

        if not isinstance(value, str):
            raise ValueError(f'The {self.property_name} must a string')

        if len(value) == 0:
            raise ValueError(f'The {self.property_name} cannot be empty')

        instance.__dict__[self.property_name] = value

class Person:
    first_name = RequiredString()
    last_name = RequiredString()
```

Kada kompajlirate kod, videćete da Pajton kreira objekte deskriptora za `first_name` i `last_name` i automatski poziva `__set_name__` metodu ovih objekata. Evo rezultata:

```py
`__set_name__` was called with owner=<class '__main__.Person'> and name=first_name
`__set_name__` was called with owner=<class '__main__.Person'> and name=last_name
```

U ovom primeru, argument `owner`  je podešen na `Person` klasu u `__main__` modulu, a `name` argument je podešen na atribut `first_name` i `last_name` shodno tome.

To znači da Pajton automatski poziva `__set_name__` kada se kreira klasa koja je vlasnik `Person`. Sledeće izjave su ekvivalentne:

```py
first_name = RequiredString()
```

i

```py
first_name.__set_name__(Person, 'first_name')
```

Unutar `__set_name__` metode, dodeljujemo `name` argument atributu `property_name` instance objekta `descriptor` kako bismo mu kasnije mogli pristupiti u metodi `__get__` and `__set__`:

```py
self.property_name = name
```

I `first_name` i `last_name` su promenljive klase `Person`. Ako pogledate atribut klase `Person.__dict__`, videćete dva objekta deskriptora `first_name` i `last_name`:

```py
from pprint import pprint

pprint(Person.__dict__)
```

Izlaz:

```py
mappingproxy({'__dict__': <attribute '__dict__' of 'Person' objects>,
            '__doc__': None,
            '__module__': '__main__',
            '__weakref__': <attribute '__weakref__' of 'Person' objects>,
            'first_name': <__main__.RequiredString object at 0x0000019D6AB947F0>,
            'last_name': <__main__.RequiredString object at 0x0000019D6ACFBE80>})
```

Evo `__set__` metode klase `RequiredString`:

```py
def __set__(self, instance, value):
    print(f'__set__ was called with instance={instance} and value={value}')

    if not isinstance(value, str):
        raise ValueError(f'The {self.property_name} must be a string')

    if len(value) == 0:
        raise ValueError(f'The {self.property_name} cannot be empty')

    instance.__dict__[self.property_name] = value
```

Kada dodelite novu vrednost deskriptoru, Pajton poziva `__set__` metodu da postavi atribut na instanci klase owner na novu vrednost. Na primer:

```py
person = Person()
person.first_name = 'John'
```

Izlaz:

```py
`__set__` was called with `instance=<__main__.Person` object at 0x000001F85F7167F0> and value=John.
```

U ovom primeru, instance argument je `person` objekat, a vrednost je string 'John'. Unutar `__set__` metode, podižemo `ValueError` ako nova vrednost nije string ili ako je prazan string.

U suprotnom, dodeljujemo vrednost atributu instance `first_name` objekta `person`:

```py
instance.__dict__[self.property_name] = value
```

Imajte na umu da Pajton koristi `instance.__dict__` rečnik za čuvanje atributa instance objekta instance.

Kada podesite `first_name` i `last_name` instance objekta `Person`, videćete atribute instance sa istim imenima u `instance.__dict__`. Na primer:

```py
person = Person()
print(person.__dict__)  # {}

person.first_name = 'John'
person.last_name = 'Doe'

print(person.__dict__) # {'first_name': 'John', 'last_name': 'Doe'}
```

Izlaz:

```py
{}
{'first_name': 'John', 'last_name': 'Doe'}
```

Sledeće prikazuje `__get__` metodu klase `RequiredString`:

```py
def __get__(self, instance, owner):
    print(f'__get__ was called with instance={instance} and owner={owner}')
    if instance is None:
        return self

    return instance.__dict__[self.property_name] or None
```

Pajton poziva `__get__` metod `Person` objekta kada pristupite `first_name` atributu. Na primer:

```py
person = Person()

person.first_name = 'John'
print(person.first_name)
```

Izlaz:

```py
__set__ was called with instance=<__main__.Person object at 0x000001F85F7167F0> and value=John
__get__ was called with instance=<__main__.Person object at 0x000001F85F7167F0> and owner=<class '__main__.Person'>
```

Metod `__get__` vraća deskriptor ako je instance `None`. Na primer, ako pristupite `first_name` ili `last_name` iz `Person` klase, videćete objekat deskriptora:

```py
print(Person.first_name)
```

Izlaz:

```py
<__main__.RequiredString object at 0x000001AF1DA147F0>
```

Ako instanca nije None, `__get__` metod vraća vrednost atributa sa imenom `property_name` objekta instance.

### Rezime uvoda u deskriptore

Deskriptori su objekti klase koji implementiraju jednu od metoda u protokolu deskriptora, uključujući `__set__`, `__get__`,`__del__`.

### Deskriptori podataka Vs deskriptori koji nisu podaci

Deskriptora ima dve vrste:

- Deskriptori podataka su objekti klase koji implementiraju `__set__` metod (i/ili `__delete__` metod)
- Deskriptori koji nisu podaci su objekti klase koji imaju samo `__get__` metodu.

Oba tipa deskriptora mogu opciono implementirati `__set_name__` metod. `__set_name__` metod ne utiče na klasifikaciju deskriptora.

Tipovi deskriptora određuju kako Pajton rešava pretragu atributa objekta.

#### Deskriptor koji nije podatak

Ako klasa koristi deskriptor koji nije deskriptor podataka, Pajton će prvo pretražiti atribut u atributima instance ( `instance.__dict__` ). Ako Pajton ne pronađe atribut u atributima instance, koristiće deskriptor podataka.

Hajde da pogledamo sledeći primer.

- Prvo, definišite klasu deskriptora koja nije deskriptor podataka `FileCount` i koja ima `__get__` metodu koja vraća broj datoteka u direktorijumu:

    ```py
    class FileCount:
        def __get__(self, instance, owner):
            print('The __get__ was called')
            return len(os.listdir(instance.path))
    ```

- Drugo, definišite `Folder` klasu koja koristi `FileCount` deskriptor:

    ```py
    class Folder:
        count = FileCount()

        def __init__(self, path):
            self.path = path
    ```

- Treće, kreirajte instancu klase `Folder` i pristupite `count` atributu:

    ```py
    folder = Folder('/')
    print('file count: ', folder.count)
    ```

    Pajton je pozvao __get__deskriptor:

    ```py
    The __get__ was called
    file count:  32
    ```

- Nakon toga, podesite `count` atribut instance folder na 100 i pristupite atributu `count`:

    ```py
    folder.__dict__['count'] = 100
    print('file count: ', folder.count)
    ```

    Izlaz:

    ```py
    file count:  100
    ```

U ovom primeru, Pajton može pronaći `count` atribut u rečniku instance `__dict__`. Stoga, ne koristi deskriptore podataka.

#### Deskriptor podataka

Kada klasa ima deskriptor podataka, Pajton će prvo potražiti atribut instance u deskriptoru podataka. Ako Pajton ne pronađe atribut, potražiće ga u rečniku `instance.__dict__`. Na primer:

- Prvo, definišite `Coordinate` klasu deskriptora:

```py
class Coordinate:
    def __get__(self, instance, owner):
        print('The __get__ was called')

    def __set__(self, instance, value):
        print('The __set__ was called')
```

- Drugo, definišite `Point` klasu koja koristi `Coordinate` deskriptor:

    ```py
    class Point:
        x = Coordinate()
        y = Coordinate()
    ```

- Treće, kreirajte novu instancu klase `Point` i dodelite vrednost atributu `x` instance `p`:

    ```py
    p = Point()
    p.x = 10
    ```

    Izlaz:

    ```py
    The __set__ was called
    ```

    Pajton je pozvao `__set__` metod deskriptora x.

- Konačno, pristupite `x` atributu pinstance:

    ```py
    p.x
    ```

    Izlaz:

    ```py
    The `__get__` was called
    ```

    Pajton je pozvao `__get__` metod deskriptora x.

### Rezime tipova deskriptora

- Deskriptori podataka su objekti klase koji implementiraju `__set__` metod (i/ili `__delete__` metod)
- Ne-podatkovni deskriptori su objekti klase koji imaju samo `__get__` metodu.
- Prilikom pristupanja atributima objekta, deskriptori podataka poništavaju atribute instance, a atributi instance poništavaju deskriptore koji nisu podaci.

## Naznake tipova

### Uvod u naznake tipova

Neki programski jezici imaju statičko tipiziranje, kao što je C/C++. To znači da je potrebno unapred deklarisati tipove promenljivih, parametara i povratnih vrednosti funkcije. Unapred definisani tipovi omogućavaju kompajlerima da provere kod pre kompajliranja i pokretanja programa.

Pajton koristi dinamičko tipiziranje, u kojem promenljive, parametri i povratne vrednosti funkcije mogu biti bilo kog tipa. Takođe, tipovi promenljivih se mogu menjati dok se program izvršava. Generalno, dinamičko tpiziranje olakšava programiranje i uzrokuje neočekivane greške koje možete otkriti tek dok se program ne pokrene.

Pajtonove naznake tipova vam pružaju opciono statičko tipiziranje kako biste iskoristili najbolje od statičkog i dinamičkog tipiziranja. Sledeći primer definiše jednostavnu funkciju koja prihvata string i vraća drugi string:

```py
def say_hi(name):
    return f'Hi {name}'

greeting = say_hi('John')
print(greeting)
```

Evo sintakse za dodavanje naznaka tipa parametru i povratnoj vrednosti funkcije:

```py
parameter: type
-> type
```

Na primer, sledeće pokazuje kako se koriste naznake tipa za `name` parametar i povratnu vrednost funkcije `say_hi()`:

```py
def say_hi(name: str) -> str:
    return f'Hi {name}'

greeting = say_hi('John')
print(greeting)
```

Izlaz:

```py
Hi John
```

U ovoj novoj sintaksi, `name` parametar ima `str` naznaku tipa:

```py
name: str
```

I povratna vrednost funkcije `say_hi()` takođe ima naznaku tipa `str`:

```py
-> str
```

Pored `str` naznake tipa, možete koristiti i druge ugrađene tipove kao što su `int`, `float`, `bool`, i `bytes`za naznake tipova.

Važno je napomenuti da Pajton interpreter potpuno ignoriše naznake tipova. Ako funkciji `say_hi()` prosledite broj, program će se pokrenuti bez ikakvog upozorenja ili greške:

```py
def say_hi(name: str) -> str:
    return f'Hi {name}'

greeting = say_hi(123)
print(greeting)
```

Izlaz:

```py
Hi 123
```

Da biste proverili sintaksu za naznake tipova, potrebno je da koristite alatku za statičku proveru tipova.

### Korišćenje mypy alata za statičku proveru tipova

Pajton nema zvanični alat za statičku proveru tipova. Trenutno, najpopularniji alat treće strane je `Mypy`. Pošto je `Mypy` paket treće strane, potrebno ga je instalirati pomoću sledeće `pip` komande:

```shell
pip instal mypy
```

Jednom instaliran `mypy`, možete koristiti za proveru tipa pre pokretanja programa pomoću sledeće komande:

```shell
mypy app.py
```

Ovo će prikazati sledeću poruku:

```shell
app.py:5: error: Argument 1 to "say_hi" has incompatible type "int"; expected "str"
Found 1 error in 1 file (checked 1 source file)
```

Greška ukazuje da je argument `say_hi` `int` dok je očekivani tip `str`.

Ako vratite argument u string i `mypy` ponovo pokrenete, prikazaće se poruka o uspehu:

```shell
Success: no issues found in 1 source file
```

### Nagoveštavanje tipa i zaključivanje

Prilikom definisanja promenljive, možete dodati naznaku tipa kao što je ova:

```py
name: str = 'John'
```

Tip `name` promenljive je str. Ako promenljivoj dodelite vrednost koja nije string name, statička provera tipa će izdati grešku. Na primer:

```py
name: str = 'Hello'
name = 100
```

Greška:

```shell
app.py:2: error: Incompatible types in assignment (expression has type "int", variable has type "str")
Found 1 error in 1 file (checked 1 source file)
```

Dodavanje tipa promenljivoj nije potrebno jer statički proveravači tipova obično mogu da zaključe tip na osnovu vrednosti dodeljene promenljivoj.

U ovom primeru, vrednost imena je literalni string, tako da će statički proverivač tipa zaključiti tip promenljive imena kao `str`. Na primer:

```py
name = 'Hello'
name = 100
```

Izdaće istu grešku:

```shell
app.py:2: error: Incompatible types in assignment (expression has type "int", variable has type "str")
Found 1 error in 1 file (checked 1 source file)
```

### Dodavanje naznaka tipova za više tipova

Sledeća `add()` funkcija vraća zbir dva broja:

```py
def add(x, y):
    return x + y
```

Brojevi mogu biti celi ili brojevi sa pokretnim decimalom. Možete koristiti modul za podešavanje naznaka tipova `typing` za više tipova.

- Prvo, uvoz `Union` iz `typing` modula:

    ```py
    from typing import Union
    ```

- Drugo, koristite `Union` da biste kreirali tip unije koji uključuje `int` i `float`:

    ```py
    def add(x: Union[int, float], y: Union[int, float]) -> Union[int, float]:
        return x + y
    ```

Evo kompletnog izvornog koda:

```py
from typing import Union

def add(x: Union[int, float], y: Union[int, float]) -> Union[int, float]:
    return x + y
```

Počevši od Pajtona 3.10, možete koristiti sintaksu X | Y da biste kreirali tip unije, na primer:

```py
def add(x: int | float, y: int | float) -> int | float:
    return x + y
```

### Pseudonimi tipa

Pajton vam omogućava da dodelite alijas tipu i koristite taj alijas za naznake tipa. Na primer:

```py
from typing import Union

number = Union[int, float]

def add(x: number, y: number) -> number:
    return x + y
```

U ovom primeru, tipu `Union[int, float]` dodeljujemo  alias `Number` i koristimo `Number` alias u funkciji `add()`.

### Dodavanje naznake tipa za liste, rečnike i skup

Možete koristiti sledeće ugrađene tipove da biste postavili savete za tipove za listu , rečnik i skup:

- list
- dict
- set

Ako u promenljivu unesete naznake kao listu, ali joj kasnije dodelite rečnik, dobićete grešku:

```py
ratings: list = [1, 2, 3]
ratings = {1: 'Bad', 2: 'average', 3: 'Good'}
```

Greška:

```py
app.py:3: error: Incompatible types in assignment (expression has type "Dict[int, str]", variable has type "List[Any]")
Found 1 error in 1 file (checked 1 source file)
```

Da biste odredili tipove vrednosti u listi, rečniku i skupovima, možete koristiti alijase tipova iz modula `typing`:

Tip alijasa   | Ugrađeni tip
--------------|---------------
List          |  list
Tuple         |  tuple
Dict          |  dict
Set           |  set
Frozenset     |  fozenset
Sequence      |  Za listu, tuple i bilo koji drugi tip podataka sekvence.
Mapping       |  Za rečnik (dict), set, frozenset i bilo koji drugi tip podataka za mapiranje
ByteStrings   |  bytes, bytearray i memmoryview.

Na primer, sledeće definiše listu celih brojeva:

```py
from typing import List

ratings: List[int] = [1, 2, 3]
```

### Nijedan tip

Ako funkcija eksplicitno ne vraća vrednost, možete koristiti None da biste uneli naznaku vraćene vrednosti. Na primer:

```py
def log(message: str) -> None:
    print(message)
```

### Tipovi literala

Ako želite da dozvolite promenljivoj ili parametru funkcije da prihvati određenu listu literalnih vrednosti, možete koristiti tipove. Na primer:

```py
from typing import Literal

def set_mode(mode: Literal['read', 'write']) -> None:
    print(f"Setting mode to {mode}")

set_mode('write')
```

Kako funkcioniše:

- Prvo, uvezite `Literal` obrazac modula `typing`:

    ```py
    from typing import Literal
    ```

- Drugo, definišite `set_mode`funkciju koja prihvata parametar `mode` tipa `Literal`. `set_mode` može da prihvati samo jedan od dva string literal-a 'read' ili 'write':

    ```py
    mode: Literal['read', 'write']
    ```

- Treće, pozovite set_modefunkciju i prosledite 'write'argument:

    ```py
    set_mode('write')
    ```

Ako pokrenete mypy komandu, prikazaće se izlaz:

```shell
Success: no issues found in 1 source file
```

Ako promenite argument na vrednost koja nije 'read' ili 'write', `mypy` ddaje greške:

```py
set_mode('execute')
```

Greška:

```shell
main.py:8: error: Argument 1 to "set_mode" has incompatible type "Literal['execute']"; expected "Literal['read', 'write']"  [arg-type]
Found 1 error in 1 file (checked 1 source file)
```

### Konačne promenljive

Možete jednom dodeliti vrednost finalnoj promenljivoj. Ako joj ponovo dodelite vrednost, naići ćete na grešku. Finalne promenljive vam pomažu da sprečite slučajnu izmenu konstanti. Na primer:

```py
from typing import Final

HTTPS : Final[bool] = True
print(HTTPS )
```

Kako to funkcioniše.

- Prvo, uvezite Finaliz typingmodula:

    ```py
    from typing import Final
    ```

- Drugo, definišite konstantu `HTTPS` tipa `Final[bool]` i postavite njenu vrednost na `True`:

    ```py
    HTTPS : Final[bool] = True
    ```

    Pored toga bool, možete koristiti i druge ugrađene tipove kao što su `int`, `float`, `string`, itd., za čuvanje konačnih vrednosti različitih tipova.

- Treće, pokažite vrednost `HTTPS`:

    ```py
    print(HTTPS)
    ```

    Program `mypy` će označiti kod kao uspešan.

- Ako pokušate da promenite HTTPS u `False`, proveravač tipa ( mypy ) će označiti dodelu kao grešku.

    ```py
    from typing import Final

    HTTPS : Final[bool] = True
    HTTPS = False # error
    ```

    Greška:

    ```py
    main.py:4: error: Cannot assign to final name "HTTPS"  [misc]
    Found 1 error in 1 file (checked 1 source file)
    ```

### Rezime naznaka tipova

Koristite naznake tipova i alate za statičku proveru tipova da biste učinili svoj kod robusnijim.

## Funkcionalno programiranje u Pajtonu

### Funkcija map()

#### Uvod u funkciju map()

Kada radite sa listom ( ili tuple ), često je potrebno da transformišete elemente liste i vratite novu listu koja sadrži transformisane elemente.

Pretpostavimo da želite da udvostručite svaki broj u sledećoj `bonuses` listi:

```py
bonuses = [100, 200, 300]
```

Da biste to uradili, možete koristiti `for` petlju za iteraciju kroz elemente, udvostručiti svaki od njih i dodati ga na novu listu kao što je ova:

```py
bonuses = [100, 200, 300]

new_bonuses = []

for bonus in bonuses:
    new_bonuses.append(bonus * 2)

print(new_bonuses)
```

Izlaz:

```py
[200, 400, 600]
```

Pajton pruža bolji način za obavljanje ove vrste zadatka korišćenjem `map()` ugrađene funkcije.

Funkcija `map()` iterira kroz sve elemente u listi (ili torki), primenjuje funkciju na svaki i vraća iterator novih elemenata.

Sledeća kod prikazuje osnovnu sintaksu funkcije `map()`:

```py
iterator = map(fn, list)
```

U ovoj sintaksi, `fn` je naziv funkcije koja će se pozivati za svaki element liste.

U stvari, funkciji map() možete proslediti bilo koju iterabilnu stavku, ne samo listu ili torku.

Vraćajući se na prethodni primer, da biste koristili `map()` funkciju, prvo definišete funkciju koja udvostručuje bonus, a zatim koristite funkciju `map()` na sledeći način:

```py
def double(bonus):
    return bonus ` 2

bonuses = [100, 200, 300]

iterator = map(double, bonuses)
```

Ili možete učiniti ovaj kod sažetijim korišćenjem lambda izraza poput ovog:

```py
bonuses = [100, 200, 300]
iterator = map(lambda bonus: bonus * 2, bonuses)
```

Kada imate iterator, možete iterirati kroz nove elemente koristeći `for` petlju.
Ili možete konvertovati iterator u listu koristeći funkciju list():

```py
bonuses = [100, 200, 300]
iterator = map(lambda bonus: bonus * 2, bonuses)
print(list(iterator))
```

#### Korišćenje funkcije map() sa listom stringova

Sledeći primer koristi `map()` funkciju za vraćanje nove liste gde se svaki element transformiše u odgovarajući slučaj:

```py
names = ['david', 'peter', 'jenifer']
new_names = map(lambda name: name.capitalize(), names)
print(list(new_names))
```

Izlaz:

```py
['David', 'Peter', 'Jenifer']
```

#### Korišćenje funkcije map() sa listom torki

Pretpostavimo da imate sledeću korpu za kupovinu predstavljenu kao listu torki:

```py
carts = [['SmartPhone', 400],
         ['Tablet', 450],
         ['Laptop', 700]]
```

I potrebno je da izračunate iznos poreza za svaki proizvod sa porezom od 10%. Pored toga, potrebno je da dodate iznos poreza trećem elementu svake stavke na listi.

Lista povratka bi trebalo da bude otprilike ovakva:

```py
[['SmartPhone', 400, 40.0],
['Tablet', 450, 45.0],
['Laptop', 700, 70.0]]
```

Da biste to uradili, možete koristiti `map()` funkciju za kreiranje novog elementa liste i dodavanje novog iznosa poreza svakom ovako:

```py
carts = [['SmartPhone', 400],
         ['Tablet', 450],
         ['Laptop', 700]]

tax = 0.1
carts = map(lambda item: [item[0], item[1], item[2] * tax], carts)

print(list(carts))
```

Izlaz:

```py
[['SmartPhone', 400, 40.0], ['Tablet', 450, 45.0], ['Laptop', 700, 70.0]]
```

#### Rezime funkcije map()

Koristite Pajton `map()` funkciju da pozovete funkciju na svakoj stavki liste i vratite iterator.

### Funkcija filter()

#### Uvod u funkciju filter()

Ponekad je potrebno da iterativno pregledate elemente liste i izaberete neke od njih na osnovu određenih kriterijuma.

Pretpostavimo da imate sledeću listu `scores`:

```py
scores = [70, 60, 80, 90, 50]
```

Da biste dobili sve elemente sa `scores` liste gde je svaki element veći ili jednak 70, koristite sledeći kod:

```py
scores = [70, 60, 80, 90, 50]

filtered = []

for score in scores:
    if score >= 70:
        filtered.append(score)

print(filtered)
```

Izlaz:

```py
[70, 80, 90]
```

Kako to funkcioniše.

- Prvo, definišite praznu listu ( `filtered` ) koja će sadržati elemente iz `scores` liste.
- Drugo, iterativno prođite kroz elemente liste `scores`. Ako je element veći ili jednak 70, dodajte ga na `filtered` listu.
- Treće, prikažite `filtered` listu na ekranu.

Pajton ima ugrađenu funkciju `filter()` koja vam omogućava da filtrirate listu (ili torku) na lepši način.

Sledeći kod prikazuje sintaksu funkcije `filter()`:

```py
filter(fn, list)
```

Funkcija `filter()` iterira kroz elemente liste i primenjuje `fn()` funkciju na svaki element. Vraća iterator za elemente gde `fn()` vraća `True`.

U stvari, drugom argumentu funkcije `filter()` možete proslediti bilo koju iterabilnu stavku, ne samo listu.

Sledeće je prikazano kako se `filter()` funkcija koristi za filtriranje liste `scores` gde je svaki rezultat veći ili jednak 70:

```py
scores = [70, 60, 80, 90, 50]
filtered = filter(lambda score: score >= 70, scores)

print(list(filtered))
```

Izlaz:

```py
[70, 80, 90]
```

Pošto `filter()` funkcija vraća iterator, možete koristiti `for` petlju da biste ga uradili iteraciju. Ili možete koristiti `list()` funkciju da biste iterator pretvorili u listu.

Primer korišćenja funkcije `filter()` u Pajtonu za filtriranje liste torki

Pretpostavimo da imate sledeću listu torki:

```py
countries = [
    ['China', 1394015977],
    ['United States', 329877505],
    ['India', 1326093247],
    ['Indonesia', 267026366],
    ['Bangladesh', 162650853],
    ['Pakistan', 233500636],
    ['Nigeria', 214028302],
    ['Brazil', 21171597],
    ['Russia', 141722205],
    ['Mexico', 128649565]
]
```

Svaki element u listi je torka koja sadrži ime države i broj stanovnika.

Da biste dobili sve zemlje čija je populacija veća od 300 miliona, možete koristiti `filter()` funkciju na sledeći način:

```py
countries = [
    ['China', 1394015977],
    ['United States', 329877505],
    ['India', 1326093247],
    ['Indonesia', 267026366],
    ['Bangladesh', 162650853],
    ['Pakistan', 233500636],
    ['Nigeria', 214028302],
    ['Brazil', 21171597],
    ['Russia', 141722205],
    ['Mexico', 128649565]
]

populated = filter(lambda c: c[1] > 300000000, countries)

print(list(populated))
```

Izlaz:

```py
[['China', 1394015977], ['India', 1326093247], ['United States', 329877505]]
```

#### Rezime funkcije filter()

Koristite Pajton `filter()` funkciju za filtriranje liste (ili torke).

### Funkcija reduce()

Ponekad želite da svedete listu na jednu vrednost. Na primer, pretpostavimo da imate listu brojeva :

```py
scores = [75, 65, 80, 95, 50]
```

A da biste izračunali zbir svih elemenata na `scores` listi, možete koristiti `for` petlju poput ove:

```py
scores = [75, 65, 80, 95, 50]

total = 0

for score in scores:
    total += score

print(total)
```

Izlaz:

```py
365
```

U ovom primeru, sveli smo celu listu na jednu vrednost, koja je zbir svih elemenata sa liste.

#### Uvod u funkciju reduce() u Pajtonu

Pajton nudi funkciju `reduce()` koja vam omogućava da skratite listu na sažetiji način.

Evo sintakse funkcije `reduce()`:

```py
reduce(fn,list)
```

`Funkcija` reduce()` primenjuje `fn` funkciju sa dva argumenta kumulativno na stavke liste, s leva na desno, da bi svela listu na jednu vrednost.

Za razliku od funkcija `map()` i `filter()`, `reduce()` nije ugrađena funkcija u Pajtonu. U stvari, `reduce()`funkcija pripada `functools` modulu.

Da biste koristili `reduce()` funkciju, potrebno je da je uvezete iz `functools` modula koristeći sledeću naredbu na vrhu datoteke:

```py
from functools import reduce
```

Sledeće ilustruje kako se `reduce()` funkcija koristi za izračunavanje zbira elemenata liste scores:

```py
from functools import reduce

def sum(a, b):
    print(f"a={a}, b={b}, {a} + {b} ={a+b}")
    return a + b


scores = [75, 65, 80, 95, 50]
total = reduce(sum, scores)
print(total)
```

Izlaz:

```py
a=75, b=65, 75 + 65 = 140
a=140, b=80, 140 + 80 = 220
a=220, b=95, 220 + 95 = 315
a=315, b=50, 315 + 50 = 365
365
```

Kao što se jasno vidi iz izlaza, `reduce()` funkcija kumulativno dodaje dva elementa liste s leva na desno i smanjuje celu listu na jednu vrednost.

Da biste kod učinili sažetijim, možete koristiti `lambda izraz` umesto definisanja `sum()` funkcije:

```py
from functools import reduce

scores = [75, 65, 80, 95, 50]
total = reduce(lambda a, b: a + b, scores)

print(total)
```

#### Rezime funkcije reduce()

Koristite Pajton `reduce()` funkciju da biste smanjili listu na jednu vrednost.

## Zatvaranja

### Uvod u zatvaranja

U Pajtonu možete definisati funkciju unutra druge funkcije. Ova funkcija se naziva ugnježđena funkcija. Na primer:

```py
def say():
    greeting = 'Hello'

    def display():
        print(greeting)

    display()
```

U ovom primeru, definišemo `display` funkciju unutar `say` funkcije. `display` funkcija se naziva ugnežđena funkcija.

Unutar `display` funkcije, pristupate `greeting` promenljivoj iz njenog `nelokalnog` opsega važenja.

Pajton naziva `greeting` promenljivu slobodnom promenljivom.

Kada pogledate `display` funkciju, zapravo gledate na:

- Sama funkcija display.
- I slobodna promenljiva `greeting` sa vrednošću 'Hello'.

Dakle, kombinacija funkcije `display` i `greeting` promenljive naziva se `zatvaranje` ( ili `closure` ):

``Po definiciji``, `zatvaranje` je ugnežđena funkcija koja referencira jednu ili više promenljivih iz svog obuhvatajućeg opsega važenja.

### Vraćanje unutrašnje funkcije

U Pajtonu, funkcija može vratiti vrednost koja je druga funkcija. Na primer:

```py
def say():
    greeting = 'Hello'

    def display():
        print(greeting)

    return display    
```

U ovom primeru, `say` funkcija vraća `display` funkciju umesto da je izvršava.
Kada `say` funkcija vrati `display` funkciju, ona zapravo vraća `zatvaranje`:

Sledeća komanda dodeljuje povratnu vrednost funkcije `say` promenljivoj `fn`. Pošto je `fn` funkcija, možete je izvršiti:

```py
fn = say()
fn()
```

Izlaz

```py
Hello
```

Funkcija `say` se izvršava i vraća funkciju `fn`. Kada se `fn` funkcija izvršava, funkcija `say` je već završena. Drugim rečima, opseg funkcije `say` je nestao u trenutku izvršavanja `fn` funkcije.

Pošto `greeting` promenljiva pripada opsegu važenja funkcije `say`, ona bi takođe trebalo da bude uništena sa opsegom važenja funkcije `say`.

Međutim, vidite da `fn` i dalje prikazuje vrednost promenljive `greeting`.

### Pajton ćelije i promenljive sa višestrukim domenima

Vrednost promenljive `greeting` se deli između dva opsega važenja:

- Funkcija say,
- Zatvaranja fn.

Oznake `greeting` su u dva različita opsega. Međutim, one uvek referenciraju isti string objekat sa vrednošću 'Hello'.

Da bi se ovo postiglo, Pajton kreira posrednički objekat koji se zove `cell`:

Da biste pronašli memorijsku adresu objekta ćelije, možete koristiti `__closure__` svojstvo na sledeći način:

```py
print(fn.__closure__)
```

Izlaz:

```py
(<cell at 0x0000017184915C40: str object at 0x0000017186A829B0>,)
```

Vraća `__closure__` torku ćelije.

U ovom primeru, memorijska adresa ćelije je 0x0000017184915C40. Ona referencira string objekat na 0x0000017186A829B0.

Ako prikažete memorijsku adresu objekta tipa string u `say` funkciji i `closure`, trebalo bi da vidite da se odnose na isti objekat u memoriji:

```py
def say():
    greeting = 'Hello'
    print(hex(id(greeting)))

    def display():
        print(hex(id(greeting)))
        print(greeting)

    return display

fn = say()
fn()
```

Izlaz:

```py
0x17186a829b0
0x17186a829b0
```

Kada pristupite vrednosti promenljive `greeting`, Pajton će tehnički "duplo skočiti" da bi dobio vrednost stringa.

Ovo objašnjava zašto kada `say()` funkcija je bila van opsega, i dalje možete pristupiti objektu stringa na koji se poziva `greeting` promenljiva.

Na osnovu ovog mehanizma, možete zamisliti `zatvaranje` kao funkciju i prošireni opseg koji sadrži slobodne promenljive.

Da biste pronašli slobodne promenljive koje zatvaranje sadrži, možete koristiti `__code__.co_freevars`, na primer:

```py
def say():

    greeting = 'Hello'

    def display():
        print(greeting)

    return display

fn = say()
print(fn.__code__.co_freevars)
```

Izlaz:

```py
('greeting',)
```

U ovom primeru, `fn.__code__.co_freevars` vraća `greeting` slobodnu promenljivu zatvaranja `fn`.

### Kada Pajton kreira zatvaranje

Pajton kreira novi opseg važenja kada se funkcija izvrši. Ako ta funkcija kreira zatvaranje, Pajton takođe kreira novo zatvaranje. Razmotrite sledeći primer:

- Prvo, definišite funkciju `multiplier` koja vraća zatvaranje:

    ```py
    def multiplier(x):
        def multiply(y):
            return x * y
        return multiply
    ```

    Funkcija `multiplier` vraća `multiply` dva argumenta. Međutim, umesto toga koristi zatvaranje.

- Drugo, pozovite multiplier funkciju tri puta:

    ```py
    m1 = multiplier(1)
    m2 = multiplier(2)
    m3 = multiplier(3)
    ```

    Ovi pozivi funkcija kreiraju tri zatvaranja. Svaka funkcija množi broj sa 1, 2 i 3.

- Treće, izvršite funkcije zatvaranja:

    ```py
    print(m1(10))
    print(m2(10))
    print(m3(10))
    ```

    Linije m1, m2 i m3 imaju različite slučajeve zatvaranja.

Spojite sve zajedno:

```py
def multiplier(x):
    def multiply(y):
        return x * y
    return multiply

m1 = multiplier(1)
m2 = multiplier(2)
m3 = multiplier(3)

print(m1(10))
print(m2(10))
print(m3(10))
```

Izlaz:

```py
10
20
30
```

### Zatvaranja i petlje

Pretpostavimo da želite da kreirate sva tri gore navedena zatvaranja odjednom i možda ćete smisliti sledeće:

```py
multipliers = []
for x in range(1, 4):
    multipliers.append(lambda y: x * y)

m1, m2, m3 = multipliers

print(m1(10))
print(m2(10))
print(m3(10))
```

Kako ovo funkcioniše:

- Prvo, deklarišite listu `multipliers` koja će čuvati zatvaranja.
- Drugo, koristite `lambda izraz` da biste kreirali zatvaranje i dodali zatvaranje listi `multipliers` u svakoj iteraciji.
- Treće, raspakujte zatvaranja sa liste u promenljive `m1`, `m2` i `m3`.
- Konačno, prosledite vrednost 10, svakom zatvaranju i izvršite ga.

Sledeće prikazuje izlaz:

```py
30
30
30
```

Ovo ne funkcioniše kako si očekivalo. Ali zašto?

`x` počinje od 1 i ide do 3 u for petlji. Nakon petlje, njegova vrednost je 3.
Svaki element liste je sledeći rezultata:

```py
lambda y: x * y
```

Pajton izvršava x kada pozovete `m1(10)`, `m2(10)` i `m3(10)`. U trenutku izvršavanja zatvaranja, `x` je 3.

Zato vidite isti rezultat kada pozovete m1(10), m2(10), i m3(10).

Da biste ovo popravili, potrebno je da naložite Pajtonu da izvrši izvršenje `x` u petlji:

```py
def multiplier(x):
    def multiply(y):
        return x ` y
    return multiply

multipliers = []
for x in range(1, 4):
    multipliers.append(multiplier(x))

m1, m2, m3 = multipliers

print(m1(10))
print(m2(10))
print(m3(10))
```

### Rezime zatvaranja

Zatvaranje je funkcija i prošireni opseg koji sadrže slobodne promenljive.

## Izuzeci

### Uvod u izuzetke

U Pajtonu, izuzeci su objekti klasa izuzetaka. Sve klase izuzetaka su podklase  `BaseException` klase.

Međutim, skoro sve ugrađene klase izuzetaka nasleđuju od `Exception` klase , koja je podklasa klase `BaseException`:

Sledeći primer definiše listu od tri elementa i pokušava da pristupi četvrtom:

```py
colors = ['red', 'green', 'blue']

print(colors[3])
```

Nevažeći indeks je izazvao `IndexError` izuzetak kako se i očekivalo:

```py
IndexError: list index out of range
```

Kada se desi izuzetak, Pajton zaustavlja program osim ako ga ne obradite. Da biste obradili izuzetak, koristite `try...except` naredbu. Na primer:

```py
colors = ['red', 'green', 'blue']

try:
    print(colors[3])
except IndexError as e:
    print(e)

print('Continue to run')
```

Izlaz:

```shell
<class 'IndexError'> - list index out of range
Continue to run
```

U ovom primeru, koristimo `try...except` naredbu za obradu `IndexError` izuzetka. Kao što možete videti iz izlaza, program nastavlja da se izvršava nakon `try...except` naredbe.

Klasa `IndexError` nasleđuje od `LookupError` klase koja nasleđuje od `Exception` klase:

I možete uhvatiti bilo koju od klasa `LookupError` ili `ExceptionIndexError` kada se dogodi izuzetak. Na primer:

```py
colors = ['red', 'green', 'blue']

try:
    print(colors[3])
except LookupError as e:
    print(e.__class__, '-', e)

print('Continue to run')
```

Izlaz:

```shell
<class 'IndexError'> - list index out of range
Continue to run
```

U ovom primeru, izuzetak je i dalje prisutni `IndexError` iako ga hvatamo sa `LookupError`. Stoga, kada obrađujete izuzetak, program za obradu izuzetaka će hvatati tip izuzetka koji navedete i bilo koji od njegovih podklasa.

Program se pokreće isto ako koristite `Exception` klasu umesto `LookupError` klase:

```py
colors = ['red', 'green', 'blue']

try:
    print(colors[3])
except Exception as e:
    print(e.__class__, '-', e)

print('Continue to run')
```

Izlaz:

```shell
<class 'IndexError'> - list index out of range
Continue to run
```

U praksi, trebalo bi da beležite izuzetke što je preciznije moguće kako biste znali kako da se nosite sa svakim izuzetkom na specifičan način.

#### Primer rada sa izuzecima

Sledeći primer definiše `division` funkciju koja vraća rezultat deljenja a sa b:

```py
def division(a, b):
    return a / b

c = division(10, 0)
```

Izlaz

```shell
ZeroDivisionError: division by zero
```

U ovom primeru, ako je b nula, doći će do `ZeroDivisionError` izuzetka. Da biste obradili `ZeroDivisionError` izuzetak, koristite `try...except` naredbu:

```py
def division(a, b):
    try:
        return {
            'success': True,
            'message': 'OK',
            'result': a / b
        }
    except ZeroDivisionError as e:
        return {
            'success': False,
            'message': 'b cannot be zero',
            'result': None
        }


result = division(10, 0)
print(result)
```

Izlaz:

```shell
{'success': False, 'message': 'b cannot be zero', 'result': None}
```

U ovom primeru, funkcija vraća rečnik koji ima tri elementa:

- `success` je bulova vrednost koja pokazuje da li je operacija uspešna ili ne.
- `message` označava poruku o grešci ili uspehu.
- `result` čuva rezultat od a / b ili `None` ako je b nula.

Sledeće prikazuje izlaz ako se `ZeroDivisionError` dogodi:

```shell
{'success': False, 'message': 'b cannot be zero', 'result': None}
```

Sada, ako ne uhvatite `ZeroDivisionError` izuzetak već opštiji izuzetak poput `Exception` klase:

```py
def division(a, b):
    try:
        return {
            'success': True,
            'message': 'OK',
            'result': a / b
        }
    except Exception as e:
        return {
            'success': False,
            'message': 'b cannot be zero',
            'result': None
        }


result = division(10, 0)
print(result)
```

Izlaz:

```shell
{'success': False, 'message': 'b cannot be zero', 'result': None}
```

Program radi kao i pre jer `try...except` takođe hvata tip izuzetka koji je podklasa klase `Exception`.

Međutim, ako funkciji `division()` prosledite dva stringa umesto dva broja, dobićete istu poruku kao da se `ZeroDivisionError` izuzetak dogodio:

```py
def division(a, b):
    try:
        return {
            'success': True,
            'message': 'OK',
            'result': a / b
        }
    except Exception as e:
        return {
            'success': False,
            'message': 'b cannot be zero',
            'result': None
        }


result = division('10', '2')
print(result)
```

Izlaz:

```shell
{'success': False, 'message': 'b cannot be zero', 'result': None}
```

U ovom primeru, izuzetak nije `ZeroDivisionError` već `TypeError`. Međutim, kod ga i dalje obrađuje kao `ZeroDivisionError` izuzetak.

Stoga bi uvek trebalo da obrađujete izuzetke od najspecifičnijeg do najmanje specifičnog. Na primer:

```py
def division(a, b):
    try:
        return {
            'success': True,
            'message': 'OK',
            'result': a / b
        }
    except TypeError as e:
        return {
            'success': False,
            'message': 'Both a & b must be numbers',
            'result': None
        }
    except ZeroDivisionError as e:
        return {
            'success': False,
            'message': 'b cannot be zero',
            'result': None
        }
    except Exception as e:
        return {
            'success': False,
            'message': str(e),
            'result': None
        }


result = division('10', '2')
print(result)
```

Izlaz:

```shell
{'success': False, 'message': 'Both a & b must be numbers', 'result': None}
```

U ovom primeru, hvatamo `TypeError`, `ZeroDivisionError`, i `Exception` redosledom kojim se pojavljuju u `try...except` izjavi.

Ako je kod koji obrađuje različite izuzetke isti, možete grupisati sve izuzetke u jedan na sledeći način:

```py
def division(a, b):
    try:
        return {
            'success': True,
            'message': 'OK',
            'result': a / b
        }
    except (TypeError, ZeroDivisionError, Exception) as e:
        return {
            'success': False,
            'message': str(e),
            'result': None
        }


result = division(10, 0)
print(result)
```

Izlaz:

```shell
{'success': False, 'message': 'division by zero', 'result': None}
```

#### Rezime uvoda u izuzetke

- Pajton izuzeci su objekti klasa, koje su podklase klase BaseException.
- Obradi izuzetak od najspecifičnijeg do najmanje specifičnog.

### Obrada izuzetaka

#### Uvod u obradu izuzetaka

Za obradu izuzetaka koristite `try` naredbu. `try` naredba ima sledeće klauzule:

```py
try:
    # code that you want to protect from exceptions
except <ExceptionType> as ex:
    # code that handle the exception
finally:
    # code that always execute whether the exception occurred or not
else:
    # code that excutes if try execute normally (an except clause must be present)
```

Hajde da detaljnije ispitamo try naredbu.

U try klauzuli smeštate kod koji štitite od jednog ili više potencijalnih izuzetaka. Dobra je praksa da kod bude što kraći. Često ćete imati jednu izjavu u `try` klauzuli.

Klauzula `try` se pojavljuje tačno jednom u `try` izjavi.

U `except` klauzuli se postavlja kod koji obrađuje određeni tip izuzetka. `try` naredba može imati nula ili više `except` klauzula. Tipično, svaka `except` klauzula obrađuje različite tipove izuzetaka na specifične načine.

U `except` klauzuli, `as ex` je opciono. I `<ExceptionType>` je takođe opciono. Međutim, ako izostavite `<ExceptionType> as ex`, imaćete golu obrađivačku jedinicu izuzetaka.

Prilikom navođenja tipova izuzetaka u `except` klauzulama, postavljate najspecifičnije do najmanje specifičnih izuzetaka od vrha nadole.

Ako imate istu logiku koja obrađuje različite tipove izuzetaka, možete ih grupisati u jednu `except` klauzulu. Na primer:

```py
try:
...
except <ExceptionType1> as ex:
    log(ex)
except <ExceptionType2> as ex:
    log(ex)
```

```py
try:
...
except (<ExceptionType1>, <ExceptionType2>) as ex:
    log(ex)
```

Važno je napomenuti da `except` redosled je važan jer će Pajton pokrenuti prvu `except` klauzulu čiji tip izuzetka odgovara nastalom izuzetku.

Klauzula `finally` se može pojaviti nula ili jednom u `try` naredbi. `finally` klauzula se uvek izvršava bez obzira da li se dogodio izuzetak ili ne.

Klauzula `else` se takođe pojavljuje nula ili jednom. I `else` klauzula je validna samo ako naredba try ima bar jednu `except` klauzulu.

Obično se postavlja kod koji se izvršava ako se `try` klauzula normalno završi.

#### Primer obrade izuzetaka

Sledeća definicija definiše funkciju koja vraća rezultat broja pomoću drugog broja:

```py
def divide(a, b):
    return a / b
```

Ako drugom argumentu odamo 0, dobićemo ZeroDivisionError izuzetak:

```py
divide(10, 0)
```

Greška:

```py
ZeroDivisionError: division by zero
```

Da biste to popravili, možete obraditi `ZeroDivisionError` izuzetak u `divide()` funkciji na sledeći način:

```py
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError as ex:
        return None
```

U ovom primeru, `divide()` funkcija vraća vrednost `None` ako se `ZeroDivisionError` desi:

```py
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError as ex:
        return None
```

Kada koristite `divide()` funkciju, potrebno je da proverite da li je rezultat None:

```py
result = divide(10, 0)

if result is not None:
    print('result:', result)
else:
    print('Invalid inputs')
```

Ali vraćanje `None` možda nije najbolje jer drugi mogu slučajno proceniti rezultat u `if` izjavi ovako:

```py
result = divide(10, 0)

if result:
    print('result:', result)
else:
    print('Invalid inputs')
```

U ovom slučaju, radi. Međutim, neće raditi ako je prvi argument nula. Na primer:

result = divide(0, 10)

```py
if result:
    print('result:', result)
else:
    print('Invalid inputs')
```

Bolji pristup je da se pozivaocu pokrene izuzetak ako se `ZeroDivisionError` izuzetak dogodio. Na primer:

```py
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError as ex:
        raise ValueError('The second argument (b) must not be zero')
```

U ovom primeru, divide() funkcija će izdati grešku ako je `b` nula. Da biste koristili `divide()` funkciju, potrebno je da uhvatite `ValueError` izuzetak:

```py
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError as ex:
        raise ValueError('The second argument (b) must not be zero')

    try:
        result = divide(10, 0)
    except ValueError as e:
        print(e)
    else:
        print('result:', result)
```

Izlaz:

```shell
The second argument (b) must not be zero
```

Dobra je praksa da se u posebnim slučajevima pokrene izuzetak umesto vraćanja `None`.

#### Primer

Kada uhvatite izuzetak u klauzuli except, potrebno je da postavite izuzetke od najspecifičnijeg do najmanje specifičnog u smislu hijerarhije izuzetaka.

Sledeće prikazuje tri klase izuzetaka: `Exception`, `LookupError` i `IndexError`:

Ako uhvatite izuzetak, potrebno je da ih postavite sledećim redosledom: `IndexError`, `LookupErorr` i `Exception`.

Na primer, sledeći kod definiše listu od tri niza znakova i pokušava da pristupi četvrtom elementu:

```py
colors = ['red', 'green', 'blue']
try:
    print(colors[3])
except IndexError as e:
    print(type(e), 'Index error')
except LookupError as e:
    print(type(e), 'Lookup error')
```

Izdaje se sledeća greška:

```py
<class 'IndexError'> Index error
```

Pristup `colors[3]` izaziva `IndexError` izuzetak. Međutim, ako zamenite redosled except klauzule i uhvatite LookupError prvu i IndexErrordrugu drugu, ovako:

```py
colors = ['red', 'green', 'blue']
try:
    print(colors[3])
except LookupError as e:
    print(type(e), 'Lookup error')
except IndexError as e:
    print(type(e), 'Index error')
```

Izlaz:

```shell
<class 'IndexError'> Lookup error
```

Izuzetak je i dalje, `IndexErroral` i poruka je obmanjujuća.

#### Rukovaoci izuzecima

Kada želite da uhvatite bilo koji izuzetak, možete koristiti obrađivače izuzetaka. Obradač izuzetaka ne određuje tip izuzetka:

```py
try:
    ...
except:
    ...
```

To je ekvivalentno sledećem:

```py
try:
    ...
except BaseException:
    ...
```

Obrada izuzetaka bez dodatnih funkcija će hvatati sve izuzetke, uključujući izuzetke `SystemExit` i `KeyboardInterupt`.

Samo jedan izuzetak će otežati prekidanje programa pomoću Control-C i prikrivanje drugih programa.

Ako želite da uhvatite sve izuzetke koji signaliziraju greške u programu, možete umesto toga koristiti `except Exception`:

```py
try:
    ...
except Exception:
    ...
```

U praksi, trebalo bi da izbegavate korišćenje obrađivača izuzetaka. Ako ne znate izuzetke koje treba da hvatate, samo pustite da se izuzetak dogodi, a zatim izmenite kod da bi obrađivao te izuzetke.

Da biste dobili informacije o izuzetku iz samog obrađivača izuzetaka, koristite `exc_info()` funkciju iz `sys` modula.

Funkcija `sys.exc_info()` vraća torku koja se sastoji od tri vrednosti:

- `type` je tip izuzetka koji se dogodio. To je podklasa od BaseException.
- `value` je instanca tipa izuzetka.
- `traceback` je objekat koji inkapsulira stek poziva na mestu gde se izuzetak prvobitno dogodio.

Sledeći primer koristi `sys.exc_info()` funkciju za ispitivanje izuzetka kada je string podeljen brojem:

```py
import sys

try:
    '20' / 2
except:
    exc_info = sys.exc_info()
    print(exc_info)
```

Izlaz:

```shell
(<class 'TypeError'>, TypeError("unsupported operand type(s) for /: 'str' and 'int'"), <traceback object at 0x000001F19F42E700>)
```

Izlaz pokazuje da kod u klauzuli `try` izaziva `TypeError` izuzetak. Stoga možete izmeniti kod da bi ga posebno obrađivao na sledeći način:

```py
try:
    '20' / 2
except TypeError as e:
    print(e)
```

Izlaz:

```shell
unsupported operand type(s) for /: 'str' and 'int'
```

#### Rezime obrade izuzetaka

- Koristite `try` izjavu za obradu izuzetka.
- U klauzulu `try` stavite samo minimalni kod koji želite da zaštitite od potencijalnih izuzetaka.
- Obrađujte izuzetke od najspecifičnijeg do najmanje specifičnog u smislu tipova izuzetaka. Redosled `except` klauzula je važan.
- Funkcija `finally` se uvek izvršava bez obzira da li su se izuzeci dogodili ili ne.
- Klauzula `else` se izvršava samo kada se `try` normalno završi. `else` klauzula je validna samo ako `try` naredba ima barem jednu `except` klauzulu.
- Izbegavajte korišćenje obrađivača izuzetaka.

### Podizanje izuzetka

#### Uvod u podizanje izuzetka

Da biste podigli izuzetak, koristite sledeću `raise` izjavu:

```py
raise ExceptionType()
```

ExceptionType() mora biti podklasa klase `BaseException`. Tipično, to je podklasa klase `Exception`. Imajte na umu da `ExceptionType` ne mora biti direktno nasleđen od `Exception` klase. Može indirektno nasleđivati od klase koja je podklasa klase `Exception`.

Klasa `BaseExceptionima` ima  `__init__` metodu koja prihvata `*args` argument. To znači da možete proslediti bilo koji broj argumenata objektu izuzetka prilikom pokretanja izuzetka.

Sledeći primer koristi `raise` naredbu za pokretanje `ValueError` izuzetka. Metodi `__init__` se prosleđuju tri argumenta `ValueError`:

```py
try:
    raise ValueError('The value error exception', 'x', 'y')
except ValueError as ex:
    print(ex.args)
```

Izlaz:

```shell
('The value error exception', 'x', 'y')
```

#### Ponovo pokretanje trenutnog izuzetka

Ponekad želite da evidentirate izuzetak i ponovo pokrenete isti izuzetak. U ovom slučaju, možete koristiti `raise` naredbu bez navođenja objekta izuzetka.

Na primer, sledeće definiše `division()` funkciju koja vraća deljenje dva broja:

```py
def division(a, b):
    try:
        return a / b
    except ZeroDivisionError as ex:
        print('Logging exception:', str(ex))
        raise
```

Ako drugom argumentu funkcije `division()` prosledite nulu,  `division()` doći će do `ZeroDivisionError` izuzetka. Međutim, umesto obrade izuzetka, možete ga evidentirati i ponovo podići.

Imajte na umu da ne morate da navodite objekat izuzetka u `raise` naredbi. U ovom slučaju, Pajton zna da će `raise` naredba podići trenutni izuzetak koji je uhvaćen klauzulom `except`.

Sledeći kod izaziva `ZeroDivisionError` izuzetak:

```py
division(1, 0)
```

I videćete i poruku o evidentiranju i izuzetak u izlazu:

```shell
Logging exception: division by zero
Traceback (most recent call last):
  File "c:/pythontutorial/app.py", line 9, in <module>
    division(1, 0)
  File "C:/pythontutorial/app.py", line 3, in division
    return a / b
ZeroDivisionError: division by zero
```

#### Pokretanje još jednog izuzetka tokom obrade izuzetka

Prilikom obrade izuzetka, možda ćete želeti da pokrenete drugi izuzetak. Na primer:

```py
def division(a, b):
    try:
        return a / b
    except ZeroDivisionError as ex:
        raise ValueError('b must not zero')
```

U `division()` funkciji, podižemo `ValueError` izuzetak ako `ZeroDivisionError` se dogodi.

Ako pokrenete sledeći kod, dobićete detalje traga steka:

```py
division(1, 0)
```

Izlaz:

```shell
Traceback (most recent call last):
  File "C:/pythontutorial/app.py", line 3, in division
    return a / b
ZeroDivisionError: division by zero
```

During handling of the above exception, another exception occurred:

```shell
Traceback (most recent call last):
  File "C:/pythontutorial/app.py", line 8, in <module>
    division(1, 0)
  File "C:/pythontutorial/app.py", line 5, in division
    raise ValueError('b must not zero')
ValueError: b must not zero
```

- Prvo, `ZeroDivisionError` izuzetak se javlja:

    ```shell
    Traceback (most recent call last):
    File "C:/pythontutorial/app.py", line 3, in division
        return a / b
    ZeroDivisionError: division by zero
    ```

- Drugo, tokom obrade `ZeroDivisionError` izuzetka, `ValueError` izuzetak se javlja:

    ```shell
    Traceback (most recent call last):
    File "C:/pythontutorial/app.py", line 8, in <module>
        division(1, 0)
    File "C:/pythontutorial/app.py", line 5, in division
        raise ValueError('b must not zero')
    ValueError: b must not zero
    ```

#### Rezime podizanje izuzetaka

- Koristite Pajton `raise` naredbu da biste podigli izuzetak.
- Prilikom obrade izuzetka, možete pokrenuti isti ili drugi izuzetak.

### raise from naredba

#### Uvod u raise from naredbu

Izjava `raise from` ima sledeću sintaksu:

```py
raise <ExceptionType> from <cause>
```

Tehnički, to je ekvivalentno sledećem:

```py
ex = ExceptionType
ex.__cause__ = cause
raise ex
```

Podrazumevano, `__cause__` atribut na objektima izuzetaka je uvek inicijalizovan na `None`.

#### Primer naredbe raise from

Sledeća `divide()` funkcija deli jedan broj drugim i vraća rezultat deljenja:

```py
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError as ex:
        raise ValueError('b must not be zero')
```

Funkcija `divide()` ima obrađivač izuzetaka koji hvata `ZeroDivisionError` izuzetak. Unutar obrađivača, generišemo novi `ValueError` izuzetak.

Ako drugom argumentu funkcije dodamo nulu divide():

```py
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError as ex:
        raise ValueError('b must not be zero') from ex

divide(10, 0)
```

dobićete sledeći trag steka:

```shell
Traceback (most recent call last):
  File "c:/python/app.py", line 3, in divide
    return a / b
ZeroDivisionError: division by zero
```

Za vreme obrade ovog izuzetka drugi izuzetak se pojavljuje:

```shell
Traceback (most recent call last):
  File "c:/python/app.py", line 8, in <module>
    divide(10, 0)
  File "c:/python/app.py", line 5, in divide
    raise ValueError('b must not be zero')
ValueError: b must not be zero
```

Poruka o uvozu je:

```shell
During handling of the above exception, another exception occurred: ZeroDivisionError
```

To znači da se izuzetak dogodio dok ste obrađivali `ValueError` izuzetak.

Da biste naložili Pajtonu da želite da izmenite i prosledite `ZeroDivisionError` izuzetak, možete koristiti `raise from` naredbu:

```py
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError as ex:
        raise ValueError('b must not be zero') from ex

divide(10, 0)
```

Kada pokrenete kod, dobićete sledeći trag steka:

```shell
Traceback (most recent call last):
  File "c:/python/app.py", line 3, in divide
    return a / b
ZeroDivisionError: division by zero

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "c:/python/app.py", line 8, in <module>
    divide(10, 0)
  File "c:/python/app.py", line 5, in divide
    raise ValueError('b must not be zero') from ex
ValueError: b must not be zero
```

Sada dobijate `ValueError` izuzetak sa uzrokom dodatim atributu `__cause__` objekta izuzetka.

Sledeći kod menja gornji kod da bi se prikazao `__cause__` atribut `ValueError` izuzetka:

```py
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError as ex:
        raise ValueError('b must not be zero') from ex

try:
    divide(10, 0)
except ValueError as ex:
    print('cause:', ex.__cause__)
    print('exception:', ex)
```

Izlaz:

```shell
cause: division by zero
exception: b must not be zero
```

#### Pajton podiže izuzetak iz None

Ako uzrok izuzetka nije važan, možete ga izostaviti korišćenjem sledeće `raise exception from None` naredbe:

```py
raise <ExceptionType> from None
```

Na primer, možete sakriti uzrok ValueErrorizuzetka u divide()funkciji na sledeći način:

```py
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        raise ValueError('b must not be zero') from None

try:
    divide(10, 0)
except ValueError as ex:
    print('cause:', ex.__cause__)
    print('exception:', ex)
```

Izlaz:

```shell
cause: None
exception: b must not be zero
```

Sada, `__cause__` je None. Takođe, `divide()` funkcija podiže `ValueError` izuzetak bez ikakvih dodatnih informacija.

Ako uklonite try izjavu u kodu koja poziva `divide()` funkciju:

```py
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        raise ValueError('b must not be zero') from None


divide(10, 0)
```

dobićete sledeći trag steka:

```shell
Traceback (most recent call last):
  File "c:/python/app.py", line 8, in <module>
    divide(10, 0)
  File "c:/python/app.py", line 5, in divide
    raise ValueError('b must not be zero') from None
ValueError: b must not be zero
```

#### Rezime raise from naredbe

-- Koristite Pajton `raise from` naredbu da biste izmenili i prosledili postojeći izuzetak.
-- Koristite `raise exception from None` naredbu da sakrijete uzrok izuzetka.

### Prilagođeni izuzetak

#### Uvod u prilagođeni izuzetak

Da biste kreirali prilagođenu klasu izuzetaka, definišete klasu koja nasleđuje ugrađenu `Exception` klasu ili jednu od njenih podklasa, kao što je `ValueError class`

Sledeći primer definiše `CustomException` klasu koja nasleđuje od `Exception` klase:

```py
class CustomException(Exception):
    """ my custom exception class """
```

Imajte na umu da `CustomException` klasa ima dokumentacioni string koji se ponaša kao iskaz. Stoga, ne morate dodavati `pass` iskaz da bi sintaksa bila validna.

Da biste podigli izuzetak `CustomException`, koristite `raise` naredbu. Na primer, sledeći kod koristi `raise` naredbu za podizanje izuzetka `CustomException`:

```py
class CustomException(Exception):
    """ my custom exception class """

try:
    raise CustomException('This is my custom exception')
except CustomException as ex:
    print(ex)
```

Izlaz:

```py
This is my custom exception
```

Kao i standardne klase izuzetaka, prilagođeni izuzeci su takođe klase. Stoga, možete dodati funkcionalnosti prilagođenim klasama izuzetaka kao što su:

- Dodavanje atributa i svojstava.
- Dodavanje metoda, npr. evidentiranje izuzetka, formatiranje izlaza itd.
- Prepisivanje metoda `__str__` and `__repr__`.
- I raditi bilo šta drugo što možete da radite sa redovnom klasom.

U praksi, želećete da prilagođene izuzetke organizujete kreiranjem prilagođene hijerarhije izuzetaka. Prilagođena hijerarhija izuzetaka vam omogućava da hvatate izuzetke na više nivoa, kao što su standardne klase izuzetaka.

#### Primer prilagođenog izuzetka

Pretpostavimo da treba da razvijete program koji pretvara temperaturu iz Farenhajta u Celzijuse.

Minimalna i maksimalna vrednost temperature u Farenhajtima su 32 i 212. Ako korisnici unesu vrednost koja nije u ovom opsegu, želite da pokrenete prilagođeni izuzetak, npr. `FahrenheitError`.

``Definišite prilagođenu klasu izuzetka``
Sledeće definiše `FahrenheitError` klasu izuzetka:

```py
class FahrenheitError(Exception):
    min_f = 32
    max_f = 212

    def __init__(self, f, *args):
        super().__init__(args)
        self.f = f

    def __str__(self):
        return f'The {self.f} is not in a valid range {self.min_f, self.max_f}'
```

Kako to funkcioniše.

- Prvo, definišite klasu `FahrenheitError` koja nasleđuje od `Exception` klase.
- Drugo, dodajte dva atributa klase, `min_f` i `max_f` koji predstavljaju minimalne i maksimalne vrednosti Farenhajta.
- Treće, definišite `__init__` metod koji prihvata vrednost Farenhajta ( f ) i određeni broj pozicionih argumenata ( *args ). U `__init__`  metodi, pozovite `__init__` metod osnovne klase. Takođe, dodelite `f` argument atributu `f` instance.
- Konačno, prepišite `__str__` metodu da biste vratili prilagođenu reprezentaciju instance klase u obliku stringa.

Sledeća definicija definiše `fahrenheit_to_celsius` funkciju koja prihvata temperaturu u Farenhajtima i vraća temperaturu u Celzijusima:

```py
def fahrenheit_to_celsius(f: float) -> float:
    if f < FahrenheitError.min_f or f > FahrenheitError.max_f:
        raise FahrenheitError(f)

    return (f - 32) 5 / 9
```

Funkcija `fahrenheit_to_celsius` podiže `FahrenheitError` izuzetak ako ulazna temperatura nije u važećem opsegu. U suprotnom, konvertuje temperaturu iz Farenhajta u Celzijuse.

``Napravite glavni program``
Sledeći glavni program koristi `fahrenheit_to_celsius` funkciju i `FahrenheitError` prilagođenu klasu izuzetaka:

```py
if __name__ == '__main__':
    f = input('Enter a temperature in Fahrenheit:')
    try:
        f = float(f)
    except ValueError as ex:
        print(ex)
    else:
        try:
            c = fahrenheit_to_celsius(float(f))
        except FahrenheitError as ex:
            print(ex)
        else:
            print(f'{f} Fahrenheit = {c:.4f} Celsius')
```

Kako to funkcioniše.

- Prvo, zamolite korisnike da unesu temperaturu u Farenhajtima.

    ```py
    f = input('Enter a temperature in Fahrenheit:')
    ```

- Drugo, konvertujte ulaznu vrednost u broj sa pokretnim zarezom . Ako float() ne može da konvertujete ulaznu vrednost, program će izazvati `ValueError` izuzetak. U ovom slučaju, prikazuje poruku o grešci iz `ValueError` izuzetka:

    ```py
    try:
        f = float(f)
        # ...
    except ValueError as ex:
        print(ex)
    ```

- Treće, konvertujte temperaturu u Celzijuse pozivanjem `fahrenheit_to_celsius` funkcije i ispišite poruku o grešci ako ulazna vrednost nije validna Fahrenheit vrednost:

    ```py
    try:
        c = fahrenheit_to_celsius(float(f))
    except FahrenheitError as ex:
        print(ex)
    else:
        print(f'{f} Fahrenheit = {c:.4f} Celsius')
    ```

Spojite sve zajedno

```py
class FahrenheitError(Exception):
    min_f = 32
    max_f = 212

    def __init__(self, f, *args):
        super().__init__(args)
        self.f = f

    def __str__(self):
        return f'The {self.f} is not in a valid range {self.min_f, self.max_f}'

def fahrenheit_to_celsius(f: float) -> float:
    if f < FahrenheitError.min_f or f > FahrenheitError.max_f:
        raise FahrenheitError(f)

    return (f - 32) ` 5 / 9


if __name__ == '__main__':
    f = input('Enter a temperature in Fahrenheit:')
    try:
        f = float(f)
    except ValueError as ex:
        print(ex)
    else:
        try:
            c = fahrenheit_to_celsius(float(f))
        except FahrenheitError as ex:
            print(ex)
        else:
            print(f'{f} Fahrenheit = {c:.4f} Celsius')
```

#### Rezime prilagođeni izuzetak

- Podklasirajte `Exception` klasu ili jednu od njenih podklasa da biste definisali prilagođenu klasu izuzetaka.
- Napravite hijerarhiju klasa izuzetaka kako biste klase izuzetaka učinili organizovanijim i hvatali izuzetke na više nivoa.

## Dekoratori

### Uvod u dekoratore

Dekorator je funkcija koja uzima drugu funkciju kao argument i proširuje njeno ponašanje bez eksplicitne promene originalne funkcije.

#### Jednostavan primer za razumevanje koncepta

Sledeće definiše `net_price` funkciju:

```py
def net_price(price, tax):
    """ calculate the net price from price and tax
    Arguments:
        price: the selling price
        tax: value added tax or sale tax
    Return
        the net price
    """
    return price ` (1 + tax)
```

Funkcija `net_price` izračunava neto cenu iz prodajne cene i poreza. Vraća vrednost `net_price` kao broj.

Pretpostavimo da treba da formatirate neto cenu koristeći valutu USD. Na primer, 100 postaje $100. Da biste to uradili, možete koristiti dekorator.

Po definiciji, dekorator je funkcija koja uzima funkciju kao argument:

```py
def currency(fn):
    pass
```

I vraća drugu funkciju:

```py
def currency(fn):
    def wrapper(*args, **kwargs):
        fn(*args, **kwargs)

    return wrapper
```

Funkcija `currency` vraća `wrapper` funkciju. `wrapper` funkcija ima parametre `*args` i `**kwargs`.

Ovi parametri vam omogućavaju da pozovete bilo koju `fn` funkciju sa bilo kojom kombinacijom pozicionih argumenata i argumenata ključnih reči.

U ovom primeru, wrapper funkcija u suštini direktno izvršava `fn` funkciju i ne menja  ponašanje funkcije `fn`.

U `wrapper` funkciji možete pozvati `fn` funkciju, dobiti njen rezultat i formatirati rezultat kao string valute:

```py
def currency(fn):
    def wrapper(*args, **kwargs):
        result = fn(*args, **kwargs)
        return f'${result}'
    return wrapper
```

Funkcija `currency` je dekorator.

Prihvata bilo koju funkciju koja vraća broj i formatira taj broj kao string valute.

Da biste koristili `currency` dekorator, potrebno je da `net_price` prosledite funkciju kako biste dobili novu funkciju i izvršili novu funkciju kao da je originalna funkcija. Na primer:

```py
def currency(fn):
    def wrapper(*args, **kwargs):
        result = fn(*args, **kwargs)
        return f'${result}'
    return wrapper

net_price = currency(net_price)
print(net_price(100, 0.05))
```

Izlaz:

```shell
$105.0
```

#### Definicija dekoratora u Pajtonu

Generalno, dekorater je:

- Funkcija koja uzima drugu funkciju (originalnu funkciju) kao argument i vraća drugu funkciju ( ili zatvaranje )
- Zatvaranje obično prihvata bilo koju kombinaciju pozicionih argumenata i argumenata samo za ključne reči.
- Funkcija zatvaranja poziva originalnu funkciju koristeći argumente prosleđene zatvaranju i vraća rezultat funkcije.

Unutrašnja funkcija je zatvaranje jer referencira `fn` argument iz svog obuhvatajućeg opsega ili funkcije dekoratora.

#### Simbol @

U prethodnom primeru, `currency` je dekorator. Funkciju `net_price` možete dekorisati  koristeći sledeću sintaksu:

```py
net_price = currency(net_price)
```

Generalno, ako `decorate` je funkcija dekoratora i želite da dekorišete drugu funkciju `fn`, možete koristiti ovu sintaksu:

```py
fn = decorate(fn)
```

Da bi bilo praktičnije, Pajton pruža kraći način ovako:

```py
@decorate
def fn():
    pass
```

Na primer, umesto korišćenja sledeće sintakse:

```py
net_price = currency(net_price)
```

... možete dekorisati `net_price` funkciju koristeći `@currency` dekorator na sledeći način:

```py
@currency
def net_price(price, tax):
    """ calculate the net price from price and tax
    Arguments:
        price: the selling price
        tax: value added tax or sale tax
    Return
        the net price
    """
    return price ` (1 + tax)
```

Sada sve to spojimo.

```py
def currency(fn):
    def wrapper(*args, **kwargs):
        result = fn(*args, **kwargs)
        return f'${result}'
    return wrapper


@currency
def net_price(price, tax):
    """ calculate the net price from price and tax
    Arguments:
        price: the selling price
        tax: value added tax or sale tax
    Return
        the net price
    """
    return price (1 + tax)

print(net_price(100, 0.05))
```

#### Introspekcija dekorisanih funkcija

Kada dekorišete funkciju:

```py
@decorate
def fn(*args,**kwargs):
    pass
```

To je ekvivalentno:

```py
fn = decorate(fn)
```

Funkcija decorate vraća novu funkciju, koja je funkcija omotač ili zatvaranje.

Ako koristite ugrađenu help funkciju da biste prikazali dokumentaciju nove funkcije, nećete videti dokumentaciju originalne funkcije. Na primer:

```py
help(net_price)
```

Izlaz:

```shell
wrapper(*args, **kwargs)
None
```

Takođe, ako proverite ime nove funkcije, Pajton će vratiti ime unutrašnje funkcije koju je vratio dekorator:

```py
print(net_price.__name__)
```

Izlaz:

```shell
wrapper
```

Dakle, kada dekorišete funkciju, izgubićete originalni potpis funkcije i dokumentaciju.

Da biste ovo popravili, možete koristiti `wraps` funkciju iz `functools` standardnog modula. U stvari, `wraps` funkcija je takođe dekorator.

Sledeće je prikazano kako se koristi `wraps` dekorator:

```py
from functools import wraps

def currency(fn):
    @wraps(fn)                          # <<--- 
    def wrapper(*args, **kwargs):
        result = fn(*args, **kwargs)
        return f'${result}'
    return wrapper

@currency
def net_price(price, tax):
    """ calculate the net price from price and tax
    Arguments:
        price: the selling price
        tax: value added tax or sale tax
    Return
        the net price
    """
    return price ` (1 + tax)

help(net_price)
print(net_price.__name__)
```

Izlaz:

```shell
net_price(price, tax)
    calculate the net price from price and tax
    Arguments:
        price: the selling price
        tax: value added tax or sale tax
    Return
        the net price

net_price
```

Rezime dekoratora

- Dekorator je funkcija koja menja ponašanje druge funkcije bez eksplicitnog modifikovanja iste.
- Koristite `@` simbol za dekorisanje funkcije.
- Koristite `wraps` funkciju iz `functools` ugrađenog modula da biste zadržali dokumentaciju i naziv originalne funkcije.

### Dekoratori sa argumentima

#### Uvod u dekorator sa argumentima

Pretpostavimo da imate funkciju koja se zove `say` i da ispisuje poruku:

```py
def say(message):
    ''' print the message 
    Arguments
        message: the message to show
    '''
    print(message)
```

i želite da izvršite say() funkciju 5 puta ponavljajući svaki put kada je pozovete. Na primer:

```py
say('Hi')
```

Trebalo bi da prikaže sledeću `Hi` poruku pet puta na sledeći način:

```shell
Hi
Hi
Hi
Hi
Hi
```

Da biste to uradili, možete koristiti običan dekorator:

```py
@repeat
def say(message):
    ''' print the message
    Arguments
        message: the message to show
    '''
    print(message)
```

A dekorator `repeat` možete definisati na sledeći način:

```py
def repeat(fn):
    @wraps(fn)
    def wrapper(*args, **kwargs):
        for _ in range(5):
            result = fn(**args, **kwargs)
        return result

    return wrapper    
```

Sledeći kod prikazuje kompletan kod:

```py
from functools import wraps

def repeat(fn):
    @wraps(fn)
    def wrapper(*args, **kwargs):
        for _ in range(5):
            result = fn(*args, **kwargs)
        return result

    return wrapper

@repeat
def say(message):
    ''' print the message
    Arguments
        message: the message to show
    '''
    print(message)

say('Hello')
```

Šta ako želite da izvršite say() funkciju deset puta više? U ovom slučaju, potrebno je da promenite čvrsto kodiranu vrednost 5 u `repeat` dekoratoru.

Međutim, ovo rešenje nije fleksibilno. Na primer, želite da koristite `repeat` dekorator da biste izvršili funkciju 5 puta, a zatim još 10 puta. `repeat` dekorator ne bi ispunio zahtev.

Da biste ovo popravili, potrebno je da promenite `repeat` dekorator tako da prihvata argument koji određuje koliko puta funkcija treba da se izvrši ovako:

```py
@repeat(5)
def say(message):
    ...
```

Da bi se definisao `repeat` dekorator, `repeat(5)` treba da vrati originalni dekorator.

```py
def repeat(times):
    # return the original "repeat" decorator
```

Nova `repeat` funkcija vraća dekorator. I često se naziva fabrika dekoratora.

Sledeća `repeat` funkcija vraća dekorator:

```py
def repeat(times):
    ''' call a function a number of times '''
    def decorate(fn):
        @wraps(fn)
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = fn(*args, **kwargs)
            return result
        return wrapper
    return decorate
```

U ovom kodu, `decorate` funkcija je dekorator. Ekvivalentna je originalnom `repeat` dekoratoru.

Imajte na umu da nova funkcija repeat nije dekorator. To je fabrika dekoratora koja vraća dekorator.

Ako sve to spojimo,

```py
from functools import wraps

def repeat(times):
    ''' call a function a number of times '''
    def decorate(fn):
        @wraps(fn)
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = fn(*args, **kwargs)
            return result
        return wrapper
    return decorate

@repeat(10)
def say(message):
    ''' print the message
    Arguments
        message: the message to show
    '''
    print(message)

say('Hello')
```

Izlaz:

```shell
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
Hello
```

#### Rezime dekoratora sa argumentima

Koristite fabrički dekorator da biste vratili dekorator koji prihvata argumente.

### Klasa kao dekorator

#### Uvod u klasu kao dekorator

Do sada ste naučili kako da koristite funkcije za definisanje dekoratora.

Na primer, sledeća `star` funkcija ispisuje određeni broj `'` znakova pre i posle pozivanja dekorisane funkcije:

```py
def star(n):
    def decorate(fn):
        def wrapper(*args, **kwargs):
            print(n`'`')
            result = fn(*args, **kwargs)
            print(result)
            print(n`'`')
            return result
        return wrapper
    return decorate
```

`star` je fabrika dekoratora koja vraća dekorator. Prihvata argument koji određuje broj ` znakova za prikazivanje.

Sledeće ilustruje kako se koristi `star` fabrika dekoratora:

```py
def star(n):
    def decorate(fn):
        def wrapper(*args, **kwargs):
            print(n * `*`)
            result = fn(*args, **kwargs)
            print(result)
            print(n * `*`)
            return result
        return wrapper
    return decorate

@star(5)
def add(a, b):
    return a + b

add(10, 20)
```

Izlaz:

```shell
*****
30
*****
```

Fabrika dekoratora `star()` prima argument i vraća pozivnu funkciju. Pozivna funkcija prima argument ( `fn` ), što je funkcija koja će biti dekorisana. Takođe, pozivna funkcija može pristupiti argumentu ( `n` ) koji je prosleđen fabrici dekoratora.

Instanca klase može biti pozana implementira `__call__` metodu. Stoga, metodu `__call__` možete napraviti kao dekorator.

Sledeći primer prepisuje `star` fabriku dekoratora koristeći klasu umesto toga:

```py
class Star:
    def __init__(self, n):
        self.n = n

    def __call__(self, fn):
        def wrapper(*args, **kwargs):
            print(self.n`'`')
            result = fn(*args, **kwargs)
            print(result)
            print(self.n`'`')
            return result
        return wrapper
```

I možete koristiti `Star` klasu kao dekorater ovako:

```py
@Star(5)
def add(a, b):
    return a + b
```

@Star(5) vraća instancu klase `Star`. Tu instanca moguće je pozvati, tako da možete uraditi nešto poput ovoga:

```py
add = Star(5)(add)
```

Dakle, možete koristiti pozivajuće klase za dekorisanje funkcija.

Spojite sve ovo zajedno:

```py
from functools import wraps

class Star:
    def __init__(self, n):
        self.n = n

    def __call__(self, fn):
        @wraps(fn)
        def wrapper(*args, **kwargs):
            print(self.n * `*`)
            result = fn(*args, **kwargs)
            print(result)
            print(self.n * `*`)
            return result
        return wrapper

@Star(5)
def add(a, b):
    return a + b

add(10, 20)
```

Izlaz:

```shell
*****
30
*****
```

#### Rezime klase kao dekoratora

- Koristite pozivajuće klase kao dekoratore implementacijom `__call__` metode.
- Prosledite argumente dekoratora metodi `__init__`.

### Monkey patching

#### Uvod u krpljenje koda

Majmunsko krpljenje je tehnika koja vam omogućava da izmenite ili proširite ponašanje postojećih `modula`, `klasa` ili `funkcija` tokom izvršavanja programa bez promene originalnog izvornog koda.

#### Primena majmunskog krpljenja

Da biste primenili tehniku majmunskog krpljenja, pratite ove korake:

- Prvo, identifikujte cilj koji može biti `modul`, `klasa`, `metoda` ili `funkcija` koju želite da zakrpite.
- Drugo, kreirajte svoj peč tako što ćete napisati kod za dodavanje, modifikovanje ili zamenu postojeće logike.
- Treće, primenite zakrpu koristeći dodelu da biste je primenili na cilj. Zakrpa će prebrisati ili proširiti postojeće ponašanje.

Iako je majmunsko krpljenje moćan alat, trebalo bi da ga koristite pažljivo kako biste izbegli neočekivano ponašanje.

#### Primer krpljenja majmuna

Pretpostavimo da imate `MyClass` klasu koja ima samo jednu `__init__()` metodu:

```py
class Robot:
    def __init__(self, name):
        self.name = name
```

Da biste proširili ponašanje klase `Robot` tokom izvršavanja bez promene `Robot` klase, možete koristiti tehniku "majmunskog krpljenja".

Slika koja vam je potrebna da biste proširili ponašanje koje omogućava robotskim instancama da govore. Evo koraka za to:

- Prvo, definišite funkciju koja se zove `add_speech` koja prihvata klasu kao parametar:

    ```py
    def add_speech(cls):
        cls.speak = lambda self, message: print(message)
        return cls
    ```

    Funkcija `add_speech()` dodaje `speak()` metod dodavanjem atributa `speak` klasi `cls`, dodeljivanjem `lambda izraza` i vraćanjem klase. `Lambda izraz` prihvata poruku i prikazuje je u konzoli.

- Drugo, zakrpite `Robot` klasu tako što ćete je proslediti `add_speech()` metodi:

    ```py
    Robot = add_speech(Robot)
    ```

    Nakon ove linije koda, `Robot` klasa će imati `speak()` metodu.

    Imajte na umu da funkciju `add_speech()` možete koristiti za krpljenje bilo koje klase, ne samo `Robot` klase.

- Treće, kreirajte novu instancu klase `Robot` i pozovite `speak()` metodu:

    ```py
    robot = Robot('Optimus Prime')
    robot.speak('Hi')
    ```

Spojite sve ovo zajedno:

```py
def add_speech(cls):
    cls.speak = lambda self, message: print(message)
    return cls

class Robot:
    def __init__(self, name):
        self.name = name

Robot = add_speech(Robot)

robot = Robot('Optimus Prime')
robot.speak('Hi')
```

Ako pokrenete program, videćete sledeći izlaz:

```shell
Hi
```

Pošto je ova linija koda dekorator:

```py
Robot = add_speech(Robot)
```

možete ga ukloniti i koristiti sintaksu dekoratora:

```py
@add_speech
class Robot:
    def __init__(self, name):
        self.name = name
```

Novi kod će izgledati ovako:

```py
def add_speech(cls):
    cls.speak = lambda self, message: print(message)
    return cls

@add_speech
class Robot:
    def __init__(self, name):
        self.name = name

robot = Robot('Optimus Prime')
robot.speak('Hi')
```

#### Kada koristiti majmunsko krpljenje

U praksi, trebalo bi da koristite "monkey patching" samo kada je to neophodno jer može otežati razumevanje i debagovanje koda.

Na primer, ako koristite biblioteku treće strane i ona ima hitnu grešku za koju ne možete čekati zvanično objavljivanje, u tom slučaju možete koristiti "monkey patching" da biste primenili brze ispravke dok čekate odgovarajuće rešenje.

Drugi scenario je da želite da dodate funkcionalnost klasama koje ne kontrolišete i ne možete da koristite druge tehnike poput nasleđivanja ili kompozicije, u ovom slučaju je korisno "monkey patching" ( "majmunsko krpljenje" ).

U praksi, naći ćete "monkey patching" u biblioteci lažnih primeraka kao što je standardni `unittest.mockModul`. `unittest.mockModul` ima `patch()` metodu koja privremeno zamenjuje cilj lažnim objektom.

#### Rezime Monkey patching

- Tehnika "majmunskog krpljenja“ u Pajtonu vam omogućava da dinamički menjate ili proširujete postojeći kod tokom izvršavanja bez promene originalnog koda.
- Koristite tehniku majmunskog krpljenja ako imate dobar razlog za to.

## Iteratori

### Uvod u iteratore

Iterator je objekat koji implementira:

- `__iter__` metoda koja vraća sam objekat.
- `__next__` metoda koja vraća sledeću stavku. Ako su sve stavke vraćene, metoda izbacuje `StopIteration` izuzetak.

Imajte na umu da su ove dve metode poznate i kao `Iterator protokol`.

Pajton vam omogućava da koristite iteratore u `for` petljama, `razumevanju` i drugim ugrađenim funkcijama, uključujući `map`, `filter`, `reduce` i `zip`.

#### Primer iteratora u Pajtonu

Kvadrat broja je proizvod celog broja sa samim sobom. Na primer, kvadrat broja 2 je 4 ( = 2 ` 2 ).

Sledeći primer definiše `Square` `iterator` klasu koja vraća kvadratne brojeve.

```py
class Square:
    def __init__(self, length):
        self.length = length
        self.current = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.current >= self.length:
            raise StopIteration

        self.current += 1
        return self.current `` 2
```

Kako to funkcioniše.

- Prvo, inicijalizujte atribute `length` i `current` u `__init__` metodi. Atribut `length` određuje broj kvadriranih brojeva koje klasa treba da vrati. A `current` atribut prati trenutni ceo broj.

- Drugo, implementirajte `__iter__` metodu koja vraća `self` objekat.

- Treće, implementirajte `__next__` metodu koja vraća sledeći kvadrirati broj. Ako je broj vraćenih kvadratnih brojeva veći od dužine, metoda izbacuje izuzetak `StopIteration`.

#### Korišćenje objekta iteratora

Sledeće je prikazano kako se koristi `iterator` `Square` u `for` petlji:

```py
square = Square(5)

for sq in square:
    print(sq)
```

Izlaz:

```shell
1
4
9
16
25
```

Kako ovo funkcioniše:

- Prvo, kreirajte novu instancu klase `Square`.
- Zatim, koristite `for` petlju za iteraciju kroz elemente `Square` iteratora.

Kada prođete kroz sve stavke, iterator je iscrpljen. To znači da morate da kreirate novi iterator da biste ponovo iterirali kroz njegove stavke.

Ako pokušate da koristite iterator koji je već iscrpljen, dobićete izuzetak `StopIteration`. Na primer:

```py
next(square)
```

```shell
Error:
StopIteration
```

Takođe, iterator se ne može ponovo pokrenuti jer ima samo `__next__` metodu koja vraća sledeću stavku iz kolekcije.

#### Rezime uvoda u iteratore

- Iterator je objekat koji implementira `__iter__` i `__next__` metode.
- Iterator se ne može ponovo koristiti nakon što su svi elementi vraćeni.

### Iterator naspram iterabilnog objekta

``Rezime``: u ovom tutorijalu ćete saznati o iteratoru i iterabilnim objekatima u Pajtonu i njihovim razlikama.

#### Pojam iteratora

Iterator je objekat koji implementira `iterator` protokol. Drugim rečima, iterator je objekat koji implementira sledeće metode:

- `__iter__` vraća sam objekat iteratora.
- `__next__` vraća sledeći element.

Kada završite iteraciju kolekcije pomoću iteratora, iterator je iscrpljen. To znači da više ne možete koristiti objekat iteratora.

#### Pojam iterabilni objekta

Iterabilni objekat je objekat preko kojeg možete iterirati.

Objekat je iterabilan kada implementira `__iter__` metodu. A njegova `__iter__` metoda vraća novi iterator.

#### Ugrađene liste i iteratori

U Pajtonu, lista je uređena kolekcija elemenata. Takođe je iterabilna jer objekat liste ima `__iter__` metodu koja vraća iterator. Na primer:

```py
numbers = [1, 2, 3]

number_iterator = numbers.__iter__()
print(type(number_iterator))
```

Izlaz:

```shell
<class 'list_iterator'>
```

U ovom primeru, `__iter__` metod vraća iterator tipa `list_iterator`.

Pošto `list_iterator` implementira `__iter__` metodu, možete koristiti `iter` ugrađenu funkciju da biste dobili objekat iteratora:

```py
numbers = [1, 2, 3]
number_iterator = iter(numbers)
```

Pošto `list_iterator` takođe implementira `__next__` metod, možete koristiti ugrađenu funkciju `next` za iteraciju kroz listu:

```py
numbers = [1, 2, 3]

number_iterator = iter(numbers)

next(number_iterator)
next(number_iterator)
next(number_iterator)
```

Ako ponovo pozovete `next` funkciju, dobićete `StopIteration` izuzetak.

```py
next(number_iterator)
```

```shell
Error:
StopIteration
```

To je zato što je `list_iterator` iscrpljen. Da biste ponovo iterirali listu, potrebno je da kreirate novi iterator.

Ovo ilustruje odvajanje liste od njenog iteratora. Lista se kreira jednom, dok se iterator kreira svaki put kada je potrebno da se iterira kroz listu.

#### Pajton iterator i iterabilni objekat

Sledeće definiše `Colors` klasu:

```py
class Colors:
    def __init__(self):
        self.rgb = ['red', 'green', 'blue']
        self.__index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.__index &gt;= len(self.rgb):
            raise StopIteration

        # return the next color
        color = self.rgb[self.__index]
        self.__index += 1
        return color
```

U ovom primeru, `Colors` klasa igra dve uloge:

- iterabilnu i
- iteratorsku.

Klasa `Colors` je iterator jer implementira `__iter__` i `__next__` metod. `__iter__` metod vraća sam objekat. `__next__` metod vraća sledeću stavku sa liste.

Klasa `Colors` je takođe iterabilna jer implementira `__iter__` metodu koja vraća sam objekat, a to je iterator.

Sledeći kod kreira novu instancu klase `Colors` i iterira kroz njene elemente koristeći `for` petlju:

```py
colors = Colors()

for color in colors:
    print(color)
```

Kada završite sa iteracijom, `colors` objekat postaje beskoristan. Ako pokušate ponovo da je iterirate, dobićete `StopIteration` izuzetak:

```py
next(colors)
```

```shell
Error:
StopIteration
```

Ako koristite `for` petlju, nećete dobiti ništa nazad. `Iterator` je prazan:

```py
for color in colors:
    print(color)
```

Da biste ponovo ponovili iteraciju, potrebno je da kreirate novi `colors` objekat sa `rgb` atributom. Ovo je neefikasno.

#### Odvajanje iteratora od iterabilnog objekta

Hajde da odvojimo `iterator` `colors` od njegovog iterabilnog objekta kao što Pajton radi sa iteratorom liste i listom.

Sledeće definiše `Colors` klasu:

```py
class Colors:
    def __init__(self):
        self.rgb = ['red', 'green', 'blue']

    def __len__(self):
        return len(self.rgb)
```

Sledeće definiše `ColorIterator` klasu:

```py
class ColorIterator:
    def __init__(self, colors):
        self.__colors = colors
        self.__index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.__index >= len(self.__colors):
            raise StopIteration

        # return the next color
        color = self.__colors.rgb[self.__index]
        self.__index += 1
        return color
```

Kako to funkcioniše.

- Metoda `__init__` prihvata iterabilni tip koja je instanca klase `Colors`.
- Metoda `__iter__` vraća sam iterator.
- Metoda `__next__` vraća sledeći element iz `Colors` objekta.

Sledeće je prikazano kako se koristi `ColorIterator` za iteraciju kroz `Colors` objekat:

```py
colors = Colors()
color_iterator = ColorIterator(colors)

for color in color_iterator:
    print(color)
```

Da biste ponovo iterirali `Colors` objekat, samo treba da kreirate novu instancu klase `ColorIterator`.

Postoji jedan problem!

Kada želite da iterirate `Colors` objekat, potrebno je da ručno kreirate novi `ColorIterator` objekat. Takođe, potrebno je da zapamtite ime iteratora `ColorIterator`.

Bilo bi odlično kada biste ovo mogli automatizovati. Da biste to uradili, možete učiniti `Colors` klasu iterabilnom implementacijom `__iter__` metode:

```py
class Colors:
    def __init__(self):
        self.rgb = ['red', 'green', 'blue']

    def __len__(self):
        return len(self.rgb)

    def __iter__(self):
        return ColorIterator(self)
```

Metoda `__iter__` vraća novu instancu klase `ColorIterator`.

Sada možete iterirati `Colors` objekat bez eksplicitnog kreiranja `ColorIteratorobjekta`:

```py
colors = Colors()

for color in colors:
    print(color)
```

Interno, `for` petlja poziva `__iter__` metod objekta `colors` da bi dobila iterator i koristi ga za iteraciju kroz elemente objekta `colors`.

Sledeće postavlja `ColorIterator` klasu unutar `Colors` klase da bi ih kapsuliralo u jednu klasu:

```py
class Colors:
    def __init__(self):
        self.rgb = ['red', 'green', 'blue']

    def __len__(self):
        return len(self.rgb)

    def __iter__(self):
        return self.ColorIterator(self)

    class ColorIterator:
        def __init__(self, colors):
            self.__colors = colors
            self.__index = 0

        def __iter__(self):
            return self

        def __next__(self):
            if self.__index >= len(self.__colors):
                raise StopIteration

            # return the next color
            color = self.__colors.rgb[self.__index]
            self.__index += 1
            return color
```

#### Rezime iteratori vs iterabilni objekti

- Iterabilni objekat je objekat koji implementira `__iter__` metodu koja vraća iterator.
- Iterator je objekat koji implementira `__iter__` metodu koja vraća samog sebe i `next` metodu koja vraća sledeći element.
- Iteratori su takođe iterabilne jedinice.

### iter

#### Uvod u funkciju iter

Funkcija `iter()` vraća iterator datog objekta:

```py
iter(object)
```

Funkcija `iter()` zahteva argument koji može biti iterabilni tip ili sekvenca. Generalno, argument `object` može biti bilo koji objekat koji podržava protokol `iterator` ili sekvence.

Kada pozovete `iter()` funkciju na objektu, funkcija prvo traži `__iter__()` metod tog objekta.

Ako `__iter__()` metod postoji, `iter()` funkcija ga poziva da bi dobila iterator. U suprotnom, `iter()` funkcija će tražiti `__getitem__()` metod.

Ako je `__getitem__()` dostupan, `iter()` funkcija kreira objekat iteratora i vraća taj objekat. U suprotnom, izaziva `TypeError` izuzetak.

#### Primeri funkcije iter()

Sledeći primer definiše jednostavnu `Counter` klasu i koristi `iter()` funkciju za dobijanje iteratora objekta `Counter`:

```py
class Counter:
    def __init__(self):
        self.__current = 0

counter = Counter()
iterator = iter(counter)
```

Ovo bi izazvalo `TypeError` jer counterobjekat nije iterabilan.

```py
TypeError: 'Counter' object is not iterable
```

Sledeće dodaje `__getitem__()` metodu `Counter` klasi:

```py
class Counter:
    def __init__(self):
        self.current = 0

    def __getitem__(self, index):
        if isinstance(index, int):
            self.current += 1
            return self.current
```

Pošto `Counter` implementira `__getitem__()` metodu koja vraća element na osnovu indeksa, to je sekvenca.

Sada možete koristiti `iter()` funkciju da biste dobili iterator `Counter`:

```py
counter = Counter()

iterator = iter(counter)
print(type(iterator))
```

Izlaz:

```shell
<class 'iterator'>
```

U ovom slučaju, Pajton kreira objekat iteratora i vraća ga. Stoga, možete koristiti objekat iteratora za iteraciju counter:

```py
for _ in range(1, 4):
    print(next(iterator))
```

Sledeće dodaje `CounterIterator` klasu klasi `Counter` i implementira iterabilni protokol:

```py
class Counter:
    def __init__(self):
        self.current = 0

    def __getitem__(self, index):
        if isinstance(index, int):
            self.current += 1
            return self.current

    def __iter__(self):
        return self.CounterIterator(self)

    class CounterIterator:
        def __init__(self, counter):
            self.__counter = counter

        def __iter__(self):
            return self

        def __next__(self):
            self.__counter.current += 1
            return self.__counter.current
```

Kako to funkcioniše.

- Klasa Counter implementira `__iter__()` metodu koja vraća `iterator`. `Iterator` za povratak je nova instanca klase `CounterIterator`.
- Klasa `CounterIterator` podržava protokol iteratora implementacijom metoda `__iter__()` i `__next__()`.

Kada postoje obe metode `__iter__()` i `__getitem__()` funkcija uvek koristi metod: `iter()`.

```py
counter = Counter()

iterator = iter(counter)
print(type(iterator))
```

Izlaz:

```shell
<class '__main__.Counter.CounterIterator'>
1
2
3
```

U ovom primeru, `iter()` funkcija poziva `__iter__()` metod umesto `__getitem__()` metode. Zato vidite `CounterIterator` u izlazu.

#### Drugi oblik funkcije iter()

Sledeća slika prikazuje drugi oblik funkcije `iter()`:

```py
iter(callable, sentinel)
```

Pozvaće iter(callable,sentinel) se pozivajuća metoda kada next()se pozove.

Vratiće vrednost koju je vratila pozivna funkcija ili će izazvati izuzetak StopIteration ako je rezultat jednak vrednosti sentinel-a.

Uzmimo primer da bismo razumeli kako to iter(callable, sentinel)funkcioniše.

- Prvo, definišite funkciju koja vraća zatvaranje:

    ```py
    def counter():
        count = 0

        def increase():
            nonlocal count
            count += 1
            return count

        return increase
    ```

    Funkcija `counter()` vraća zatvaranje. A zatvaranje vraća novi ceo broj počevši od jedan kada se pozove.

- Drugo, koristite `counter()` funkciju da biste prikazali brojeve od 1 do 3:

    ```py
    cnt = counter()

    while True:
        current = cnt()
        print(current)
        if current == 3:
            break
    ```

    Izlaz:

    ```shell
    1
    2
    3
    ```

    Da biste ga učinili generičkijim, možete umesto toga koristiti iterator.

- Treće, definišite novi iterator `Counter`:

    ```py
    class CounterIterator:
        def __init__(self, fn, sentinel):
            self.fn = fn
            self.sentinel = sentinel

        def __iter__(self):
            return self

        def __next__(self):
            current = self.fn()
            if current == self.sentinel:
                raise StopIteration

            return current
    ```

Konstruktor `CounterIterator`-a prihvata pozivajući tip `fn` i `sentinel`.

Metod `__next__()` vraća vrednost koju je vratila pozivna metoda ( `fn` ) ili podiže `StopIteration` izuzetak ako je vraćena vrednost jednaka `sentinelu`.

Sledeće je prikazano kako se koristi `CounterIterator`:

```py
cnt = counter()
iterator = CounterIterator(cnt, 4)
for count in iterator:
    print(count)
```

Izlaz:

```shell
1
2
3
```

Umesto definisanja novog iteratora svaki put kada želite da iterirate vrednosti koje vraća pozivna funkcija, možete koristiti `iter(callable, sentinel)` funkciju:

```py
cnt = counter()
iterator = iter(cnt, 4)

for count in iterator:
    print(count)
```

Izlaz:

```shell
1
2
3
```

#### Provera da li je objekat iterabilan

Koristite funkciju `iter()` da biste proverili da li je objekat iterabilan.

Da biste utvrdili da li je objekat iterabilan, možete proveriti da li implementira `__iter__()` ili `__getitem__()` metod.

Međutim, možete koristiti iter() funkciju da biste testirali da li je objekat iterabilan na sledeći način:

```py
def is_iterable(object):
    try:
        iter(object)
    except TypeError:
        return False
    else:
        return True
```

Ako objekat ne implementira ni `__iter__()` metod ni `__getitem__()` metod, `iter()` funkcija podiže `TypeError` izuzetak.

Sledeće je prikazano kako se koristi `is_iterable()` funkcija:

```py
print(is_iterable([1, 2, 3]))
print(is_iterable('Python iter'))
print(is_iterable(100))
```

Izlaz:

```shell
True
True
False
```

#### Rezime iter funkcije

Koristite `iter()` funkciju da biste dobili iterator objekta.

## Generatori

### Funkcije generatora

#### Uvod u generatore

Tipično, Pajton izvršava regularnu funkciju od vrha do dna na osnovu modela "izvršavanje od početka do završetka".

To znači da Pajton ne može pauzirati regularnu funkciju na pola, a zatim nastaviti funkciju nakon toga. Na primer:

```py
def greeting():
    print('Hi!')
    print('How are you?')
    print('Are you there?')
```

Kada Pajton izvršava `greeting()` funkciju, on izvršava kod red po red od vrha do dna.

Takođe, Pajton ne može pauzirati funkciju u nekom redu:

```py
print('How are you?')
```

... već izvršava i prelazi na drugi red koda i nastavlja izvršavanje dalje.

Da biste pauzirali funkciju na pola i nastavili od mesta gde je funkcija pauzirana, koristite `yield` naredbu.

Kada funkcija sadrži barem jednu `yield` naredbu, to je funkcija `generatora`.

Po definiciji, `generator` je funkcija koja sadrži barem jednu naredbu `yield`.

Kada pozovete `funkciju generatora`, ona vraća novi `objekat generatora`. Međutim, ne pokreće funkciju.

Generatorski objekti (ili generatori) implementiraju protokol `iterator`. U stvari, generatori su `lenji iteratori`. Stoga, da biste izvršili funkciju generatora, pozivate `next()` ugrađenu funkciju na objektu generatora.

#### Jednostavan primer generatora u Pajtonu

Pogledajte sledeći primer:

```py
def greeting():
    print('Hi!')
    yield 1
    print('How are you?')
    yield 2
    print('Are you there?')
    yield 3
```

Pošto `greeting()` funkcija sadrži `yield` naredbu, ona je `funkcija generatora`.

Naredba `yield` je kao `return` naredba u regularnoj funkciji. Međutim, postoji velika razlika. Kada Pajton naiđe na `yield` naredbu, vraća vrednost navedenu u `yield` naredbi. Pored toga, pauzira izvršavanje funkcije.

Ako ponovo "pozovete" istu funkciju, Pajton će nastaviti od mesta `yield` naredbe.

Kada pozovete `funkciju generatora`, ona vraća `objekat generatora`. Na primer:

```py
messenger = greeting()
```

`messenger` je objekat `generator` koji je takođe i `iterator`.

Da biste izvršili telo funkcije `greeting()`, potrebno je da koristite `next()` ugrađenu funkciju:

```py
result = next(messenger)
print(result)
```

Kada se `greeting()` funkcija izvrši, ona prikazuje prvu poruku Hi! i vraća 1:

```shell
Hi!
1
```

Takođe, pauzira se odmah kod prve `yield` izjave. Ako ponovo "pozovete" `greeting()` funkciju, ona će nastaviti izvršavanje od poslednje `yield` izjave:

```py
result = next(messenger)
print(result)
```

Izlaz:

```shell
How are you?
2
```

I možete ga ponovo pozvati:

```py
result = next(messenger)
print(result) 
```

Izlaz:

```shell
Are you there?
3
```

Međutim, ako ponovo izvršite generator, izazvaće izuzetak `StopIteration` jer je u pitanju iterator:

```py
next(messenger)
```

```shell
Error:
StopIteration      
```

Evo celog programa:

```py
def greeting():
    print('Hi!')
    yield 1
    print('How are you?')
    yield 2
    print('Are you there?')
    yield 3

messenger = greeting()

result = next(messenger)
print(result)

result = next(messenger)
print(result)

result = next(messenger)
print(result)

print(result)
```

Izlaz:

```shell
Hi!
1
How are you?
2
Are you there?
3
3
```

#### Korišćenje generatora za kreiranje iteratora

Sledeći primer definiše iterator koji vraća kvadrat celog broja.

```py
class Squares:
    def __init__(self, length):
        self.length = length
        self.current = 0

    def __iter__(self):
        return self

    def __next__(self):
        result = self.current `` 2

        self.current += 1

        if self.current > self.length:
            raise StopIteration

        return result
```

Možete koristiti `Squares` iterator da generišete kvadratne brojeve prvih 5 celih brojeva od 0 do 5:

```py
length = 5
square = Squares(length)
for s in square:
    print(s)
```

Ovaj kod radi kako smo očekivali. Ali ima jedan problem, a to je da ima mnogo šablonskog načina rada.

I kao što možete pretpostaviti, koristite funkciju generatora da biste izgradili taj iterator.

Sledeće prepisuje `Squares` iterator kao `funkciju generatora`:

```py
def squares(length):
    for n in range(length):
        yield n `` 2
```

Novi kod je mnogo kraći i izražajniji. Upotreba `squares funkcije generatora` je slična gore navedenom iteratoru:

```py
length = 5
square = squares(length)
for s in square:
    print(s)
```

#### Rezime generatora

- Pajton `generatori` su funkcije koje sadrže barem jednu naredbu `yield`.
- `Funkcija generatora` vraća `objekat generatora`.
- `Objekat generatora` je `iterator`. Stoga se iscrpljuje kada nema elemenata za vraćanje.

### Izrazi generatora

#### Uvod u izraze generatora

`Izraz generatora` je izraz koji vraća `objekat generatora`.

U osnovi, `funkcija generatora` je funkcija koja sadrži `yield` naredbu i vraća `objekat generatora`.

Na primer, sledeće definiše funkciju generatora:

```py
def squares(length):
    for n in range(length):
        yield n `` 2
```

`Funkcija generatora` `squares` vraća `objekat generatora` koji proizvodi kvadrate celih brojeva od 0 do ( length - 1 ).

Pošto je `objekat generatora iterator`, možete koristiti petlju `for` da biste iterirali kroz njegove elemente:

```py
def squares(length):
    for n in range(length):
        yield n `` 2

for square in squares(5):
    print(square)
```

Izlaz:

```shell
0
1
4
9
16
```

`Izraz generatora` vam pruža jednostavniji način za vraćanje `objekta generatora`.

Sledeći primer definiše izraz generatora koji vraća kvadrate celih brojeva od 0 do 4:

```py
squares = (n `` 2 for n in range(5))
```

Pošto je `squares` `objekat generatora`, možete iterirati kroz njegove elemente ovako:

```py
for square in squares:
    print(square)
```

Kao što vidite, umesto korišćenja funkcije za definisanje `funkcije generatora`, možete koristiti `izraz generatora`.

`Izraz generatora` je sličan `listi razumevanja` u smislu sintakse. Na primer, `izraz generatora` takođe podržava složene sintakse, uključujući:

- `if` naredbe
- Više ugnježđenih petlji
- Ugnežđena razumevanja

Međutim, izraz generatora koristi male () umesto uglastih zagrada [].

#### Izrazi generatora u odnosu na liste razumevanja

Sledeće je prikazano kako se koristi `lista razumevanja` za generisanje kvadrata brojeva od 0 do 4:

```py
square_list = [n `` 2 for n in range(5)]
```

I ovo definiše generator kvadrata brojeva:

```py
square_generator = (n `` 2 for n in range(5))
```

``Sintaksa``
Što se tiče sintakse, izraz generatora koristi zagrade () dok razumevanje liste koristi uglaste zagrade [].

``Iskorišćenost memorije``
Razumevanje liste vraća listu, dok izraz generatora vraća objekat generatora. To znači da razumevanje liste vraća kompletnu listu elemenata unapred. Međutim, izraz generatora vraća listu elemenata, jedan po jedan, na osnovu zahteva.

Razumevanje liste je brzo, dok je izraz generatora lenj. Drugim rečima, razumevanje liste kreira sve elemente odmah i učitava ih sve u memoriju. Suprotno tome, izraz generatora kreira jedan element na osnovu zahteva. On učitava samo jedan element u memoriju.

``Izraz generatora naspram iteratora``
Razumevanje liste vraća iterabilnu vrednost. To znači da možete iterirati preko rezultata razumevanja liste iznova i iznova. Međutim, izraz generatora vraća iterator, tačnije lenji iterator koji postaje iscrpljen kada završite sa iteracijom preko njega.

#### Rezime generatorskog izraza

Koristite izraz generatora u da biste vratili generator.

## Menadžeri konteksta

### Uvod u menadžere konteksta

`Menadžer konteksta` je objekat koji definiše kontekst izvršavanja unutar `with` naredbe.

Počnimo sa jednostavnim primerom da bismo razumeli koncept menadžera konteksta. Pretpostavimo da imate datoteku pod nazivom `data.txt` koja sadrži ceo broj 100.

Sledeći program čita datoteku `data.txt`, pretvara njen sadržaj u broj i prikazuje rezultat na standardnom izlazu:

```py
f = open('data.txt')
data = f.readlines()

# convert the number to integer and display it
print(int(data[0]))

f.close()
```

Kod je jednostavan i jasan.

Međutim, `data.txt` može sadržati podatke koji se ne mogu konvertovati u broj. U ovom slučaju, kod će rezultirati izuzetkom.

Na primer, ako `data.txt` sadrži string '100' umesto broja 100, dobićete sledeću grešku:

```py
ValueError: invalid literal for int() with base 10: "'100'"
```

Zbog ovog izuzetka, Pajton možda neće pravilno zatvoriti datoteku.

Da biste ovo popravili, možete koristiti sledeću `try...except...finally` izjavu:

```py
try:
    f = open('data.txt')
    data = f.readlines()
    # convert the number to integer and display it
    print(int(data[0]))
except ValueError as error:
    print(error)
finally:
    f.close()
```

Pošto se kod u `finally` bloku uvek izvršava, kod će uvek pravilno zatvoriti datoteku.

Ovo rešenje funkcioniše kako se očekuje. Međutim, prilično je opširno.

Stoga, Pajton vam pruža bolji način koji vam omogućava da automatski zatvorite datoteku nakon što završite njenu obradu.

Tu dolaze do izražaja `menadžeri konteksta`.

Sledeće je prikazano kako se koristi menadžer konteksta za obradu `data.txt` datoteke:

```py
with open('data.txt') as f:
    data = f.readlines()
    print(int(data[0])    
```

U ovom primeru koristimo `open()` funkciju sa `with` izrazom . Nakon `with` bloka, Pajton će automatski zatvoriti/osloboditi sve resurse.

### Pajton sa izrazom

Evo tipične sintakse izjave with:

```py
with context as ctx:
    # use the the object 

# context is cleaned up
```

Kako to funkcioniše.

- Kada Pajton naiđe na `with` izraz, kreira novi kontekst. Kontekst može opciono vratiti object.
- Nakon `with` bloka, Pajton automatski čisti kontekst.
- Opseg `ctx` ima isti opseg kao i `with` naredba. To znači da možete pristupiti `ctx` i unutar i nakon `with` naredbe.

Sledeće je prikazano kako pristupiti promenljivoj `f` nakon `with` izraza:

```py
with open('data.txt') as f:
    data = f.readlines()
    print(int(data[0]))

print(f.closed)  # True
```

### Protokol za upravljanje kontekstom

Pajton menadžeri konteksta rade na osnovu protokola za menadžer konteksta.

Protokol menadžera konteksta ima sledeće metode:

- `__enter__()` – podesi kontekst i opciono vrati neki objekat
- `__exit__()` – očisti objekat.

Ako želite da klasa podržava protokol menadžera konteksta, potrebno je da implementirate ove dve metode.

Pretpostavimo da je `ContextManager` klasa koja podržava protokol menadžera konteksta.

Sledeće je prikazano kako se koristi `ContextManager` klasa:

```py
with ContextManager() as ctx:
    # do something
# done with the context
```

Kada koristite `ContextManager` klasu sa `with` naredbom, Pajton implicitno kreira instancu klase `ContextManager` i automatski poziva `__enter__()` metodu na toj instanci.

Metoda `__enter__()` može opciono vratiti objekat. Ako je to slučaj, Pajton dodeljuje vraćenom objektu `ctx`.

Obratite pažnju da se `ctx` poziva na objekat koji vraća metoda `__enter__()`. Ne poziva se na instancu klase `ContextManager`.

Ako se izuzetak desi unutar bloka `with` ili nakon `with` bloka, Pajton poziva `__exit__`() metodu na objektu instance.

Funkcionalno, `with` naredba je ekvivalentna sledećoj `try...finally` naredbi:

```py
instance = ContextManager()
ctx = instance.__enter__()

try:
    # do something with the txt
finally:
    # done with the context
    instance.__exit__()
```

Metoda `__enter__`

U `__enter__()` metodi možete izvršiti potrebne korake za podešavanje konteksta.Opciono, možete vratiti objekat iz `__enter__()` metode.

Metoda `__exit__`

Pajton uvek izvršava `__exit__()` metodu čak i ako se u bloku `with` pojavi izuzetak.

Metoda `__exit__()` prihvata tri argumenta: tip izuzetka, vrednost izuzetka i objekat trasiranja. Svi ovi argumenti će biti None ako se ne dogodi izuzetak.

```py
def __exit__(self, ex_type, ex_value, ex_traceback):
    ...
```

Metod `__exit__()` vraća bulovsku vrednost, ili `True` ili `False`.

Ako je povratna vrednost `True`, Python će svaki izuzetak učiniti nečujnim. U suprotnom, neće se izuzetak isključiti.

### Aplikacije za upravljanje kontekstom u Pajtonu

Kao što vidite iz prethodnog primera, uobičajena upotreba menadžera konteksta je automatsko otvaranje i zatvaranje datoteka.

Međutim, možete koristiti menadžere konteksta u mnogim drugim slučajevima:

1) `Otvori – Zatvori`

    Ako želite da automatski otvorite i zatvorite resurs, možete koristiti menadžer konteksta. Na primer, možete otvoriti soket i zatvoriti ga pomoću `menadžera konteksta.

2) `Zaključaj – otpusti`

    Menadžeri konteksta mogu vam pomoći da efikasnije upravljate zaključavanjima objekata. Oni vam omogućavaju da automatski postavite zaključavanje i otključate ga.

3) `Start – stop`
    Menadžeri konteksta vam takođe pomažu da radite sa scenarijem koji zahteva faze pokretanja i zaustavljanja.

    Na primer, možete koristiti menadžer konteksta da biste pokrenuli tajmer i automatski ga zaustavili.

4) `Promena – resetovanje`

    Menadžeri konteksta mogu da rade sa scenarijima promena i resetovanja.

    Na primer, vaša aplikacija treba da se poveže sa više izvora podataka. I ima podrazumevanu vezu. Da biste se povezali sa drugim izvorom podataka:

    - Prvo, koristite menadžer konteksta da biste promenili podrazumevanu vezu na novu.
    - Drugo, radite sa novom vezom

    Sledeći kod prikazuje jednostavnu implementaciju funkcije `open()` korišćenjem protokola menadžera konteksta:

    ```py
    class File:
        def __init__(self, filename, mode):
            self.filename = filename
            self.mode = mode

        def __enter__(self):
            print(f'Opening the file {self.filename}.')
            self.__file = open(self.filename, self.mode)
            return self.__file

        def __exit__(self, exc_type, exc_value, exc_traceback):
            print(f'Closing the file {self.filename}.')
            if not self.__file.closed:
                self.__file.close()

            return False

    with File('data.txt', 'r') as f:
        print(int(next(f)))
    ```

    Kako to funkcioniše.

    - Prvo, inicijalizujte filenamei mode u `__init__()` metodi.
    - Drugo, otvorite datoteku u `__enter__()` metodi i vratite objekat datoteke.
    - Treće, zatvorite datoteku ako je otvorena u `__exit__()` metodi.

5) `Pokretanja i zaustavljanja`

    Sledeći kod definiše Timer klasu koja podržava protokol menadžera konteksta:

    ```py
    from time import perf_counter

    class Timer:
        def __init__(self):
            self.elapsed = 0

        def __enter__(self):
            self.start = perf_counter()
            return self

        def __exit__(self, exc_type, exc_value, exc_traceback):
            self.stop = perf_counter()
            self.elapsed = self.stop - self.start
            return False
    ```

    Kako to funkcioniše.

    - Prvo, uvezite `perf_counter` iz `time` modula.
    - Drugo, pokrenite tajmer u `__enter__()` metodi
    - Treće, zaustavite tajmer u `__exit__()` metodi i vratite proteklo vreme.

    Sada možete koristiti `Timer` klasu da izmerite vreme potrebno za izračunavanje Fibonačijevog broja od 1000, milion puta:

    ```py
    def fibonacci(n):
        f1 = 1
        f2 = 1
        for i in range(n-1):
            f1, f2 = f2, f1 + f2

        return f1


    with Timer() as timer:
        for _ in range(1, 1000000):
            fibonacci(1000)

    print(timer.elapsed)
    ```

### Rezime kontekst menadžera

- Koristite Pajton `kontekstne menadžere` da biste definisali kontekste izvršavanja prilikom izvršavanja u `with` naredbi.
- Implementirajte `__enter__()` i `__exit__()` metode za podršku protokolu menadžera konteksta.

## Decimal modul

### Uvod u decimal modul

Mnogi decimalni brojevi nemaju tačne reprezentacije u binarnom sistemu sa pokretnim zarezom, kao što je 0,1. Kada koristite ove brojeve u aritmetičkim operacijama, dobićete rezultat koji ne biste očekivali. Na primer:

```py
x = 0.1
y = 0.1
z = 0.1

s = x + y + z

print(s)
```

Izlaz:

```shell
0.30000000000000004
```

Rezultat je 0,30000000000000004, a ne 0,3.

Da biste rešili ovaj problem, koristite `Decimal` klasu iz `decimal` modula na sledeći način:

```py
import decimal
from decimal import Decimal

x = Decimal('0.1')
y = Decimal('0.1')
z = Decimal('0.1')

s = x + y + z

print(s)
```

Izlaz:

```shell
0.3
```

Izlaz je kao što se očekivalo.

Pajton `decimal` modul podržava aritmetiku koja funkcioniše isto kao aritmetika koju učite u školi.

Za razliku od float-a , Pajton tačno predstavlja decimalne brojeve. I tačnost se prenosi i na aritmetiku. Na primer, sledeći izraz vraća tačno 0,0:

```py
Decimal('0.1') + Decimal('0.1') + Decimal('0.1') - Decimal('0.3')
```

### Decimalni kontekst

Decimalni broj se uvek povezuje sa kontekstom koji kontroliše sledeće aspekte:

- Preciznost tokom aritmetičke operacije
- Algoritam za zaokruživanje

Podrazumevano, kontekst je globalan. Globalni kontekst je podrazumevani kontekst. Takođe, možete podesiti privremeni kontekst koji će stupiti na snagu lokalno bez uticaja na globalni kontekst.

Da biste dobili podrazumevani kontekst, pozovete `getcontext()` funkciju iz `decimal` modula:

```py
decimal.getcontext()
```

Funkcija `getcontext()` vraća podrazumevani kontekst, koji može biti globalni ili lokalni.

Da biste kreirali novi kontekst kopiran iz drugog konteksta, koristite `localcontext()` funkciju:

```py
decimal.localcontext(ctx=None)
```

`localcontext()` vraća novi kontekst kopiran iz konteksta `ctx` ako je naveden.

Kada dobijete kontekstni objekat, možete pristupiti preciznosti i rutiranju preko svojstva `pre`, `rounding` respektivno:

- `ctx.pre`: preuzimanje ili podešavanje preciznosti. ctx.pre je ceo broj koji je podrazumevano 28
- `ctx.rounding`: preuzima ili postavlja mehanizam zaokruživanja. Zaokruživanje je string. Podrazumevana vrednost je 'ROUND_HALF_EVEN'. Zabeležene brojeve sa pokretnim plutajućim sklopom takođe koriste ovaj mehanizam zaokruživanja.

Pajton pruža sledeće mehanizme zaokruživanja:

Zaokruživanje   | Opis
----------------|----------------------
ROUND_UP        | zaokruženo od nule
ROUND_DOWN      | zaokruženo prema nuli
ROUND_CEILING   | zaokruženo do max (prema pozitivnoj beskonačnosti)
ROUND_FLOOR     | zaokruženo do min (prema negativnoj beskonačnosti)
ROUND_HALF_UP   | zaokruženo na najbliže, vezano dalje od nule
ROUND_HALF_DOWN | zaokruženo na najbliže, vezano za nulu
ROUND_HALF_EVEN | zaokruženo na najbliži, vezati za parni broj (najmanja značajna cifra)

Ovaj primer ilustruje kako dobiti podrazumevanu preciznost i zaokruživanje podrazumevanog konteksta:

```py
import decimal

ctx = decimal.getcontext()

print(ctx.prec)
print(ctx.rounding)
```

Izlaz:

```shell
28
ROUND_HALF_EVEN
```

Sledeći primer pokazuje kako 'ROUND_HALF_EVEN' mehanizam zaokruživanja stupa na snagu:

```py
import decimal
from decimal import Decimal


x = Decimal('2.25')
y = Decimal('3.35')

print(round(x, 1))
print(round(y, 1))
```

Izlaz:

```shell
2.2
3.4
```

Ako promenite zaokruživanje na 'ROUND_HALF_UP', dobićete drugačiji rezultat:

```py
import decimal
from decimal import Decimal


ctx = decimal.getcontext()
ctx.rounding = decimal.ROUND_HALF_UP

x = Decimal('2.25')
y = Decimal('3.35')

print(round(x, 1))
print(round(y, 1))
```

Izlaz:

```shell
2.3
3.4
```

Sledeći primer vam pokazuje kako da kopirate podrazumevani kontekst i promenite zaokruživanje na 'ROUND_HALF_UP':

```py
import decimal
from decimal import Decimal

x = Decimal('2.25')
y = Decimal('3.35')

with decimal.localcontext() as ctx:
    print('Local context:')
    ctx.rounding = decimal.ROUND_HALF_UP
    print(round(x, 1))
    print(round(y, 1))

print('Global context:')
print(round(x, 1))
print(round(y, 1))
```

Izlaz:

```shell
Local context:
2.3
3.4
Global context:
2.2
3.4
```

Obratite pažnju da lokalni kontekst ne utiče na globalni kontekst. Nakon bloka with, Pajton koristi podrazumevani mehanizam zaokruživanja.

### Decimal konstruktor

Konstruktor `Decimal` vam omogućava da kreirate novi `Decimal` objekat na osnovu vrednosti:

```py
Decimal(value='0', context=None)
```

Argument `value` može biti ceo broj, string, torka, broj sa pokretnim zamahom ili drugi objekat tipa Decimal. Ako ne navedete argument vrednosti, podrazumevano se koristi '0'.

Ako je vrednost torka, trebalo bi da ima tri komponente:

- znak (0 za pozitivnu ili 1 za negativnu),
- torku cifara i
- celobrojni eksponent:

(sign, (digit1,digit2, digit3,...), exponent)

Na primer:

```py
3.14 = 314 x 10^-2
```

Torka ima tri elementa kao što sledi:

- znak je 0
- cifre su (3,1,4)
- eksponent je -2

Stoga, konstruktoru ćete morati da prosledite sledeći tupl Decimal:

```py
import decimal
from decimal import Decimal

x = Decimal((0, (3, 1, 4), -2))
print(x)
```

Izlaz:

```shell
3.14
```

Obratite pažnju da preciznost decimalnog konteksta utiče samo na aritmetičku operaciju, a ne na `Decimal` konstruktor. Na primer:

```py
import decimal
from decimal import Decimal

decimal.getcontext().prec = 2

pi = Decimal('3.14159')
radius = 1

print(pi)

area = pi ` radius ` radius
print(area)
```

Kada koristite broj sa pokretnim zarezom (float) koji nema tačnu binarnu reprezentaciju sa pokretnim zarezom, `Decimal` konstruktor ne može da kreira tačnu decimalnu reprezentaciju. Na primer:

```py
import decimal
from decimal import Decimal

x = Decimal(0.1)
print(x)
```

Izlaz:

```shell
0.1000000000000000055511151231257827021181583404541015625
```

U praksi, koristićete string ili torku da biste konstruisali Decimal.

### Decimalne aritmetičke operacije

Neki aritmetički operatori ne funkcionišu isto kao sa brojevima sa pokretnim zarezom ili celim brojevi, kao što su div ( // ) i mod( % ).

Za decimalne brojeve, // operator vrši skraćeno deljenje:

```py
x // y = trunc( x / y)
```

Klasa `Decimal` pruža neke matematičke operacije kao što su `sqrt` i `log`. Međutim, nema sve funkcije definisane u `math` modulu.

Kada koristite funkcije iz `math` modula za decimalne brojeve, Pajton će konvertovati `Decimal` objekte u brojeve sa pokretnim zarezom pre nego što izvrši aritmetičke operacije. Ovo dovodi do gubitka preciznosti ugrađene u decimalne objekte.

### Rezime decimal modula

- Koristite Pajton `decimal` modul kada želite da podržite brzu, ispravno zaokruženu aritmetiku sa pokretnim zarezom.
- Koristite `Decimal` klasu iz `decimal` modula za kreiranje objekta Decimal od stringova, celih brojeva i torki.
Brojevi `Decimal` imaju kontekst koji kontroliše mehanizam preciznosti i zaokruživanja.
- Klasa `Decimal` nema sve metode definisane u `math`modulu. Međutim, trebalo bi da koristite aritmetičke metode klase `Decimal` ako su dostupne.


[Nazad](django_orm.md#q-objects)

### F() izrazi i Q() objekti

 U Django-ovom objektno-relacionom mapperu (ORM), `Q()` i `F()` su moćni alati za izgradju složenih i efikasnih upita baze podataka.

#### Q() objekti

**Svrha**: `Q()` objekti, uvezeni iz `django.db.models`, omogućavaju izgradju složenih uslova upita koristeći logičke operatore kao što su `OR` ( `|` ), `AND` ( `&` ) i `NOT` ( `~`).

**Upotreba**: Oni obuhvataju argumente kjučnih reči za filtere, omogućavajući uslove koji prevazilaze jednostavne operacije i koje su svojstvene standardnim filter () pozivima.

##### Primer Q objekata

```py
from django.db.models import Q

# Query for objects where 'name' is 'Alice' OR 'age' is 30
Model.objects.filter(Q(name='Alice') | Q(age=30))

# Query for objects where 'status' is 'active' AND NOT 'is_deleted'
Model.objects.filter(Q(status='active') & ~Q(is_deleted=True))
```

#### F() izrazi

**Svrha**: `F()` izrazi, takođe uvezeni iz `django.db.models`, omogućavaju upućivaje na vrednosti poja modela direktno unutar upita baze podataka bez povlačeja u Python memoriju. Ovo omogućava operacije na nivou baze podataka kao što su poređenja, aritmetika i ažuriranja između polja.

**Upotreba**: Oni predstavjaju kolonu baze podataka ili izračunatu vrednost zasnovanu na drugim kolonama, omogućavajući vam da obavjate operacije direktno unutar upita baze podataka.

##### Primer F izraza

```py
from django.db.models import F

# Update the 'stock' of all products by adding 10
Product.objects.update(stock=F('stock') + 10)

# Filter products where 'price' is greater than 'cost'
Product.objects.filter(price__gt=F('cost'))
```

#### Kjučne razlike i zajedničko korišćenje

- Q() su za logiku, F() su za vrednosti

  Q() se fokusira na izgradju logičkih kombinacija uslova, dok se F() fokusira na manipulaciju ili upoređivaje vrednosti poja unutar Baze podataka.

- Kombinovanje

  Q() i F() mogu se koristiti zajedno za kreiraje visoko sofisticiranih upita, kao što je filtriraje zasnovano na logičkoj kombinaciji uslova gde jedan ili više uslova ukjučuju poređenja između poja koristeći F() izraze.

U suštini, `Q()` pruža logički okvir za vaše upite, dok `F()` pruža sredstva za interakciju i manipulaciju podacima na nivou baze podataka, što dovodi do efikasnijih i moćnijih Dango ORM operacija.

[Nazad](django_orm.md#q-objects)

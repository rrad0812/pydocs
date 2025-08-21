
# JavaScript u adminu

## Sadržaj

[Nazad](sadrzaj.md)

- [Događaji u inline formi](#događaji-u-inline-formi)
- [Kako raditi sa AJAX Request u Django](#kako-raditi-sa-ajax-request-u-django)
  - [Kako Ajax radi u Django](#kako-ajax-radi-u-django)
- [Pogled na Django](#pogled-na-django)
  - [Korišćenje jQuery za AJAX u Django](#korišćenje-jquery-za-ajax-u-django)
  - [Zašto koristiti jQuery](#zašto-koristiti-jquery)
  - [Inicijalni setup](#inicijalni-setup)
  - [AJAX Request implementacija](#ajax-request-implementacija)
  - [Najbolje prakse za AJAX JavaScript u Django-u](#najbolje-prakse-za-ajax-javascript-u-django-u)
- [Ajax request u Django adminu na edit strani](#ajax-request-u-django-adminu-na-edit-strani)
- [Izgradnja Ajax request-a od nule u admin-u](#izgradnja-ajax-request-a-od-nule-u-admin-u)

### Događaji u inline formi

Možda ćete želeti da pokrenete neki JavaScript kada se umetnuti obrazac (zapis) doda ili ukloni u obrascu promene admina. `formset:added` i `formset:removed` jQueri događaji to dozvoljavaju.

Rukovaocu događaja se prenose tri argumenta:

- `Event` je jQuery događaj.
- `$row` je novododani (ili uklonjeni) red (zapis).
- `formsetName` je skup obrazaca kome red pripada.

Događaj se pokreće pomoću prostora imena `django.jQuery`.

U prilagođenom `change_form.html` šablonu proširite `admin_change_form_document_ready` blok i dodajte kod:

`change_form.html`

```html
{% extends 'admin/change_form.html' %}
{% load static %}
{% block admin_change_form_document_ready %}
{{ block.super }}
<script src="{% static 'app/formset_handlers.js' %}"></script>
{% endblock %}
```

`app/static/app/formset_handlers.js`

```js
(function($) {
  $(document).on('formset:added',
    function(event, $row, formsetName){
      if (formsetName == 'author_set') {
        // Do something
      }
    }
  );
  $(document).on('formset:removed',
    function(event, $row, formsetName) {
      // Row removed
    });
})(django.jQuery);
```

Dve stvari koje treba imati na umu:

- JavaScript kod mora da se nalazi u bloku šablona ako nasleđujete admin/change_form.html ili se neće prikazati u konačnom HTML -u.
- {{ block.super }} se dodaje jer Djangov admin_change_form_document_ready blok sadržiJavaScript kod za rukovanje raznim operacijama u obrascu za promenu, pa nam je potrebno i to da se prikaže.

Ponekad ćete morati da radite sa jQuery dodacima koji nisu registrovani u prostoru django.jQuery imena. Da biste to uradili, promenite način na koji kod sluša događaje. Umesto da omotate slušaoca udjango.jQuery imenski prostor, poslušajte događaj koji se odatle pokrenuo. Na primer:

`change_form.html`

```html
{% extends 'admin/change_form.html' %}
{% load static %}
{% block admin_change_form_document_ready %}
{{ block.super }}
<script src="{% static 'app/unregistered_handlers.js' %}"></script>
{% endblock %}
```

`app/static/app/unregistered_handlers.js`

```js
django.jQuery(document).on('formset:added', function(event, $row, formsetName) {
  // Row added
});
django.jQuery(document).on('formset:removed', function(event, $row, formsetName) {
  // Row removed
});
```

[Sadržaj](#sadržaj)

### Kako raditi sa AJAX Request u Django

AJAX je akronim za Asynchronous JavaScript And XML. AJAX tehnologije omogućavaju web stranama da rade ažuriranje asinhronom razmenom podataka sa serverom. U suštini, web strana može biti ažurirana bez učitavanja kompletne web strane. To se ostvaruje preko XMLHttpRequest objekta za slanje i primanje podataka. Kada se događaj kao: slanje podataka forme, učitavanje strane ili kada je neko dugme kliknuto, na web strani dogodi, jedan XMLHttpRequest objekat je kreiran i poslan je zahtev na server. Server će odgovoriti na zahtev. Zahtevani odgovor je uhvaćen i web server odgovara sa zahtevanim podacima.

AJAX je koristan u scenarijima kada želite da napravite GET i POST zahtev za učitavanjem podataka sa servera asinhrono. To omogućava aplikacijama da budu dinamičnije i redukuje vreme učitavanja.

[Sadržaj](#sadržaj)

#### Kako Ajax radi u Django

- Javascript kod u Browser/client-strani
Brauzer podnosi zahtev kada se događaj dogodi na web stranici. JavaScript kod generiše XHR objekat koji sadrži JavaScript objekt ili podatke i šalje se kao objekt zahteva na webserver. XHR objekt će u osnovi  imenovati funkciju povratnog poziva na serveru ili URL-u.

- Zahtevom upravlja webserver
Kada je zahtev poslan, odgovarajuća funkcija povratnog poziva rukuje zahtjevom i ona će poslati uspeh ili neuspeh odgovor. Zbog asinhrone prirode zahteva, kod će se izvršiti bez prekida, a server obrađuje zahtev.

- Uspešan ili neuspešan odgovor
Uspešan odgovor će sadržati podatke u više formata poput teksta, JSON, XML.

- Stvarni primer
AJAKS ima veoma mnogo aplikacija na veb stranama i aplikacijama. Primer je taster sličan na Facebooku ili Linkedln. Kada se post lajkuje, nekoliko akcija se izvodi i na serveru i na klijentu. Na primer, boja tastera može se promeniti ili će se broj lajkova na postu biti uvećan u bazi podataka.

- Pojednostavljeni proces
Kada se klikne na lajk dugme, XHR objekat se šalje na server, server prima zahtev i vrši radnju kreiranu za zahtev. U ovom slučaju se registruje lajk za post. Server zatim šalje odgovor uspeha, a taster lajk menja boju ili je povećan broj lajkova ili oboje.

[Sadržaj](#sadržaj)

### Pogled na Django

Django je opisan kao web okvir za perfekcioniste sa rokovima i prevladava kao Pajton web okvir. Django se može okarakterisati kao MVC (Model Viev Controller) i MTV (Model Template View). Klase modela su u orm-u i instance odgovaraju redovima u tabeli. Šablon sistem je dizajniran za lakoću upotrebe i ograničava meru u kojoj se HTML koristi kroz Python izvorni kod. View je funkcija koja pravi odgovor uz pomoć šablona.

[Sadržaj](#sadržaj)

#### Korišćenje jQuery za AJAX u Django

JQuery je jedna od najpopularnijih JavaScript biblioteka koje su korišćene za razvijanje drugih okvira JavaScript-a poput React-a ili Angular-a. Omogućava funkcije sa DOM na web stranama.

[Sadržaj](#sadržaj)

#### Zašto koristiti jQuery

Ajax ima drugačiju implementaciju na više brouzera. Različiti pretraživači koriste razne XHR API-e koji analiziraju zahtev i odgovoraju neznatno različito.

JQuery vam pruža ugrađeno rešenje da ažurirate svoj kod iako je to što je učinio nezavisno. JQuery kod se aktivno razvija i stoga se ažurira.

**jQuery pruža različite metode korišćenja DOM-a**:

jQuery sadrži mnoge DOM objekte i srodne metode. Ovi se objekti mogu koristiti u zavisnosti od potreba programera, a JQuery ga čini pogodnijim za upotrebu.

**JQuery je osnova za mnoge popularne okvire**:

Okviri poput React JS-a, Angular JS zasnivaju se na jQuery-u. jQuery je praktično svuda u popularnim okvirima JavaScript. jQuery je veoma popularan.

**jQuery je viralna biblioteka među web programerima**:

JavaScript okviri pružaju gotove widgete, a okviri mogu vam dozvoliti da napišete JavaScript višeg nivoa i malo više pythonic. Postoje i drugi načini za upotrebu Ajax u Django-u, ali ovi razlozi čine JQuery popularnim izborom. Stoga ćemo ga koristiti da bismo demonstrirali kako raditi sa zahtevima u Django-u.

**Scenario registracija korisnika**:

Kao web programer, želite da validirate polje "User" "name" u registraciju ili u prikazu registracije, čim korisnik završi upisivanje korisničkog imena. Provera koju želite da napravite je jednostavna.

[Sadržaj](#sadržaj)

#### Inicijalni setup

Posle inicijalnog setup-a za Django, kreiraj jedan projekat i `base.html` fajl.

`base.html`

```html
{% load static %}
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>{% block title %}Default Title{% endblock %}</title>
  <link rel="stylesheet" type="text/css" href="{% static 'css/app.css' %}">
  {% block stylesheet %}
  {% endblock %}
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
  {% block javascript %}
  {% endblock %}

</body>
</html>
```

Primetite da jQuery biblioteka i JavaScript resursi trebaju biti smešteni na kraj HTML strane da garantuje da DOM će biti učitan kada je skript izvršen i da izbegne inlajn skriptove koji koriste jQuery.

`views.py`

```py
from django.contrib.auth.forms import UserCreationForm
from django.views.generic.edit import CreateView

class SignUpView(CreateView):
    template_name = 'core/signup.html'
    form_class = UserCreationForm
```

`url.py`

```py
from django.conf.urls import url
from core import views

urlpatterns = [
    path(' signup/', views.SignUpView.as_view(), name='signup')
]
```

`signup.html`

```html
{% extends 'base.html' %}
​
{% block content %}

<form method="post">
{% csrf_token %}
{{ form.as_p }}
<button type="submit">Sign up</button>

</form>
{​ % endblock %}
```

[Sadržaj](#sadržaj)

#### AJAX Request implementacija

Ispod implementacije AJAX-a je asinhroni zahtev da potvrdi da li je korisnički ulaz za korisničko ime već iskorišćen ili je dostupan za upotrebu. Prvi korak uključuje gledanje HTML-a generisan kao {{form.AS_P}} da bi pregledao polje za korisničko ime.

Da biste potvrdili korisničko ime, kreiramo listener za događaj promene
`id_username` polja.

`signup.html`

```html
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
```

U implementaciji iznad, događaj promene će se pojaviti svaki put kada se menja vrednost polja za korisničko ime. Zatim kreiramo pogled koji proverava da li je korisničko ime prihvaćeno i vraća JSON odgovor.

`pyviews.py`

```py
from django.contrib.auth.models import User
f rom django.http import JsonResponse

def validate_username (request):
    username = request.GET.get('username', None)
    data = {
        'is_taken': User.objects.filter(username__iexact=username).exists()
    }
    return JsonResponse(data)
```

Sledeći korak je dodavanje rute na prikaz (views.py).

`urls.py`

```py
from django.conf.urls import url
f​rom core import views

urlpatterns = [
    path(' signup/', views.SignUpView.as_view(), name='signup'),
    path(' ajax/validate_username/', , views.validate_username,
        name='validate_username'),
]
```

Na kraju, implementiramo AJAX:

`signup.html`

```html
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
```

Kod iznad je dodao događaj za rukovanje korisničkim unosom, koji potom naziva funkciju koja koristi AJAX da proveri da li je dato ime iskorišćeno i vraća odgovor.

[Sadržaj](#sadržaj)

#### Najbolje prakse za AJAX JavaScript u Django-u

- Izbegavajte modifikovanje JavaScript koda sa Python kodom.
- Držite URL reference u HTML-u i upravljajte korisničkim porukama u Pithon kodu
- Na HTML polju, preferirajte dodavanje ime klase.

Nakon što dodate ime klase, možete da priključite događaj za promenu na klasu `js-validite-username`. Prefiks JS na imenu klase sugerira na JavaScript kod koji komunicira sa ovim elementom.

Obrasci su u opasnosti od Cross-Site Request Forgeries (CSRF). Da biste sprečili napade CSRF-a, dodajte {% csrf_token%} šablon oznaku u obrazac. To će dodati polje skrivenog unosa koji sadrži token poslat sa svakim POST zahtevom.

[Sadržaj](#sadržaj)

### Ajax request u Django adminu na edit strani

U ovom tutorijalu učimo kako kreirati Ajax request za validaciju nekih podataka dok editujemo podatke u Django admin edit formi.

#### Izgradnja Ajax request-a od nule u admin-u

Koristimo sledeći model:

```py
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
```

Recimo da bi smo, kada dodajemo ili editujemo model Product, voleli da verifikujemo cenu unešenu od strane korisnika u odredjenom rangu vrednosti. Da bi to uradili treba da osluškujemo (korišćenjem javascript-a) ’price’ ulaznu liniju, i ako je vrednost unešena, uradimo jedan Ajax zahtev da proverimo da li je cena u odgovarajućem rangu.

Prvo moramo da prepišemo podrazumevani change_form.html fajl admina, prvo kreiramo novi direktorijum `<application>/templates/admin/<application>/product/`
I kopiramo unutra fajl podrazumevani change_form.html fajl iz našeg virtualenv cp .../site-packages/django/contrib/admin/templates/admin/change_form.html
Pre modifikacije tog fajla, pogledajmo izvorni kod strane, da bi znali kako je ’price’ polje imenovano:

```html
<div class="form-row field-price">
<div>

<label class="required" for="id_price">Price:</label>
<input type="number" name="price" value="2.0" step="any" required id="id_price">

</div>
</div>
```

Kao što smo očekivali ulazno polje je idetifikovano sa “id_price”. Sada modifikujemo dati fajl dodavanjem javascript koda unutar pratećeg bloka:

```html
{% block admin_change_form_document_ready %}
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script> <script>
  django.jQuery(document).ready(function(){
    django.jQuery("#id_price").change(function(){
      console.log("Price change");
    });
  })
</script>
```

Pre implementacije Ajax request-a, mi samo logujemo u javascript konzolu promenu cene, da proverimo je sve OK. Ako je sve OK. Sada možemo implementirati Ajax poziv:

```js
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
    success: function(data) { // `data` is the return of the `checkPrice` view
      // function
    console.log(data);
    }
  });
  });
})
</script>
```

Dobili smo cenu kao $(this).val(); i potom smo konstruisali naš url na kome ćemo napraviti poziv. Sada, ako reučitamo našu stranu i pokušamo da promenimo cenu, videćemo na javascript konzolnom logu:

Imamo 404 error page što je logično jer još nismo implementirali checkPrice prikaz. Uradimo to.

U views.py fajlu aplikacije, kreirajmo jednostavnu funkciju prikaza da proverimo vrednost cene:

```PY
from django.http import JsonResponse

def checkPrice(request):
  price = request.GET['price']
  if int(price) > 10:
    data = {"status": "KO"}
   else:
     data = {"status":"OK"}
   return JsonResponse(data)
```

Do vas je da u prikazu implementirate vaša biznis pravila. Za ovaj primer, proverićemo da li je cena veća od 10 i i vraćamo nazad JSON sa KO ako nije ili OK statusom ako jeste.

Potom u našem backoffice direktorijumu, kreirajmo urls.py fajl:

```py
#coding=utf-8
from django.conf.urls import url
from backoffice.views import *

urlpatterns = [
    url(r'^backoffice/checkPrice/$', checkPrice, name='checkPrice'),
]
```

Na kraju deklarišemo novu rutu unutar projektnog urls.py fajla:

```PY
from django.conf.urls import url,include
from django.contrib import admin
from backoffice.admin import

site admin.site = site admin.autodiscover()
urlpatterns = [
    url('', include('backoffice.urls')),url(r'^admin/', admin.site.urls),
]
```

Sada, ako reučitate stranu i pokušate da promenite cenu, videćete u konzolnom logu Ajax odgovor sa statusom. Na vama je šta ćete da uradite sa Ajax odgovorom. Na primer, proverićemo da li je status OK i ako jeste postavićemo cenu na 10 u input boksu.

```JS
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
    success: function (data) {// `data` is the return of the `checkPrice` view
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
```

Sada, ako reučitate admin stranu i pokušate da unesete vrednost veću od 10 za cenu, vrednost će automatski biti postavljena na 10, zahvaljujući Ajax pozivu.


# Dodavanje grafikona u Django sa grafikonom.js

U ovom tutorijalu ćemo pogledati kako da dodamo grafikone u Django sa `Charts.js`. Koristićemo Django da modelira i pripremi podatke, a zatim ih doneti asinhrono iz šablona pomoću Ajaksa. Napokon, pogledaćemo kako da kreiramo novi Django Admin prikaz i proširimo postojeće admin šablone kako bi dodali grafikone na Django admin.

## Sadržaj

- [Šta je Chart.js](#šta-je-chartsjs)
- [Podešavanje projekta](#podešavanje-projekta)
- [Kreiranje db modela](#kreiranje-db-modela)
- [Popunjavanje baze podataka](#popunjavanje-baze-podataka)
- [Priprema i serviranje podataka](#priprema-i-serviranje-podataka)
- [Prikazi](#prikazi)
- [Url-ovi](#url-ovi)
- [Testiranje](#testiranje)
- [Kreiranje grafikona sa Chart.js](#kreiranje-grafikona-sa-chartjs)
- [Dodavanje grafikona na Django Admin](#dodavanje-grafikona-na-django-admin)
  - [Kreiranje novog Django Admin prikaza](#kreiranje-novog-django-admin-prikaza)
    - [Kreiranje AdminConfig unutar core/apps.py](#kreiranje-adminconfig-unutar-coreappspy)

## Šta je Charts.js?

`Charts.Js` je JavaScript biblioteka otvorenog koda za vizuelizaciju podataka.Podržava osam različiti tipovi grafikona:

- bar,
- linija,
- površina,
- pita,
- mehurić,
- radar,
- polarni i
- rasuti.

Fleksibilna je i veoma prilagodljiva.Podržava animacije. Dodajte najbolje
od svega, lako je koristiti.

Da biste započeli, samo morate da unesete skriptu Charts.js zajedno sa
`<canvas>` nodom u HTML fajl:

```js
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.2.1/dist/ chart.umd.
    min.js"></script>
<canvas id="chart" width="500" height="500"></canvas>
```

Sada možemo kreirati grafikon:

```js
let ctx = document.getElementById("chart").getContext("2d");

let chart = new Chart(ctx, {
  type: "bar",
  data: {
    labels: ["2020/Q1", "2020/Q2", "2020/Q3", "2020/Q4"],
    datasets: [
      {
        label: "Gross volume ($)",
        backgroundColor: "#79AEC8",
        borderColor: "#417690",
        data: [26900, 28700, 27300, 29200]
      }
    ]
  },
  options: {
    title: {
    text: "Gross Volume in 2020",
    display: true
    }
  }
});
```

Ovaj kod kreira grafikon koji možete videti na: [Code Pen linku](https://codepen.io/mjhea0/pen/wvogmJy)

Pogledajmo kako da dodamo grafikone u Django sa Charts.js.

## Podešavanje projekta

Stvorimo jednostavnu aplikaciju za kupovinu. Generišećemo podatke o uzorku koristeći Django naredbu `management` i zatim ga vizualizujmo sa `Charts.js`.

Ako preferirate različitu biblioteku JavaScript Chart-a poput `D3.js` ili `Chartist`, Možete da koristite isti pristup za dodavanje grafikona u Django i Django Admin. Samo ćete morati da prilagodite kako se podaci formatiraju na krajnjim tačkama JSON-a.

Workflow:

1. Nabavite podake koristeći `Django Orm` upite.
2. Formatirajte podatke i vratite ih preko zaštićene krajnje tačke.
3. Zatražite podatke iz šablona pomoću `Ajax`.
4. Inicijalizirajte `Chart,js` i učitajte podatke.

Započnimo stvaranjem novog direktorijuma i postavljanjem novog projekta Django:

```shell
$ mkdir django-interactive-charts && cd django-interactive-charts
$ python3.11 -m venv env
$ source env/bin/activate

(env)$ pip install django==4.2
(env)$ django-admin startproject core .
```

Sada kreiramo novu aplikaciju `shop`:

```shell
(env)$ python manage.py startapp shop
```

Registrujemo aplikaciju u `core/settings.py` ispod `INSTALLED_APPS` :

`# core/settings.py`

```py
INSTALLED_APPS = [
  "django.contrib.admin",
  "django.contrib.auth",
  "django.contrib.contenttypes",
  "django.contrib.sessions",
  "django.contrib.messages",
  "django.contrib.staticfiles",
  "shop.apps.ShopConfig", # new
]
```

## Kreiranje db modela

Zatim napravimo modele stavki i kupovine:

`shop/models.py`

```py
from django.db import models

class Item(models.Model):
  name = models.CharField(max_length=255)
  description = models.TextField(null=True)
  price = models.FloatField(default=0)

  def __str__(self):
    return f"{self.name} (${self.price})"

class Purchase(models.Model):
  customer_full_name = models.CharField(max_length=64)
  item = models.ForeignKey(to=Item, on_delete=models.CASCADE)
  PAYMENT_METHODS = [
    ("CC", "Credit card"),
    ("DC", "Debit card"),
    ("ET", "Ethereum"),
    ("BC", "Bitcoin"),
  ]
  payment_method = models.CharField(max_length=2, default="CC",
    choices=PAYMENT_METHODS)
  time = models.DateTimeField(auto_now_add=True)
  successful = models.BooleanField(default=False)

  class Meta:
    ordering = ["-time"]

  def __str__(self):
    return f"{self.customer_full_name}, {self.payment_method}
    ({self.item.name})"
```

1. Stavka predstavlja prodajni predmet u našoj prodavnici
2. Kupovina predstavlja kupovinu prodajnog predmeta ili stavke.

Napravite migracije, a zatim ih primenite:

```shell
(env)$ python manage.py makemigrations
(env)$ python manage.py migrate
```

Registrujte modele u `shop/admin.py`:

`shop/admin.py`

```py
from django.contrib import admin
from shop.models import Item, Purchase

admin.site.register(Item)
admin.site.register(Purchase)
```

## Popunjavanje baze podataka

Pre stvaranja bilo kojeg grafikona, trebaju nam neki podaci.Stvorio sam jednostavnu komandu koju možemo da koristimo da popunjava bazu podataka.

Kreirajte novi direktorijum u "shop" direktorijumu sa imenom "management", a onda u tom direktorijumu kreirajte drugi direktorijum sa imenom "commands". Unutar  "commands" direktorijuma kreirajte novi fajl sa imenom `populate_db.py`.

```shell
management
└── commands
  └── populate_db.py
```

`shop/management/commands/populate_db.py`

```py
import random
from datetime import datetime, timedelta
import pytz
from django.core.management.base import BaseCommand
from shop.models import Item, Purchase

class Command(BaseCommand):
  help = "Populates the database with random generated data."
  
  def add_arguments(self, parser):
    parser.add_argument("--amount", type=int, help="The number of 
    purchases that should be created.")
  
  def handle(self, *args, **options):
    names = ["James", "John", "Robert", "Michael", "William", "David",
    "Richard", "Joseph", "Thomas", "Charles"]
    surname = ["Smith", "Jones", "Taylor", "Brown", "Williams",
    "Wilson", "Johnson", "Davies", "Patel", "Wright"]
    items = [
      Item.objects.get_or_create(name="Socks", price=6.5),
      Item.objects.get_or_create(name="Pants", price=12),
      Item.objects.get_or_create(name="T-Shirt", price=8),
      Item.objects.get_or_create(name="Boots", price=9),
      Item.objects.get_or_create(name="Sweater", price=3),
      Item.objects.get_or_create(name="Underwear", price=9),
      Item.objects.get_or_create(name="Leggings", price=7),
      Item.objects.get_or_create(name="Cap", price=5),
    ]
    amount = options["amount"] if options["amount"] else 2500
    for i in range(0, amount):
      dt = pytz.utc.localize(datetime.now() -
        timedelta(days=random.randint(0, 1825)))
      purchase = Purchase.objects.create(
        customer_full_name=random.choice(names) + " " + 
          random.choice(surname), 
        item=random.choice(items)[0],
        payment_method=random.choice(Purchase.PAYMENT_METHODS)[0],
        successful=True if random.randint(1, 2) == 1 else False,
      )
      purchase.time = dt
      purchase.save()
    self.stdout.write(self.style.SUCCESS("Successfully populated the   
      database."))
```

Pokrenite sledeću komandu za popunjavanje DB:

```shell
(env)$ python manage.py populate_db --amount 1000
```

Ako je sve dobro prošlo, trebalo bi da vidite poruku "Successfully populated the database." o uspešno naseljenoj bazi podataka u konzoli.

Ovo je dodalo 8 stavki i 1.000 kupovina u bazi podataka.

## Priprema i serviranje podataka

Naša aplikacija će imati sledeće krajnje tačke:

1. `chart/filter-options/` - navodi sve godine u kojima imamo evidenciju.
2. `chart/sales/<YEAR>/` - donosi mesečne bruto prodajne podatke po godini.
3. `chart/spend-per-customer/<YEAR>/` - donosi mesečno trošenje po kupcu po godini.
4. `chart/payment-success/<YEAR>/` - donosi godišnje podatke o uspehu plaćanja.
5. `chart/payment-method/<YEAR>/` - dohvajete godišnjih podatke o načinu plaćanja (`Credit card`, `Debit card`, `Ethereum`, `Bitcoin`).

Pre nego što dodamo grafikone, napravimo nekoliko "utility" funkcija  koje će olakšati kreiranje grafikona.Dodajmo novi direktorijum u u root projekta pod nazivom "Utils". Zatim dodajmo novu datoteku u taj
direktorijum pod nazivom `Charts.py`:

`util/charts.py`

```py
months = [
  "January", "February", "March", "April",
  "May", "June", "July", "August",
  "September", "October", "November", "December"
]

colorPalette = [
  "#55efc4", "#81ecec", "#a29bfe", "#ffeaa7", 
  "#fab1a0","#ff7675", "#fd79a8"]

colorPrimary, colorSuccess, colorDanger = "#79aec8", colorPalette[0],
colorPalette[5]

def get_year_dict():
  year_dict = dict()

  for month in months:
    year_dict[month] = 0
  
  return year_dict

def generate_color_palette(amount):
  palette = []

  i = 0
  while i < len(colorPalette) and len(palette) < amount:
    palette.append(colorPalette[i])
    i += 1
    if i == len(colorPalette) and len(palette) < amount:
    i = 0
    return palette
```

Dakle, definisali smo svoje boje grafikona i stvorili sledeće dve metode:

1. `get_year_dict()` - kreira rečnik meseci i vrednosti, koji će se koristiti za dodavanje mesečnih podataka.

2. `generate_color_palette(amount)` - generiše ponavljajuće boje repeating color palette palete koja će biti prosledjena u grafikone.

## Prikazi

Kreiranje prikaza:

`shop/views.py`

```py
from django.contrib.admin.views.decorators import staff_member_required
from django.db.models import Count, F, Sum, Avg
from django.db.models.functions import ExtractYear, ExtractMonth
from django.http import JsonResponse

from shop.models import Purchase
from utils.charts import months, colorPrimary, colorSuccess, colorDanger,
generate_color_palette, get_year_dict

@staff_member_required
def get_filter_options(request):
  grouped_purchases = Purchase.objects.annotate(year=ExtractYear("time")).
    values("year").order_by("-year").distinct()
  options = [purchase["year"] for purchase in grouped_purchases]

  return JsonResponse({"options": options,})

@staff_member_required
def get_sales_chart(request, year):
  purchases = Purchase.objects.filter(time__year=year)
  grouped_purchases = 
    purchases.annotate(price=F("item__price")).annotate(month=ExtractMonth("time")) \
    .values("month").annotate(average=Sum("item__price")).values("month", "average").order_by("month")
  sales_dict = get_year_dict()

  for group in grouped_purchases:
    sales_dict[months[group["month"]-1]] = round(group["average"], 2)
  
  return JsonResponse({
    "title": f"Sales in {year}",
    "data": {
      "labels": list(sales_dict.keys()),
      "datasets": [{
        "label": "Amount ($)",
        "backgroundColor": colorPrimary,
        "borderColor": colorPrimary,
        "data": list(sales_dict.values()),
    }]
    },
  })

@staff_member_required
def spend_per_customer_chart(request, year):
  purchases = Purchase.objects.filter(time__year=year)
  grouped_purchases =
  purchases.annotate(price=F("item__price")).annotate(month=ExtractMonth("time")) \
  .values("month").annotate(average=Avg("item__price")).values("month", "average").order_by("month")
  spend_per_customer_dict = get_year_dict()
  
  for group in grouped_purchases:
    spend_per_customer_dict[months[group["month"]-1]] =
      round(group["average"], 2)
  
  return JsonResponse({
    "title": f"Spend per customer in {year}",
    "data": {
      "labels": list(spend_per_customer_dict.keys()),
      "datasets": [{
        "label": "Amount ($)",
        "backgroundColor": colorPrimary,
        "borderColor": colorPrimary,
        "data": list(spend_per_customer_dict.values()),
      }]
    },
  })

@staff_member_required
def payment_success_chart(request, year):
  purchases = Purchase.objects.filter(time__year=year)

  return JsonResponse({
    "title": f"Payment success rate in {year}",
    "data": {
      "labels": ["Successful", "Unsuccessful"],
      "datasets": [{
        "label": "Amount ($)",
        "backgroundColor": [colorSuccess, colorDanger],
        "borderColor": [colorSuccess, colorDanger],
        "data": [
          purchases.filter(successful=True).count(),
          purchases.filter(successful=False).count(),
        ],
      }]
    },
  })

@staff_member_required
def payment_method_chart(request, year):
  purchases = Purchase.objects.filter(time__year=year)
  grouped_purchases =
    purchases.values("payment_method").annotate(count=Count("id"))\
    .values("payment_method", "count").order_by("payment_method")
  payment_method_dict = dict()
  
  for payment_method in Purchase.PAYMENT_METHODS:
    payment_method_dict[payment_method[1]] = 0

  for group in grouped_purchases:
    payment_method_dict[dict(Purchase.PAYMENT_METHODS)
    [group["payment_method"]]] = group["count"]

  return JsonResponse({
    "title": f"Payment method rate in {year}",
    "data": {
      "labels": list(payment_method_dict.keys()),
      "datasets": [{
        "label": "Amount ($)",
        "backgroundColor":
          generate_color_palette(len(payment_method_dict)),
        "borderColor":
          generate_color_palette(len(payment_method_dict)),
        "data": list(payment_method_dict.values()),
      }]
    },
  })
```

> [!Note]:
>
> 1. `get_filter_options()`  
  Donosi sve kupovine, grupiše ih po godini, izdvaja godinu od polja vremena i vraća ih na listu.
> 2. `get_sales_chart()`  
  Donosi sve kupovine (u određenoj godini) i njihove cene, grupiraju ih po mesecu i izračunavaju mesečnu sumu cena.
> 3. `spend_per_customer_chart()`  
  Donosi sve kupovine (u određenoj godini) i njihove cene, grupiraju ih po mesecu i izračunavaju prosečnu mesečnu cenu.
> 4. `payment_success_chart()`  
  Računa se uspešne i neuspešne kupovine u  određenoj godini.
> 5. `payment_method_chart()`  
  Donosi sve kupovine (u određenoj godini), grupiše ih na način plaćanja, računa kupovinu i vraća ih u rečniku.

Imajte na umu da svaki prikaz ima `@staff_member_required` dekorator.

## URL-ovi

Kreiranje URLova aplikacionog nivoa:

`shop/urls.py`

```py
from django.urls import path
from . import views

urlpatterns = [
    path("chart/filter-options/", views.get_filter_options, name="chart-
        filter-options"),
    path("chart/sales/<int:year>/", views.get_sales_chart, name="chart-
        sales"),
    path("chart/spend-per-customer/<int:year>/",
        views.spend_per_customer_chart, name="chart-spend-per-customer"),
    path("chart/payment-success/<int:year>/", views.payment_success_chart,
        name="chart-payment-success"),
    path("chart/payment-method/<int:year>/", views.payment_method_chart,
        name="chart-payment-method"),
]
```

Sada povezujemo URL-ove na projektni url:

`core/urls.py`

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("shop/", include("shop.urls")), # new
]
```

## Testiranje

Sada kada smo registrovali URL adrese, hajde da testiramo krajnje tačke da vidimo da li sve ispravno funkcioniše.

Kreirajmo superusera, a zatim pokrenimo razvojni server:

```shell
(env)$ python manage.py createsuperuser
(env)$ python manage.py runserver
```

Sada, u brauzeru, navigujmo na <http://localhost:8000/shop/chart/filter-options/>. Trebalo bi da vidimo nešto ovako:

```shell
{
  "options": [
    2023,
    2022,
    2021,
    2020,
    2019,
    2018
  ]
}
```

Da biste videli mesečne podatke o prodaji za 2022, idite na <http://localhost:8000/shop/chart/sales/2022/>:

```shell
{
  "title": "Sales in 2022",
  "data": {
    "labels": [
      "January",
      "February",
      "March",
      "April",
      "May",
      "June",
      "July",
      "August",
      "September",
      "October",
      "November",
      "December"
    ],
    "datasets": [
      {
        "label": "Amount ($)",
        "backgroundColor": "#79aec8",
        "borderColor": "#79aec8",
        "data": [
          134.0,
          104.0,
          97.0,
          100.0,
          144.5,
          118.0,
          116.5,
          153.0,
          130.0,
          143.0,
          149.0,
          114.0
        ]
      }
    ]
  }
}
```

## Kreiranje grafikona sa Chart.js

Kretanje dalje, povezujemo Charts.js.

Dodajemo "templates" direktorijum na "shop". Zatim dodajemo novu datoteku pod nazivom `statistics.html`:

```html
<!-- shop/templates/statistics.html -->

<!DOCTYPE html>
<html lang="en">

<head>
  <title>Statistics</title>
  
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.2.1/dist/
    chart.umd.min.js"></script>
  
  <script src="https://code.jquery.com/jquery-3.6.4.min.js"
    integrity="sha256-oP6HI9z1XaZNBrJURtCoUT5SUnxFr8s3BzRl+cbzUq8="
    crossorigin="anonymous"> </script>
  
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-v4-
    grid-only@1.0.0/dist/bootstrap-grid.min.css">
</head>

<body>
<div class="container">
<form id="filterForm">

<label for="year">Choose a year:</label>
<select name="year" id="year"></select>
<input type="submit" value="Load" name="_load">

</form>
  <div class="row">
    <div class="col-6">
      <canvas id="salesChart"></canvas>
    </div>
    <div class="col-6">
      <canvas id="paymentSuccessChart"></canvas>
    </div>
    <div class="col-6">
      <canvas id="spendPerCustomerChart"></canvas>
    </div>
    <div class="col-6">
      <canvas id="paymentMethodChart"></canvas>
    </div>
  </div>
  <script>
    let salesCtx = document.getElementById("salesChart").getContext("2d");
    
    let salesChart = new Chart(salesCtx, {
      type: "bar",
      options: {
        responsive: true,
        title: {
          display: false,
          text: ""
        }
      }
    });
    
    let spendPerCustomerCtx = document.getElementById  
      ("spendPerCustomerChart").getContext("2d");
    
    let spendPerCustomerChart = new Chart(spendPerCustomerCtx, {
      type: "line",
      options: {
        responsive: true,
        title: {
          display: false,
          text: ""
        }
      }
    });

    let paymentSuccessCtx = 
      document.getElementById("paymentSuccessChart").getContext("2d");
  
    let paymentSuccessChart = new Chart(paymentSuccessCtx, {
      type: "pie",
      options: {
        responsive: true,
        maintainAspectRatio: false,
        aspectRatio: 1,
        title: {
          display: false,
          text: ""
        },
        layout: {
          padding: {
            left: 0,
            right: 0,
            top: 0,
            bottom: 25
          }
        }
      }
    });
  
    let paymentMethodCtx =
      document.getElementById("paymentMethodChart").getContext("2d");
    
    let paymentMethodChart = new Chart(paymentMethodCtx, {
      type: "pie",
      options: {
        responsive: true,
        maintainAspectRatio: false,
        aspectRatio: 1,
        title: {
          display: false,
          text: ""
        },
        layout: {
          padding: {
            left: 0,
            right: 0,
            top: 0,
            bottom: 25
          }
        }
      }
    });
  </script>
</div>

</body>
</html>
```

Ovaj blok koda stvara HTML platna koja `Chart.js` koristi za inicijalizaciju.

Takođe smo prošli odgovorni na opcije svake grafikone tako da se prilagođavaju na osnovu veličine prozora.

Dodajte sledeću skriptu u svoju HTML datoteku:

```html
<script>
$(document).ready(function() {
  $.ajax({
    url: "/shop/chart/filter-options/",
    type: "GET",
    dataType: "json",
    success: (jsonResponse) => {
      // Load all the options
      jsonResponse.options.forEach(option => {
        $("#year").append(new Option(option, option));
      });
      // Load data for the first option
      loadAllCharts($("#year").children().first().val());
    },
    error: () => console.log("Failed to fetch chart filter options!")
  });
});

$("#filterForm").on("submit", (event) => {
  event.preventDefault();
  
  const year = $("#year").val();
  loadAllCharts(year)
});

function loadChart(chart, endpoint) {
  $.ajax({
    url: endpoint,
    type: "GET",
    dataType: "json",
    success: (jsonResponse) => {
      // Extract data from the response
      const title = jsonResponse.title;
      const labels = jsonResponse.data.labels;
      const datasets = jsonResponse.data.datasets;

      // Reset the current chart
      chart.data.datasets = [];
      chart.data.labels = [];

      // Load new data into the chart
      chart.options.title.text = title;
      chart.options.title.display = true;
      chart.data.labels = labels;
      datasets.forEach(dataset => {

      chart.data.datasets.push(dataset);
  });
  chart.update();

  },
  error: () => console.log("Failed to fetch chart data from " +
    endpoint + "!")
  });

}

function loadAllCharts(year) {
  loadChart(salesChart, `/shop/chart/sales/${year}/`);
  loadChart(spendPerCustomerChart, `/shop/chart/spend-per-customer/
    ${year}/`);
  loadChart(paymentSuccessChart, `/shop/chart/payment-success/${year}/`);
  loadChart(paymentMethodChart, `/shop/chart/payment-method/${year}/`);
}
</script>
```

Kada se stranica učitava, ovaj skript šalje AJAX zahtev za /chart/filter opcije/ da dovedu sve aktivne godine i učita ih u formu.

1. `loadchart` opterećenja grafikona podataka sa krajnje tačke Django u grafikon
2. `LOADALLCHARTS` opterećuje sve grafikone

Imajte na umu da smo koristili JQuery da podnesem AJAX zahteva za jednostavnost.Umesto toga, slobodno koristite API.

Kreirajte novi prikaz:

`shop/view.py`

```py
@staff_member_required
def statistics_view(request):
    return render(request, "statistics.html", {})
```

Ne zaboravite uvoz:

```py
from django.shortcuts import render
```

Pridružite URL prikazu:

`shop/urls.py`

```py
from django.urls import path
from . import views

urlpatterns = [
  path("statistics/", views.statistics_view, name="shop-statistics"), # new
  path("chart/filter-options/", views.get_filter_options, name="chart-
    filter-options"),
  path("chart/sales/<int:year>/", views.get_sales_chart, name="chart-
    sales"),
  path("chart/spend-per-customer/<int:year>/",
    views.spend_per_customer_chart, name="chart-spend-per-customer"),
  path("chart/payment-success/<int:year>/", views.payment_success_chart,
    name="chart-payment-success"),
    path("chart/payment-method/<int:year>/", views.payment_method_chart,
    name="chart-payment-method"),
]
```

Grafikoni su sada pristupni na <http://localhost:8000/shop/statistics/>.

## Dodavanje grafikona na Django Admin

Što se tiče integracija grafikona u Admin Django, možemo:

1. Napravite novi Django Admin prikaz
2. Proširite postojeću administrativni šablon

### Kreiranje novog Django Admin prikaza

Stvaranje novog Django Admin Vieva je najčišći i najjasniji pristup.U ovom pristupu stvorićemo novi Adminsite i promeniti ga u podešavanjima, `settings.py` datoteci.

Prvo unutra `shop/templates` direktorijuma, dadajmo `admin` direktorijum. Dodajmo `statistics.html` šablon u njega:

```html
<!-- shop/templates/admin/statistics.html -->

{% extends "admin/base_site.html" %}

{% block content %}
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.2.1/dist/
  chart.umd.min.js">
</script>

<script src="https://code.jquery.com/jquery-3.6.4.min.js"
  integrity="sha256-oP6HI9z1XaZNBrJURtCoUT5SUnxFr8s3BzRl+cbzUq8="
  crossorigin="anonymous">
</script>

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-v4-
  grid-only@1.0.0/dist/bootstrap-grid.min.css">

<form id="filterForm">
  <label for="year">Choose a year:</label>
  <select name="year" id="year"></select>
  <input type="submit" value="Load" name="_load">
</form>

<script>
  $(document).ready(function() {
    $.ajax({
      url: "/shop/chart/filter-options/",
      type: "GET",
      dataType: "json",
      success: (jsonResponse) => {
        // Load all the options
        jsonResponse.options.forEach(option => {
          $("#year").append(new Option(option, option));
        });
        // Load data for the first option
        loadAllCharts($("#year").children().first().val());
      },
      error: () => console.log("Failed to fetch chart filter options!")
    });
  });

  $("#filterForm").on("submit", (event) => {
    event.preventDefault();
    const year = $("#year").val();
    loadAllCharts(year);
  });

  function loadChart(chart, endpoint) {
    $.ajax({
      url: endpoint,
      type: "GET",
      dataType: "json",
      success: (jsonResponse) => {
        // Extract data from the response
        const title = jsonResponse.title;
        const labels = jsonResponse.data.labels;
        const datasets = jsonResponse.data.datasets;
        
        // Reset the current chart
        chart.data.datasets = [];
        chart.data.labels = [];
        
        // Load new data into the chart
        chart.options.title.text = title;
        chart.options.title.display = true;
        chart.data.labels = labels;
        datasets.forEach(dataset => {
          chart.data.datasets.push(dataset);
        });
        chart.update();
      },
      error: () => console.log("Failed to fetch chart data from " +
      endpoint + "!")
    });
  }

  function loadAllCharts(year) {
    loadChart(salesChart, `/shop/chart/sales/${year}/`);
    loadChart(spendPerCustomerChart, `/shop/chart/spend-per-customer/
      ${year}/`);
    loadChart(paymentSuccessChart, `/shop/chart/payment-success/${year}/`);
    loadChart(paymentMethodChart, `/shop/chart/payment-method/${year}/`);
  }
</script>

<div class="row">
  <div class="col-6">
    <canvas id="salesChart"></canvas>
  </div>
  <div class="col-6">
    <canvas id="paymentSuccessChart"></canvas>
   </div>
  <div class="col-6">
    <canvas id="spendPerCustomerChart"></canvas>
  </div>
  <div class="col-6">
    <canvas id="paymentMethodChart"></canvas>
  </div>
</div>

<script>
  let salesCtx = document.getElementById("salesChart").getContext("2d");
  let salesChart = new Chart(salesCtx, {
    type: "bar",
    options: {
      responsive: true,
      title: {
        display: false,
        text: ""
      }
    }
  });
  let spendPerCustomerCtx =
    document.getElementById("spendPerCustomerChart").getContext("2d");
  let spendPerCustomerChart = new Chart(spendPerCustomerCtx, {
    type: "line",
    options: {
      responsive: true,
      title: {
        display: false,
        text: ""
      }
    }
  });
  let paymentSuccessCtx =
    document.getElementById("paymentSuccessChart").getContext("2d");
  let paymentSuccessChart = new Chart(paymentSuccessCtx, {
    type: "pie",
    options: {
      responsive: true,
      maintainAspectRatio: false,
      aspectRatio: 1,
      title: {
        display: false,
        text: ""
      },
    layout: {
      padding: {
        left: 0,
          right: 0,
          top: 0,
          bottom: 25
        }
      }
    }
  });
  let paymentMethodCtx =
    document.getElementById("paymentMethodChart").getContext("2d");
  let paymentMethodChart = new Chart(paymentMethodCtx, {
    type: "pie",
    options: {
      responsive: true,
      maintainAspectRatio: false,
      aspectRatio: 1,
      title: {
        display: false,
        text: ""
      },
      layout: {
        padding: {
          left: 0,
          right: 0,
          top: 0,
          bottom: 25
        }
      }      
    }
  });
</script>
{% endblock %}
```

Sledeće kreiramo `core/admin.py` fajl:

`core/admin.py`

```py
from django.contrib import admin
from django.contrib.admin.views.decorators import staff_member_required
from django.shortcuts import render
from django.urls import path

@staff_member_required
def admin_statistics_view(request):
  return render(request, "admin/statistics.html", {
    "title": "Statistics"
  })

class CustomAdminSite(admin.AdminSite):
  
  def get_app_list(self, request, _=None):
    app_list = super().get_app_list(request)
    app_list += [
      {
        "name": "My Custom App",
        "app_label": "my_custom_app",
        "models": [
          {
            "name": "Statistics",
            "object_name": "statistics",
            "admin_url": "/admin/statistics",
            "view_only": True,
          }
        ],
      }
    ]     
    return app_list

    def get_urls(self):
      urls = super().get_urls()
      urls += [
        path("statistics/", admin_statistics_view, name="admin-
          statistics"),
      ]
      return urls
```

Ovde smo stvorili prikaz zaštićenog osoblja koji se zove `admin_statistics_view`. Zatim smo kreirali novi `Adminsite` i `Overderode` `Get_App_List` da biste na njega dodali vlastitu prilagođenu primenu. Obezbedili smo svoju aplikaciju sa modelom samo veštačkom prikazu pod nazivom `Statistike`.I na kraju, prevladamo `get_urls` i dodelimo URL na naš novi prikaz.

#### Kreiranje AdminConfig unutar core/apps.py

`core/apps.py`

```py
from django.contrib.admin.apps import AdminConfig

class CustomAdminConfig(AdminConfig):
  default_site = "core.admin.CustomAdminSite"
```

Ovde kreiramo novi `AdminConfig`, koji učitava `CustomAdminSite`  umesto Djangoov podrazumevani.

Zamenite podrazumevani Adminnconfig sa novim u `core/settings.py`:

```py
INSTALLED_APPS = [
  "core.apps.CustomAdminConfig", # replaced
  "django.contrib.auth",
  "django.contrib.contenttypes",
  "django.contrib.sessions",
  "django.contrib.messages",
  "django.contrib.staticfiles",
  "shop.apps.ShopConfig",
]
```

Sada ažurirajte `core/urls.py`:

`core/urls.py`

```py
from django.contrib import admin
from django.urls import path, include

from .admin import admin_statistics_view # new

urlpatterns = [
  path( # new
    "admin/statistics/", admin.site.admin_view(admin_statistics_view),
    name="admin-statistics"
  ),
  path("admin/", admin.site.urls),
  path("shop/", include("shop.urls")),
]
```

Pogledajte na <http://localhost:8000/admin/> da vidite novi Django admin prikaz.

## Proširite postojeće admin šablone

Uvek možete proširiti šablone admina i nadjačati delove koje želite.

Na primer, ako želite da stavite grafikone ispod modela vaše prodavnice, to možete učiniti nadjačavajući šablon `app_index.html`.Dodajte novu `app_index.html` datoteku u `shop/templates/admin/shop`, a zatim dodajte HTML pronađene ovde.

## Zaključak

U ovom tutorialu ste naučili kako da poslužite podatke sa Django-om, a zatim ga vizualizujte koristeći `Charts.js`. Takođe smo pogledali dva različita pristupa za integrisanje grafikona u Admin Django.

Preuzmite kod sa `django-interactive-charts` repoa GitHuba.

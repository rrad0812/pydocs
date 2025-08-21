# CSV export-import

## Sadržaj

[Nazad](sadrzaj.md)

- CSV Export iz Django admina?
- CSV Import u Django admin
- CSV Export preko komandne linije
- CSV Import preko komandnde linije

CSV Export iz Django admina?

Od vas je zatraženo da dodate mogućnost izvoza Hero i Villain iz admina. Postoji veliki broj nezavisnih
aplikacija koje to omogućavaju, ali to je prilično lako i bez dodavanja nove zavisnosti. Dodaćete admin
akciju u HeroAdmin i VillanAdmin. Svaka admin akcija uvek ima isti potpis:
def admin_action(ModelAdmin, request, queryset):
ili alternativno možete dodati direktno u ModelAdmin kao:

class SomeModelAdmin(admin.ModelAdmin):
def admin_action(self, request, queryset):

Da dodate izvoz HeroAdmin-u možete napraviti sledeće:

actions = ["export_as_csv"]
def export_as_csv(self, request, queryset):

pass
export_as_csv.short_description = "Export Selected"

To dodaje akciju export_as_csv. Sada definišemo export_as_csv:
import csv
from django.http import HttpResponse
...
def export_as_csv(self, request, queryset):

meta = self.model._meta
field_names = [field.name for field in meta.fields]
response = HttpResponse(content_type='text/csv')
response['Content-Disposition'] = 'attachment';
filename={}.csv'.format(meta)
writer = csv.writer(response)
writer.writerow(field_names)
for obj in queryset:

row = writer.writerow([getattr(obj, field) for field in field_names])
return response

Ovo izvozi sve izabrane zapise. Primetite, export_as_csv nema ništa specifično sa Hero modelom, tako da
možemo ekstrahovati metod u mixin. Sa tim promenama, kod izgleda ovako:

class ExportCsvMixin:
def export_as_csv(self, request, queryset):

meta = self.model._meta
field_names = [field.name for field in meta.fields]
response = HttpResponse(content_type='text/csv')
response['Content-Disposition'] = 'attachment;
filename={}.csv.format(meta)
writer = csv.writer(response)
writer.writerow(field_names)
for obj in queryset:

row = writer.writerow(



P a g e | 117
[getattr(obj, field) for field in field_names])
return response

export_as_csv.short_description = "Export Selected"
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

list_display = ("name", "is_immortal", "category", "origin",
"is_very_benevolent")

list_filter = ("is_immortal", "category", "origin",
IsVeryBenevolentFilter)

actions = ["export_as_csv"]
...

@admin.register(Villain)
class VillainAdmin(admin.ModelAdmin, ExportCsvMixin):

list_display = ("name", "category", "origin")
actions = ["export_as_csv"]

Možete dodati CSV izvoz i drugim modelima, nasledjujući ExportCsvMixin.

CSV Import u Django admin
Od vas je zatraženo da dozvolite uvoz CSV-a na Hero adminu. To ćete uraditi dodavanjem linka na
listview stranu Hero, koji vodi na stranu sa obrascem za otpremanje.

Napisaćete hendler za akciju POSTza kreiranje objekata iz CSV-a:

class CsvImportForm(forms.Form):
csv_file = forms.FileField()

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):

...
change_list_template = "entities/heroes_changelist.html"
def get_urls(self)get_urls(self):

urls = super().get_urls()
my_urls = [
...
path('import-csv/', self.import_csv),

]
return my_urls + urls

def import_csv(self, request):
if request.method == "POST":

csv_file = request.FILES["csv_file"]
reader = csv.reader(csv_file)
Create Hero objects from passed in
data#...
self.message_user(request, "Your csv file has been imported")
return redirect("..")

form =
CsvImportForm()

payload =



P a g e | 118
{"form":form}

return render(request, "admin/csv_form.html", payload)
)

Zatim kreirate entities/heroes_changelist.html šablon, nadjačavajući admin/change_list.html:

{% extends 'admin/change_list.html' %}
{% block object-tools %}
<a href="import-csv/">Import CSV</a>
<br />
{{ block.super }}
{% endblock %}

Na kraju kreirate csv_form.html:

{% extends 'admin/base.html' %}
{% block content %}
<div>
<form action="." method="POST" enctype="multipart/form-data">
{{ form.as_p }}
{% csrf_token %}
<button type="submit">Upload CSV</button>
</form>
</div>
<br />
{% endblock %}

Sa ovim promenama, dobijate link na listview strani.

CSV Export preko komandne linije
Napravimo fajl:

# app_name/management/commands/dump_app_name_csv.py
import os
import csv
from django.conf import settings
from academy.models import Invite
from django.core.management.base import BaseCommand
class Command(BaseCommand):

def handle(self, *args, **options):
print "Dumping CSV"
csv_path = os.path.join(settings.BASE_DIR, "dump_invites.csv")
csv_file = open(csv_path, 'wb')
csv_writer = csv.writer(csv_file)
csv_writer.writerow(['name', 'branch', 'gender', 'date_of_birth',

'race', 'notes', 'reporter'])
for obj in Invite.objects.all():

row = [obj.name, obj.branch, obj.gender, obj.date_of_birth,
obj.race,obj.notes, obj.reporter]

csv_writer.writerow(row)



P a g e | 119
Pokrenimo gornji fajl:

$ python manage.py dump_app_name_csv

Podaci će biti izvezeni u fajl "dump_invites.csv".

CSV Import preko komandnde linije
Napravimo sledeću šemu direktorijuma aplikacije:

app_name/
init .py

admin.py
apps.py
models.py
management
/

init .py
commands/

init .py
migrations/
tests.py
views.py

Napravimo fajl app_name/management/commands/load_app_name_csv.py sa sadržajem:
import csv
from app_name.models import Invite
from django.core.management.base import BaseCommand
class Command(BaseCommand):

def handle(self, *args, **options):
print "Loading CSV"
csv_path = "./app_name_invites.csv" Iz ovog csv se učitavaju podaci
csv_file = open(csv_path, 'rb')
csv_reader = csv.DictReader(csv_file)
for row in csv_reader:

obj = Invite.objects.create(name=row['Name'], branch=row['Branch'])
print obj

Pokrenimo gornji fajl:

$ python manage.py load_app_name_csv
Podaci će biti učitani u Invite tabelu iz fajla "./app_name_invites.csv".


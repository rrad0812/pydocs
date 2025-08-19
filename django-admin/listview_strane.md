# ListView strane

## Sadržaj

[Nazad](sadrzaj.md)

- [Kako prikazati veliki broj zapisa na listview strani?](#kako-prikazati-veliki-broj-zapisa-na-listview-strani)
- [Kako ukinuti Django admin paginaciju?](#kako-ukinuti-django-admin-paginaciju)
- [Kako dodati datumski bazirano filtriranje na Django admin?](#kako-dodati-datumski-bazirano-filtriranje-na-django-admin)
- [Kako prikazati unutrašnju ManyToMany ili ManyToOne vezu na listview strani?](#kako-prikazati-unutrašnju-manytomany-ili-manytoone-vezu-na-listview-strani)

### Kako prikazati veliki broj zapisa na listview strani?

Treba da povaćate broj prikazanih zapisa na Hero `listview` admin strani na 250. Podrazumevano je 100.:

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    list_per_page = 250
```

Možete takodje postaviti i manju vrednost ( od 100 ).

[Sadržaj](#sadržaj)

### Kako ukinuti Django admin paginaciju?

Ako želite da ukinete paginaciju u potpunosti na `listview` strani:

```py
import sys
...
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    list_per_page = sys.maxsize
```

[Sadržaj](#sadržaj)

### Kako dodati datumski bazirano filtriranje na Django admin?

Možete dodati datumski bazirano filtriranje sa postavkom `date_hierarchy`:

```py

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    date_hierarchy = 'added_on'
```

Ovo može biti skupo na velikom broju objekata. Kao alternativa, možete naslediti
`SimpleListFilter`, i dozvoliti filtriranje samo na godinama ili mesecima.

[Sadržaj](#sadržaj)

### Kako prikazati unutrašnju ManyToMany ili ManyToOne vezu na listview strani?

Za Hero model možete pratiti njihove roditelje koristeći:

```py
father = models.ForeignKey( "self", related_name="children", null=True, blank=True,
    on_delete=models.SET_NULL
)
```

Od vas je zatraženo da pokažete decu svakog Hero na `listview` strani. Hero objekti imaju unutrašnju vezu, ali to ne možete dodati u `list_display`. Morate dodati atribut u ModelAdmin i koristiti ga u `list_display`.

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    def children_display(self, obj):
        return ", ".join([child.name for child in obj.children.all()])

    children_display.short_description = "Children"
```

[Sadržaj](#sadržaj)

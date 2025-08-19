
# Izračunata polja

## Sadržaj

[Nazad](sadrzaj.md)

- [Kako prikazati izračunato polje na listview strani?](#kako-prikazati-izračunato-polje-na-listview-strani)
- [Kako opitimizovati upite u Django adminu sa annotacijama?](#kako-opitimizovati-upite-u-django-adminu-sa-annotacijama)
- [Kako omogućiti sortiranje na izračunatim poljima?](#kako-omogućiti-sortiranje-na-izračunatim-poljima)
- [Kako omogućiti filtriranje po izračunatim poljima?](#kako-omogućiti-filtriranje-po-izračunatim-poljima)
- [Kako prikazati ’on’ ili ’off’ ikone umesto izračunatih boolean polja?](#kako-prikazati-izračunato-polje-na-listview-strani)

### Kako prikazati izračunato polje na listview strani?

Imate admin za Origin model:

```py
@admin.register(Origin)
class OriginAdmin(admin.ModelAdmin):
    list_display = ("name",)
```

Pored imena, takođe želimo da prikažemo broj `heroes` i broj `vilians` za svaki origin, što nije DB polje na Origin modelu. To možete učiniti na dva načina.

Dodavanjem metoda na model:

```py
def hero_count(self,):
    return self.hero_set.count()

def villain_count(self):
    return self.villain_set.count()
```

I promeneite  `list_display` na:

```py
list_display = ("name", "hero_count", "villain_count")
```

Dodavanjem metoda na ModelAdmin:

```py
def hero_count(self, obj):
    return obj.hero_set.count()

def villain_count(self, obj):
    return obj.villain_set.count()
```

sada `list_display`, izgleada ovako:

```py
list_display = ("name", "hero_count", "villain_count")
```

Bilo koji metod da primenimo, pokrenuće dva dodatna upita po objektu ( slogu ).

[Sadržaj](#sadržaj)

### Kako opitimizovati upite u Django adminu sa annotacijama?

Ako imate više izračunatih polja u adminu, admin može postati spor. Uzmimo primer sa modela Origin:

```py
@admin.register(Origin)
class OriginAdmin(admin.ModelAdmin):
    list_display = ("name", "hero_count", "villain_count")
    
    def hero_count(self, obj)
        return obj.hero_set.count()
    
    def villain_count(self, obj):
        return obj.villain_set.count()
```

Da bi popravili performase nadjačajmo metod `get_queryset` da obeležimo ( annotate ) prebrojane zapise. Sa ovakvom promenom ModelAdmin izgleda ovako:

```py
@admin.register(Origin)
class OriginAdmin(admin.ModelAdmin):
    list_display = ("name", "hero_count", "villain_count")
    
    def get_queryset(self, request):
        queryset = super().get_queryset(request)
        queryset = queryset.annotate(
            _hero_count=Count("hero", distinct=True),
            _villain_count=Count("villain", distinct=True),
        )
        return queryset

    def hero_count(self, obj):
        return obj._hero_count

    def villain_count(self, obj):
        return bj._villain_count
```

Sada nema viška upita. Admin sada izgleda kao i pre obeležavanja.

[Sadržaj](#sadržaj)

### Kako omogućiti sortiranje na izračunatim poljima?

Django dodaje mogućnosti sortiranja na poljima koja su atributi na modelima. Kada dodate izračunato polje, Django ne zna kako da izvrši `order_by`, pa ne dodaje mogućnost sortiranja na tom polju. Ako želite dodati sortiranje na izračunato polje, morate reći Django-u šta da prosledi u `order_by`. To možete učiniti postavljanjem atributa `admin_order_field` na metodi izračunatog polja.

Polaziš od admina kojeg si napisao u prethodnom poglavlju i dodaš:

```py
hero_count.admin_order_field = '_hero_count'
villain_count.admin_order_field = '_villain_count'
```

Sa ovim promenama `AdminModel` postaje:

```py
@admin.register(Origin)
class OriginAdmin(admin.ModelAdmin):
    list_display = ("name", "hero_count", "villain_count")

    def get_queryset(self, request):
        queryset = super().get_queryset(request)
        queryset = queryset.annotate(
            _hero_count=Count("hero", distinct=True),
            _villain_count=Count("villain", distinct=True),
        )
        return queryset

    def hero_count(self, obj):
        return obj._hero_count

    def villain_count(self, obj):
        return

    hero_count.admin_order_field = '_hero_count'
    villain_count.admin_order_field = '_villain_count'
```

Sada možemo sortirati i po izračunatim poljima (`hero_count`).

[Sadržaj](#sadržaj)

### Kako omogućiti filtriranje po izračunatim poljima?

Imate Hero admin koji izgleda ovako:

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin):
    list_display = ("name", "is_immortal", "category", "origin", "is_very_benevolent")
    list_filter = ("is_immortal", "category", "origin",)
    
    def is_very_benevolent(self, obj):
        return obj.benevolence_factor > 75
```

Želimo da filtriramo i po izračunatim poljima. Da to uradimo, potrebno je da nasledimo `SimpleListFilter` klasu:

```py
class IsVeryBenevolentFilter(admin.SimpleListFilter):
    title = 'is_very_benevolent'
    parameter_name = 'is_very_benevolent'
    
    def lookups(self, request, model_admin):
        return (('Yes', 'Yes'),('No', 'No'),)

    def queryset(self, request, queryset): value = self.value()
        if value == 'Yes':
            return queryset.filter(benevolence_factor gt=75)
        elif value == 'No':
            return queryset.exclude(benevolence_factor lte=75)
        
        return queryset
```

I promenimo list_filter na:

```py
list_filter = ("is_immortal", "category", "origin", IsVeryBenevolentFilter)
```

[Sadržaj](#sadržaj)

### Kako prikazati ’on’ ili ’off’ ikone umesto izračunatih boolean polja?

Prethodno smo dodali izračunato boolean polje:

```py
return obj.benevolence_factor > 75
```

`is_very_benevolent` polje prikazuje string `True` ili `False`, za razliku od ugradjenih `BooleanFields` koji prikazuju `on` ili `off` indikator. Da bi to popravili, dodajmo boolean atribut našem metodu. ModelAdmin sada izgleda ovako:

```py
@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin):
    list_display = ("name", "is_immortal", "category", "origin", "is_very_benevolent")
    list_filter = ("is_immortal", "category", "origin", IsVeryBenevolentFilter)

    def is_very_benevolent(self, obj):
        return obj.benevolence_factor > 75
    
    is_very_benevolent.boolean = True
```

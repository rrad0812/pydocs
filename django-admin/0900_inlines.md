
# Inlines

## Sadržaj

[Nazad](sadrzaj.md)

### Kako editovati višestruke modele iz jednog admina?

Ukratko, treba da koristite `inlines`. Imate "Category" model, i potrebno je da dodajete i editujete "Villain" modele unutar "Category" admina:

```py
class VillainInline(admin.StackedInline):
    model = Villain

@admin.register(Category)
class CategoryAdmin(admin.ModelAdmin):
    ...
    inlines = [VillainInline]
```

Vidite da je forma za dodavanje i editovanje "Villain" unutar "Category" admina. Ako `Inline` model ima više polja, koristite `StackedInline` inače koristite `TabularInline`.

### Kako dodati OneToOne vezu kao inline?

OneToOne veza može biti postavljena na isti način kao i ManyToOne. Ali, samo jedna strana OneToOne veze može biti postavljena kao inline.

Možete imati HeroAcquaintance model sa OneToOne vezom na Hero korišćenjem:

```py
class HeroAcquaintance(models.Model):
    """Non family contacts of a Hero"""
    hero = models.OneToOneField(Hero, on_delete=models.CASCADE)
    ....
```

Možete dodati kao inline na Hero kao:

```py
class HeroAcquaintanceInline(admin.TabularInline):
model = HeroAcquaintance

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    inlines = [HeroAcquaintanceInline]
    inlines = [HeroAcquaintanceInline]
```

### Kako dodati ugnježdene inlines u Django adminu?

Imate modele ovako definisane:

```py
class Category(models.Model):
    ...

class Hero(models.Model):
    category = models.ForeignKey(Catgeory)
    ...

class HeroAcquaintance(models.Model):
    hero = models.OneToOneField(Hero, on_delete=models.CASCADE)
```

Želite jednu admin stranu za kreiranje Category, Hero i HeroAcquaintance objekata. Django ne podržava umetnute inline sa ManyToOne ili OneToOne vezama koje povezuju više od jednog nivoa. Imate malo opcija:

- Možete promeniti HeroAcquaintance model, tako da imate direktnu FK vezu na Category:

    ```py
    class HeroAcquaintance(models.Model):
        hero = models.OneToOneField(Hero, on_delete=models.CASCADE)
        category = models.ForeignKey(Category)
    
        def save(self, *args, **kwargs):
            self.category = self.hero.category
            super().save(*args, **kwargs)
    ```

    Potom možete povezati HeroAcquaintanceInline na CategoryAdmin, i dobiti neku
    vrstu umetnutog inline.

- Alternativno, postoje gotovi Django add-on apps, koje pružaju umetnute inlines.
Pogledaj na Github ili DjangoPackages.

### Kako kreirati jedan admin iz dva različita modela?

Hero ima FK vezu na Category, tako da možete izabrati Category iz Hero admina. Ako želite da kreirate Category objekat iz Heroa dmina, možete promeniti formu Hero admina i prilagoditi ponašanje save_model:

```py
class HeroForm(forms.ModelForm):
    category_name = forms.CharField()

class Meta:
    model = Hero
    exclude = ["category"]

@admin.register(Hero)
class HeroAdmin(admin.ModelAdmin, ExportCsvMixin):
    form = HeroForm
    ....

    def save_model(self, request, obj, form, change):
        category_name = form.cleaned_data["category_name"]
        category, _ = Category.objects.get_or_create(name=category_name)
        obj.category = category
        super().save_model(request, obj, form, change)
```

Na ovaj način Hero admin može da kreira i ažurira Category model.

### Kako pristupiti parent instanci?

Moj cilj je da nadjačam has_add_permission funkciju bazirano na vrednosti polja statusparentinstance. Ja ne želim da dozvolim dodavanje dečijih instanci ako je status parent instance različit od 1.

```py
class ChildInline(admin.TabularInline):
    model = Child
    form = ChildForm
        fields = (
            ...
        )
    extra = 0

    def has_add_permission(self, request):
        # Return True only if the parent has status == 1
        # How to get to the parent instance?
        #return True

class ParentAdmin(admin.ModelAdmin):
    inlines = [ChildInline,]
    ...
```

**Odgovor_1**:

Za dobijanje parent objekta iskoristiti podatke iz reqeust. resolve funkcija iz django.urls modula vraća podatke koji su nama interesantni. Funkcioniše kada su argumenti prisutni u zahtevu.

```py
from django.contrib import admin
from django.urls import resolve
from app.models import YourParentModel, YourInlineModel

class YourInlineModelInline(admin.StackedInline):
    model = YourInlineModel
    
    def get_parent_object_from_request(self, request):
        resolved = resolve(request.path_info)
        if resolved.args:
            return self.parent_model.objects.get(pk=resolved.args[0])
        return None

    def has_add_permission(self, request):
        parent = self.get_parent_object_from_request(request)
        # Validate that the parent status is active (1)
        if parent:
            return parent.status == 1
        # No parent - return original has_add_permission() check
        return super(YourInlineModelInline, self).has_add_permission(request)

@admin.register(YourParentModel)
class YourParentModelAdmin(admin.ModelAdmin):
    inlines = [YourInlineModelInline]
```

**Odgovor_2**:

Da bi došli do parent_obj u child klasi, nadjačamo get_formset, postavljanjući parent_obj kao prosledjeni obj datog metoda.

```py
class ChildInline(admin.TabularInline):
    model = Child
    form = ChildForm
    fields = (...)
    extra = 0

    def get_formset(self, request, obj=None, **kwargs):
        self.parent_obj = obj
        return super(ChildInline, self).get_formset(request, obj, **kwargs)

    def has_add_permission(self, request):
        return self.parent_obj.status == 1

class ParentAdmin(admin.ModelAdmin):
    inlines = [ChildInline, ]
```

**Odgovor_3**:

Samo treba da dodate obj parametar i proverite parent model status:

```py
class ChildInline(admin.TabularInline):
    model = Child
    form = ChildForm
    fields = (
        ...
    )
    extra = 0
    
    def has_add_permission(self, request, obj=None):
        if obj.status == 1:
            return True
        else:
            return False

class ParentAdmin(admin.ModelAdmin):
    inlines = [ChildInline,]
```

**Odgovor_4**:

Pokušao sam predloženo ali ne radi za mene, morao sam da koristim varijantu iz Odgovor_1:

```py
def get_parent_object_from_request(self, request):
    resolved = resolve(request.path_info)
    if resolved.kwargs:
        return self.parent_model.objects.get(pk=resolved.kwargs["object_id"])
    return None
```

### Kako dobiti objekt iz formfield_for_foreignkey?

Pokušao sam da filtriram opcije prikazane u foreignkey polju, unutar django admin inlajna, zbog njihovog editovanja.

```py
class ProjectGroupMembershipInline(admin.StackedInline):
    model = ProjectGroupMembership
    extra = 1
    formset = ProjectGroupMembershipInlineFormSet
    form = ProjectGroupMembershipInlineForm
    
    def formfield_for_foreignkey(self, db_field, request=None, **kwargs):
        if db_field.name == 'group':
            kwargs['queryset'] = Group.objects.filter(

        some_filtering_here=object_being_edited)
        return super(ProjectGroupMembershipInline, self)

formfield_for_foreignkey(db_field, request, **kwargs))
```

Verifikovao sam da je kwargs prazan kada se edituje objekat, tako da ne mogu dobiti objekat odatle. Pomoć?

**Odgovor_1**:

Da bi filtrirali izbore raspložive u polju stranog ključa admin inlajna, treba nadjačati formu tako da bude moguć apdejt atributa queryset polja forme. Na taj način možete pristupiti self.instance što je objekat koji se edituje u formi.

```py
class ProjectGroupMembershipInlineForm(forms.ModelForm):

def init (self, *args, **kwargs):
super(ProjectGroupMembershipInlineForm, self). init (*args, **kwargs)
self.fields['group'].queryset = Group.objects.filter(

some_filtering_here=self.instance)
```

Nema potrebe da koristite `formfield_for_foreignkey`.

**Odgovor_2**:

Drugi način, malo čistiji, sličan gornjem odgovoru:

```py
class ProjectGroupMembershipInline(admin.StackedInline):
    # irrelevant bits....
    def formfield_for_foreignkey(self, db_field, request, **kwargs):
        if db_field.name == "group":
            try:
                parent_id = request.resolver_match.args[0]
                kwargs["queryset"] = Group.objects.filter(some_column=parent_id)
            except IndexError:
                pass
        return super().formfield_for_foreignkey(db_field, request, **kwargs)
```

**Odgovor_3**:

Moguće je to rešiti korišćenjem formfield_for_foreignkey i uklanjanjem object ID-a iz url-a. Nije najleši način dobijnja ID-a ali Django ne obezbeđuje pristup object ID-u.

```py
class ObjectAdmin(admin.ModelAdmin):

    def formfield_for_foreignkey(self, db_field, request, **kwargs):
        obj_id = request.META['PATH_INFO'].rstrip('/').split('/')[-1]
        if db_field.name == 'my_field' and obj_id.isdigit():
            obj = self.get_object(request, obj_id)
            if obj:
                kwargs['queryset'] = models.Object.objects.filter(field=obj)
                return super().formfield_for_foreignkey(db_field, request, **kwargs)
```

**Odgovor_4**:

Django smešta parent id u kwargs a ne u args tako da korektan je sledeći način:

```py
parent_id = request.resolver_match.kwargs.get('object_id')
```

**Odgovor_5**:

Ovo je rešenje do koga sam došao

```py
def formfield_for_foreignkey(self, db_field, request, **kwargs):
    if db_field.name == "group":
        parent_id = request.resolver_match.kwargs['object_id']
        kwargs["queryset"] = Group.objects.filter(some_column=parent_id)
        return super().formfield_for_foreignkey(db_field, request, **kwargs)
```

### Pristup parent model instanci iz model forme admin inline-a

Koristim `TabularInline` u Django adminu, konfigurisan da prikaže jednu extra blank formu.

```py
class MyChildInline(admin.TabularInline):
model = MyChildModel
form = MyChildInlineForm
extra = 1
```

Modeli su MyParentModel->MyChildModel->MyInlineForm. Koristim prilagođenu formu ako da dinamički radim lookup i popunjavam lookup polja:

```py
class MyChildInlineForm(ModelForm):
    my_choice_field = forms.ChoiceField()
    
    def init (self, *args, **kwargs):
        super(MyChildInlineForm, self). init (*args, **kwargs)
        # Lookup ID of parent model. parent_id = None
        if "parent_id" in kwargs:
            parent_id = kwargs.pop("parent_id")
        elif
            self.instance.parent_id:
            parent_id = self.instance.parent_id
        elif self.is_bound:
            parent_id = self.data['%s-parent'% self.prefix]
        
        if parent_id:
            parent = MyParentModel.objects.get(id=parent_id)
        if rev:
            qs = parent.get_choices()
            self.fields['my_choice_field'].choices = [(r.name,r.value) for r in qs]
```

Ovo radi dobro za inline zapise u edit modu, ali za ekstra blank formu ne prikazuje vrednosti u mome lookup polju, pošto nema još zapisa i ne može lookup pridruženi MyParentModel zapis.

### Pristup parent model instanci iz modelforme admin inline-a

**Odgovor_1**:

Od Django 1.9, tu je get_form_kwargs(self, index)metod u BaseFormSet klasi.

```py
class MyFormSet(BaseInlineFormSet):
    
    def get_form_kwargs(self, index):
        kwargs = super().get_form_kwargs(index)
        kwargs['parent_object'] = self.instance
        return kwargs

class MyForm(forms.ModelForm):

    def init (self, *args, parent_object, **kwargs):
        self.parent_object = parent_object
        super(MyForm, self). init (*args, **kwargs)

class MyChildInline(admin.TabularInline):
    formset = MyFormSet
    form = MyForm
```

**Odgovor_2**:

Koristim Django 1.10 i ovo radi za mene:

Kreiraj formset i postavi parent objekat u kwargs:

```py
class MyFormSet(BaseInlineFormSet):
    def get_form_kwargs(self, index):
        kwargs = super(MyFormSet, self).get_form_kwargs(index)
        kwargs.update({'parent': self.instance})
        return kwargs
```

Kreiraj objekat Form i pop atribut pre poziva super():

```py
class MyForm(forms.ModelForm):

    def init (self, *args, **kwargs):
        parent = kwargs.pop('parent')
        super(MyForm, self). init (*args, **kwargs)
        # radi šta treba sa parent
```

Postavi u inline admin:

```py
class MyModelInline(admin.StackedInline):
    model = MyModel
    fields = ('my_fields', )
    form = MyFrom
    formset = MyFormSet
```

**Odgovor_3**:

AdminModel ima metod `get_formsets`. On prima objekat i vraća formsetove. Mislim da možete dodati neki info o parent objektu u formset klasu i kasnije koristiti formsetovu `__init__` metodu.

**Odgovor_4**:

Ako je polje forme od intersa konstruisano iz polja modela, možete koristiti sledeći kod za primenu prilagođenog ponašanja:

```py
class MyChildInline(admin.TabularInline):
    model = MyChildModel
    extra = 1
    
    def get_formset(self, request, parent=None, **kwargs):
    
        def formfield_callback(db_field):
            """
            Constructor of the formfield given the model field.
            """
            formfield = self.formfield_for_dbfield(db_field, request=request)
            if db_field.name == 'my_choice_field' and parent is not None:
                formfield.choices = parent.get_choices()
                return formfield
            return super(MyChildInline, self).get_formset(request, obj=obj,

        formfield_callback=formfield_callback, **kwargs)
        return result

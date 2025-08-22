
# Prilagodjeni search lookup

[Sadržaj](00_sadrzaj.md)

DjangoQL bazno pruža dva osnovna metoda koje možete prepisati:

- get_lookup_name() i
- get_lookup_value(value):

```py
class UserDateJoinedYear(IntField):name = 'date_joined_year'

    def get_lookup_name(self): 
        return 'date_joined year'

class UserQLSchema(DjangoQLSchema):
    def get_fields(self, model):
        fields = super(UserQLSchema, self).get_fields(model)
        if model == User:
            fields += [UserDateJoinedYear()]return fields

@admin.register(User)
class CustomUserAdmin(DjangoQLSearchMixin, UserAdmin):
    djangoql_schema =     UserQLSchema
```

U ovom primeru mi smo definisali prilagodjeno "date_joined_year" search polje za korisnike. I koristili smo ugradjeni Django year filter opciju u `get_lookup_name()` za filtriranje samo po "year".

Slično možete koristiti `get_lookup_value` hook za promenu search vrednosti pre nego se koristi u filteru.

[Sadržaj](00_sadrzaj.md)


# Prilagodjena preraga queryseta po annotacijama

[Sadržaj](00_sadrzaj.md)

```py
from djangoql.schema import DjangoQLSchema, IntField

class UserQLSchema(DjangoQLSchema):

    def get_fields(self, model):
        fields = super(UserQLSchema, self).get_fields(model)
        if model == User:
            fields += [IntField(name='groups_count')]
            return fields

@admin.register(User)
class CustomUserAdmin(DjangoQLSearchMixin, UserAdmin):
    djangoql_schema = UserQLSchema

    def get_queryset(self, request):
        qs = super(CustomUserAdmin, self).get_queryset(request)
        return qs.annotate(groups_count=Count('groups'))
```

Prvo smo dodali "groups_count" annotaciju na queryset koji koristi Django admin u
"CustomUserAdmin.get_queryset()" metodi. On bi trebao da sadrži broj grupa kojima jedan korisnik pripada.

Kada naš queryset sada podigne tu kolonu, možemo filtrirati po njoj. Samo treba da bude uključena u šemu.

U "UserQLSchema.get_fields()" definišemo prilagodjeno integer search polje za "User" model. Njegovo ime treba da je isto sa imenom kolone u queryset-u.

[Sadržaj](00_sadrzaj.md)

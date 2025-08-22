
# Prilagođene suggest_options

[Sadržaj](00_sadrzaj.md)

```py
from djangoql.schema import DjangoQLSchema, StrField

class GroupNameField(StrField):
    model = Groupname = 'name'
    suggest_options = True

    def get_options(self, search):
        return super(GroupNameField, self) \
    
    .get_options(search) \
        .annotate(users_count=Count('user'))\
            .order_by('-users_count')

class UserQLSchema(DjangoQLSchema):def get_fields(self, model):
    if model == Group:
        return ['id', GroupNameField()]
    return super(UserQLSchema, self).get_fields(model)

@admin.register(User)
class CustomUserAdmin(DjangoQLSearchMixin, UserAdmin):

djangoql_schema = UserQLSchema
```

U ovom primeru definisali smo prilagođeno GroupNameField koje sortira sugestije za grupu imena po popularnosti, (broj korisnika u grupi) umesto default alphabetical sortinga.

[Sadržaj](00_sadrzaj.md)

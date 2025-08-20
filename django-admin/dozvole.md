# Dozvole

## Sadržaj

[Nazad](sadrzaj.md)

- Dozvole za model
- Kako proveriti dozvole
- Kako primeniti dozvole
- Django admin i dozvole za model
- Primenite prilagođene poslovne uloge u Django Admin
- Postavka prilagođenog User admina
- Sprečavanje ažuriranja polja User modela
- Uslovno sprečavanje ažuriranja polja
- Sprečavanje ne superuser-a da daju superuser prava
- Django admin User forma u dva koraka
- Dodelite dozvole samo koristeći grupe
- Sprečavanje ne superuser-a od promena njihovih vlastitih dozvola
- Nadjačavanje dozvola na User modelu
- Ograničite pristup prilagođenim akcijama na User modelu
- Kako dozvoliti kreiranje samo jednog objekta u adminu?
- Kako ukloniti ‘Add’/’Delete’ dugme iz admina?

Django pruža dobar okvir za autentifikaciju uz usku integraciju sa Django adminom. Django admin ne primenjuje posebna ograničenja na korisnika admina. To može dovesti doopasnih scenarija koji mogu ugroziti vaš sistem.

Da li ste znali da korisnici koji pripadaju staff-u i koji upravljaju drugim korisnicima u admin-u mogu da uređuju svoje dozvole? Da li ste znali da oni takođe moguda naprave superuser-e? U administratoru Django ne postoji ništa što to sprečava, pa jena vama!

Na kraju ovog vodiča znaćete kako da zaštitite svoj sistem:

- Od eskalacije dozvola sprečavanjem korisnika da uređuju sopstvene dozvole.
- Održavajte dozvole urednim i održivim, prisiljavanjem korisnike da koriste samo grupe.
- Sprečite curenje dozvola kroz prilagođene akcije izričitim sprovođenjem potrebnih dozvola.

### Dozvole za model

Dozvole su škakljive. Ako ne podesite dozvole, sistem izlažete riziku od uljeza,curenja podataka i ljudskih grešaka. Ako zloupotrebite dozvole ili ih previše koristite, rizikujete da ometate svakodnevne operacije.

Django dolazi sa ugrađenim sistemom za potvrdu identiteta. Sistem za potvrdu identitetauključuje korisnike, grupe i dozvole.

Kada se kreira model, Django će automatski stvoriti četiri podrazumevane dozvole za sledeće akcije:

- add: Korisnici sa ovom dozvolom mogu dodati instancu (objekat) modela.
- delete: Korisnici sa ovom dozvolom mogu da izbrišu instancu (objekat) modela.
- change:Korisnici sa ovom dozvolom mogu da ažuriraju instancu (objekat) modela.
- view: Korisnici sa ovom dozvolom mogu da vide instance (objekte) ovog modela. Ova dozvola je bila

dugo očekivana i konačno je dodata u Django 2.1.

Imena dozvola slede vrlo specifičnu konvenciju imenovanja:

<app>.<action>_<modelname>

Razdvojimo to:

- `<app>` je naziv aplikacije. Na primer, model User se uvozi iz aplikacije auth (django.contrib.auth).
- `<action>` je jedna od gore navedenih akcija (add, delete, change ili view).
- `<modelname>` je ime modela, sva mala slova.

Poznavanje ove konvencije imenovanja može vam pomoći da lakše upravljate dozvolama. Na primer, ime dozvole za promenu korisnika je auth.change_user.

Kako proveriti dozvole
Dozvole na modelu se odobravaju korisnicima ili grupama. Da proverite da li odredjeni korisnik ima dozvolu, možete uraditi sledeće:

>>> from django.contrib.auth.models import User
>>> u = User.objects.create_user(username='haki')
>>> u.has_perm('auth.change_user')
False

.has_perm() će uvek vratiti True za aktivnog superuser-a, čak iako dozola zaista ne postoji:

>>> from django.contrib.auth.models import User
>>> superuser = User.objects.create_superuser(
... username='superhaki',
... email='me@hakibenita.com',
... password='secret',
... )

superuser.has_perm('does.not.exist')
True

Kada proverate dozovole za superuser, one se u stvarnosti ne proveravaju, sistem uvek vraća True.

17.2.1 Kako primeniti dozvole
Django modeli sami po sebi ne primenjuju dozvole. Jedino mesto gde se dozvole primenjuju podrazumevano je Django admin. Razlog zbog kojeg modeli ne primenjuju dozvole je taj što model obično nije svestan korisnika koji izvršava akciju. U Django app, korisnik se obično dobija iz request-a. Zbog toga se, najčešće, dozvole primenjuju na sloju prikaza.

Na primer, da biste sprečili korisnika bez dozvole da pristupi pogledu koji prikazujekorisničke informacije, uradite sledeće:

from django.core.exceptions import PermissionDenied
def users_list_view(request):

if not request.user.has_perm('auth.view_user'):
raise PermissionDenied()

Ako se korisnik, koji je podneo zahtev, prijavio se i bio potvrđen identitet, tada će request.user biti instanca User, ako se korisnik nije prijavio, tada će request.user biti instanca AnonimousUser. Ovo je poseban objekat koji Django koristi za označavanje neovlašćenog korisnika. Korišćenje has_perm na AnonimousUser uvek vraća False.

Ako korisnik koji pravi zahtev nema dozvolu view_user, tada pokrećete izuzetak PermissionDenied i klijentu se vraća odgovor sa statusom 403.

Da bi olakšao primenu dozvola u prikazima, Django nudi dekorator nazvan permission_required koji radi istu stvar:

from django.contrib.auth.decorators import permission_required
@permission_required('auth.view_user')
def users_list_view(request):

pass

Da biste primenili dozvole u šablonima, trenutnim korisničkim dozvolama možete pristupiti putem posebne promenljive šablona, naziva perms. Na primer, ako želite da prikažete dugme za brisanje samo korisnicima sa dozvolom za brisanje, učinite sledeće:

{% if perms.auth.delete_user %}

<button>Delete user!</button>
{% endif %}

Neke popularne nezavisne aplikacije, poput Django rest framework-a, takođe pružaju korisnu integraciju sa dozvolama za Django model.

Django admin i dozvole za model
Django admin ima vrlo tesnu integraciju sa ugrađenim sistemom za potvrdu identiteta, aposebno dozvole za model. Django admin primenjuje dozvole za model:

- Ako korisnik nema dozvole za model, neće moći da ga vidi ili da mu pristupi u adminu. 
- Ako korisnik ima dozvolu za pregled i promenu na modelu, tada će moći da pregleda i ažurira instance,

ali neće moći da dodaje nove instance ili briše postojeće.

Sa odgovarajućim dozvolama, admin korisnici će manje grešiti, a uljezi će teženaneti štetu.

Primenite prilagođene poslovne uloge u Django Admin
Jedno od najranjivijih mesta u svakoj aplikaciji je sistem za potvrdu identiteta. U Django app ovo je User model. Dakle, da biste bolje zaštitili svoju aplikaciju, krenućete sa korisničkim modelom.

Prvo treba da preuzmete kontrolu nad admin stranicom User modela. Django već ima vrlo lepu admin stranicu za upravljanje korisnicima. Da biste iskoristili taj sjajni posao,proširićete ugrađeni User model za admin korisnika.

17.4.1 Postavka prilagođenog User admina
Da biste obezbedili prilagođenog admina za User model, morate da opozovete registracijupostojećeg User modela koji nudi Django i da registrujete svoj:

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
# Deregistrujte osnovni user model
adminadmin.site.unregister(User)
# Registrujte svoj prilagodjeni user model admin:
@admin.register(User)
class CustomUserAdmin(UserAdmin):

pass

Vaš CustomUserAdmin je prošireni Djangov UserAdmin. Ako se sada prijavite na Django admin http://127.0.0.1:8000/admin/auth/user, videćete nepromenjen User admin. Proširenjem UserAdmin-a, možete da koristite sve ugradjene mogućnosti koje pruža Django admin.

17.4.2 Sprečavanje ažuriranja polja User modela
Admin forme bez nadzora glavni su kandidat za užasne greške. Korisnik koji pripada staff-u može lako ažurirati instancu User modela putem admina na način koji aplikacijane očekuje. U većini slučajeva korisnik neće ni primetiti da nešto nije u redu. Takve greške je obično vrlo teško pronaći i otkloniti.

Da biste sprečili da se takve greške dogode, možete sprečiti admine da menjaju određena polja u User modelu. Ako želite da sprečite bilo kog korisnika, uključujući superuser-a, da ažurira polje,možete ga označiti samo za čitanje. Na primer, polje date_joined se postavlja kada se korisnik registruje. Korisnik ove informacije nikada ne sme da menja, pa ćete ih označiti kao samo za čitanje:

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

readonly_fields = [
'date_joined',

]

Kada se polje doda u readonly_fields, ono se neće moći uređivati u podrazumevanoj admin formi. Kada je polje označeno samo za čitanje, Django će prikazati ulazni element kao onemogućen.

Ali, šta ako želite da sprečite samo neke korisnike da ažuriraju polje User modela?

17.4.3 Uslovno sprečavanje ažuriranja polja
Ponekad je korisno ažurirati polja User modela direktno u adminu. Ali ne želite da dozvolite bilo kom korisniku da to radi, želite da dozvolite samo superuser-ima da torade.

Recimo da želite da sprečite korisnike koji nisu superuser-i da promene username korisnika. Da biste to uradili, potrebno je da izmenite formu za promenu koji je generisao Django i onemogućite polje za username na osnovu trenutnog korisnika.

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

def get_form(self, request, obj=None, **kwargs):
form = super().get_form(request, obj, **kwargs)
is_superuser = request.user.is_superuser
if not is_superuser:

form.base_fields['username'].disabled = True
return form

Da biste izvršili prilagođavanje obrasca, nadjačajte get_form()metodu. Ovu metodukoristi Django za generisanje podrazumevanog obrasca za promenu modela. Da biste uslovno onemogućili polje, prvo dobavite podrazumevani obrazac koji je generisao Django, a zatim, ako korisnik nije superuser, onemogućite username polje.

Sada, kada korisnik koji nije superuser pokuša da izmeni username korisnika, polje usernameće biti onemogućeno. Svaki pokušaj izmene username polja putem Django admina neće uspeti. Kada superuser pokuša da izmeni username korisnika, polje username će moćida se uređuje i ponašaće se onako kako se očekuje.

17.4.4 Sprečavanje ne superuser-a da daju superuser prava
Superuser je vrlo jaka dozvola koja se ne sme davati olako. Međutim, bilo koji korisnik sa dozvolom za promene na User modelu može od bilo kog korisnika napraviti superuser-a, uključujući i same sebe. To se protivi celoj svrsi sistema dozvola, paželite da zatvorite ovu rupu.

Na osnovu prethodnog primera, da biste sprečili korisnike koji nisu superuser-i da daju sami sebi prava superusrer-a, dodajete sledeće ograničenje:

from typing import Set
from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

def get_form(self, request, obj=None, **kwargs):
form = super().get_form(request, obj, **kwargs)
is_superuser = request.user.is_superuser
disabled_fields = set() type: Set[str]
if not is_superuser:

disabled_fields |= {'username', 'is_superuser',}

for f in disabled_fields:
if f in form.base_fields:

form.base_fields[f].disabled = True

return form

U get_form metodi možete inicijalizovati prazan set disabled_fields koji će sadržati polja za onemogućavanje. Set je struktura podataka koja sadrži jedinstvene vrednosti. U ovom slučaju ima smisla koristiti set, jer polje možete samo jednom onemogućiti. Operator |= se koristi za izvođenje ažuriranja na setu. Dalje, ako korisnik nije superuser, u set dodajte dva polja (username i is_superuser). Oni će sprečiti osobe koje nisu superuser-i da se naprave superuser-ima.

Na kraju, prelistate polja u skupu, označite ih kao onemogućena i vratite formu.

17.4.5 Django admin User forma u dva koraka
Kada kreirate novog korisnika u Django adminu, prolazite kroz formu u dva koraka. U prvom koraku popunjavate username i password. U drugom koraku ažurirate ostatak polja. Ovaj postupak u dva koraka jedinstven je za User model. Da biste prilagodili ovaj jedinstveni proces, morate da potvrdite da polje postoji pre nego što pokušate da ga onemogućite. U suprotnom, možete dobiti KeyError.

17.4.6 Dodelite dozvole samo koristeći grupe
Način upravljanja dozvolama vrlo je specifičan za svaki tim, proizvod i kompaniju. Otkrio sam da je lakše upravljati dozvolama sa grupama. U svojim projektima stvaramgrupe za podršku, urednike sadržaja, analitičare itd. Otkrio sam da upravljanje dozvolama na korisničkom nivou može biti prava gnjavaža. Kada se dodaju novi modeli ili kada se promene poslovni zahtevi, zamorno je ažurirati svakog pojedinačnog korisnika.

Da biste upravljali dozvolama samo pomoću grupa, morate da sprečite korisnike da dodeljuju dozvole drugim korisnicima ili sami sebi. Umesto toga, želite da dozvolite samo pridruživanje korisnika grupama.

Da biste to uradili, onemogućite polje user_permissions za sve korisnike koji nisu superuser-i:

from typing import Set
from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

def get_form(self, request, obj=None, **kwargs):
form = uper().get_form(request, obj, **kwargs)
is_superuser = request.user.is_superuser
disabled_fields = set() type: Set[str]
if not is_superuser:

disabled_fields |= {'username', 'is_superuser', 'user_permissions',}
for f in disabled_fields:

if f in form.base_fields:
form.base_fields[f].disabled =
True

return form

Za primenu drugog poslovnog pravila koristite potpuno istu tehniku kao i prethodno. U sledećim odeljcima ćete primeniti složenija poslovna pravila kako biste zaštitili svoj sistem.

17.4.7 Sprečavanje ne superuser-a od promena njihovih vlastitih dozvola
Jaki korisnici su slaba tačka. Oni imaju jake dozvole, i potencijalno oštećenje koje mogu izazvati može biti značajno. Za sprečavanje eskalacije prava u slučaju upada, možete sprečiti korisnike da menjaju svoje dozvole:

from typing import Set
from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

def get_form(self, request, obj=None, **kwargs):
form = super().get_form(request, obj, **kwargs)
is_superuser = request.user.is_superuser
disabled_fields = set() type: Set[str]
if not is_superuser:

disabled_fields |= {'username', 'is_superuser', 'user_permissions',}
# Prevent non-superusers from editing their own permissions
if (not is_superuser and obj is not None and obj == request.user)

disabled_fields |= {
'is_staff', 'is_superuser', 'groups', 'user_permissions',

}
for f in disabled_fields:

if f in form.base_fields:

form.base_fields[f].disabled =
True

return form

Argument obj je instanca na kojoj trenutno radite:

- Kada je obj is None, forma se koristi za kreiranje novog korisnika.
- Kada je obj is not None, forma se koristiti za ažuriranje postojećeg korisnika.

Da biste proverili da li korisnik koji podnosi zahtev radi na sebi, uporedite sa request.user sa obj. Kada je za korisnika koji pravi zahtev request.user==obj, to znači da se korisnik ažurira. U ovom slučaju onemogućavate sva osetljiva polja koja se mogu koristiti za dobijanje dozvola.

Sposobnost prilagođavanja forme na osnovu objekta je veoma korisna. Može se koristiti za sprovođenje složenih poslovnih uloga.

17.4.8 Nadjačavanje dozvola na User modelu
Ponekad može biti korisno potpuno nadjačati dozvole na User modelu Django admina.Uobičajeni scenario je kada koristite dozvole na drugim mestima i ne želite da korisnici osoblja unose promene u admin.

Django koristi hook metode za četiri ugrađene dozvole. Interno, kuke koriste trenutne dozvole korisnika za donošenje odluke. Možete da izmenite ove kuke i donesete drugačiju odluku.

Da biste sprečili korisnike osoblja da izbrišu instancu modela, bez obzira na njihove dozvole, možete učiniti sledeće:

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

def has_delete_permission(self, request, obj=None):
return False

Baš kao i kod get_form(), obj je instanca kojom se trenutno upravlja:

- Kada je obj is None, korisnik je zatražio prikaz liste.
- Kada obj is not None, korisnik je zahtevao prikaz strane za promenu određene instance.

Imati instancu modela u ovim hook metodama vrlo je korisno za primenu dozvola na nivo instance za različite tipove akcija. Evo i drugih slučajeva upotrebe:

- Sprečavanje promena tokom radnog vremena
- Implementacija dozvola na nivou instance

17.4.9 Ograničite pristup prilagođenim akcijama na User modelu
Prilagođene admin akcije zahtevaju posebnu pažnju. Django njih ne poznaje, tako da impodrazumevano ne može ograničiti pristup. Prilagođena akcija biće dostupna bilo kom adminu sa bilo kojom dozvolom na modelu.

Da bismo ilustrovali, dodajte praktičnu admin akciju da biste više korisnika označili kao aktivne:

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.admin import
UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

actions = [
'activate_users',

]
def activate_users(self, request, queryset):

cnt = queryset.filter(is_active=False).update(is_active=True)
self.message_user(request, 'Activated {} users.'.format(cnt))

activate_users.short_description = 'Activate Users' type: ignore

Koristeći ovu akciju, staff korisnik može označiti jednog ili više korisnika i aktivirati ih sve odjednom. Ovo je korisno u svim vrstama slučajeva, na primer ako imate grešku u procesu registracije i trebate da aktivirate korisnike na veliko. Ova akcija ažurira korisničke informacije, tako da želite da ih mogu koristiti samo korisnici sa dozvolama za promenu.

Django admin koristi get_action metodu za dobijanje instance akcije. Da biste sakrili metodu activate_users() od korisnika bez dozvole za promenu User modela, nadjačajte metodu get_action():

from django.contrib import admin
from django.contrib.auth.models import User
from django.contrib.auth.adminimport UserAdmin
@admin.register(User)
class CustomUserAdmin(UserAdmin):

actions = [
'activate_users',

]
def activate_users(self, request, queryset):

assert request.user.has_perm('auth.change_user')
cnt = queryset.filter(is_active=False).update(is_active=True)
self.message_user(request, 'Activated {} users.'.format(cnt))

activate_users.short_description = 'Activate Users' type: ignore
def get_actions(self, request):

actions = super().get_actions(request)
if not request.user.has_perm('auth.change_user'):

del actions['activate_users']
return actions

get_action() vraća OrderedDict. Ključ je ime akcije, a vrednost funkcija akcije. Da biste prilagodili
povratnu vrednost, nadjačajte funkciju activate_users, preuzmete originalnu vrednost i u zavisnosti od
korisničkih dozvola uklonite.

Za staff korisnike bez dozvole change_user(), akcija activate_users se neće pojaviti upadajućem meniju
akcija.

17.4.10 Kako dozvoliti kreiranje samo jednog objekta u adminu?
UMSRA traži da ograničite broj Category objekata na 1. Oni žele da svaki entitet bude iste kategorije:

MAX_OBJECTS = 1
def has_add_permission(self, request):

if self.model.objects.count() >= MAX_OBJECTS:
return False

return super().has_add_permission(request)

Ovo će sakriti add dugme sve dok je ispunjen uslov. Možete postaviti MAX_OBJECTS na bilo koju vrednost.

17.4.11 Kako ukloniti ‘Add’/’Delete’ dugme iz admina?
UMSRA je dodala sve Category i Origin objekte i želi da onemogući dalje dodavanje ilibrisanje objekata. To ćemo postići nadjačavanjem has_add_permission i has_delete_permissionu Django adminu:

def has_add_permission(self, request):
return False

def has_delete_permission(self, request, obj=None):
return False

Primetite uklanjanje add dugmeta sa listview strane. Add i delete dugmad će takodje biti uklonjena sa changeview strane.

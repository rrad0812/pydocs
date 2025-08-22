# Validacija prilagođene forme

## Sadržaj

[Nazad](sadrzaj.md)

- [Ugrađena validacija forme](#ugrađena-validacija-forme)
- [Validacija pojedinačnog polja](#validacija-pojedinačnog-polja)
- [Validacija više polja](#validacija-više-polja)
- [Zaključak](#zaključak)

Dobra je praksa ne prihvatati podatke dostavljene od korisnika takvi kakvi jesu.  Podaci forme moraju biti potvrđeni kako bi se osiguralo da su podaci validni i da ispunjavaju sve neophodne uslove. U Django-u se ova provera vrši na dva nivoa - nivou polja i nivou forme, a oba omogućavaju dodavanje prilagođene provere.

### Ugrađena validacija forme

Django nam pomaže tako što automatski potvrđuje polja forme. To znači da možemo biti sigurni da su dostavljeni podaci tipa koji očekuje polje forme a ako nisu doći će do greške.

Ako, na primer, očekujemo da korisnik pošalje datum, a umesto toga podnese ’string', pojaviće se greška koja govori korisniku da ’string’ nije važeći datum.

Django takođe može da obezbedi automatsku proveru valjanosti u zavisnosti od polja forme. U Django dokumentima možete pronaći koje su validacije dostupne za svaki tip polja forme.

Na nivou forme, Django može da vrši automatsku proveru jedinstvenosti između više polja. Ove provere zavise od načina na koji smo podesili naše modele i pomažu u osiguravanju da korisnici ne šalju duplikate podataka ako mi to ne želimo.

Ali šta ako želite da budete sigurni da ono što korisnik podnese ispunjava neki drugi uslov koji nije automatski potvrđen? Na primer, ako smo očekivali da će korisnik predati palindrom (reč ili faza istu napred i nazad), kako bismo mogli da proverimo dali je to tačno? Ili, ako bismo samo želeli da korisnici predaju
imena ljudi koji imaju imena i prezimena koja počinju istim slovom, kako bismo to mogli proveriti?

Istražimo oba ova scenarija.

[Sadržaj](#sadržaj)

### Validacija pojedinačnog polja

Možemo da proverimo da li je reč koju je poslao korisnik palindrom tako što ćemo izvršiti dodatnu proveru na nivou polja. Prvo, pogledajmo naš model palindroma koji ima samo jedno polje za našu reč.

`#models.py`

```py
from django.db import models

class Palindrome(models.Model):
    word = models.CharField(max_length=250, blank=False)
    
    def _ str_ (self):
        return self.word
```

Dalje, napravićemo formu da bi korisnik mogao da preda palindrom.

Nasleđujemo od `ModelForm` jer želimo da sačuvamo korisničke podatke na modelu "Palindrome". U ovom trenutku, forma ima samo Django-ovu automatsku proveru forme. Ako bismo formu povezali sa prikazom, šta bi se dogodilo kada korisnik pošalje reč u formu koja nije palindrom?

Čuva se u bazi podataka. To nije ono što želimo, ali još uvek nismo učinili ništa da to sprečimo! Da bismo proverili da li je poslata reč palindrom, moramo da modifikujemo našu `PalindromeForm` tako da uključuje test koji proverava da li je reč ista napred i nazad.

`forms.py`

```py
from django.forms import ModelForm, ValidationError # Import ValidationError
from .models import Palindrome

class PalindromeForm(ModelForm):

    def clean_word(self):
        # Uzmi korisnički unešenu reč iz rečnika cleaned_data
        data = self.cleaned_data["word"]
        
        # Proveri da li je reč palindrom
        if data != "".join(reversed(data)):
            # Ako nije podigni grešku
            raise ValidationError("The word is not a palindrome")

        # Vrati podatak, uvek, čak iako nije menjan
        return data

    class Meta:
        model = Palindrome
        fields = ["word"]
```

Samo treba da izvršimo dodatnu proveru na nivou polja da bismo proverili da li je reč palindrom. Da bismo dodali ovu dodatnu validaciju polja, definišemo metodu `clean_word` koja deluje samo na polju word. Ova metoda će se pozvati nakon Djangove automatske provere valjanosti koja za nas stvara rečnik `clean_data`.
"Word" dobijemo iz `clean_data` i čuvamo je kao "data".

Sada konačno moramo da proverimo da li je reč koju je korisnik poslao palindrom upoređujući je sa obrnutom verzijom samog sebe. Ako je reč palindrom, provera prolazi i metoda vraća izvorni podatak. Ako reč nije palindrom, podižemo `ValidationError` sa prilagođenom porukom o grešci. Ova poruka će se dodati
u formu i prikazati korisniku kada se ponovo prikaže sa greškama zbog neuspešnog potvrđivanja forme.

Ako je naša forma imala više polja koja smo želeli da potvrdimo, samo treba da dodamo metodu `clean_"fieldname"` za svako polje za koje želimo da dodamo prilagođenu proveru.

Mogli smo da uključimo i više provera validacije u našu metodu "clean_word" ako smo, na primer, takođe želeli da se uverimo da je svaki podneti palindrom duži od četiri slova.

[Sadržaj](#sadržaj)

### Validacija više polja

Pogledajmo sada primer forme koji ima polja za first_name i last_name i želimo da oba počinju istim slovom. U ovom slučaju, pošto moramo da izvršimo proveru validacije koja uključuje više od jednog polja, naša validacija se vrši na nivou forme.

Naš model će imati dva CharField polja first_name i last_name.

`models.py`

```py
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=250, blank=False)
    last_name = models.CharField(max_length=250, blank=False)
```

Kao i prethodni primer, započećemo sa osnovnom formom koji nema prilagođenu proveru valjanosti.

`forms.py`

```py
from django.forms import ModelForm
from .models import Person

class PersonForm(ModelForm):

    class Meta:
        model = Person
        fields = ["first_name", "last_name"]
```

Kao i ranije, naša forma će omogućiti da se bilo koji niz znakova pošalje za "first_name" i "last_name" i kreiraće i sačuvati instancu modela sa tim vrednostima bez grešaka.

Da bismo dodali proveru valjanosti, zamenimo metodu `clean` u formi.

`forms.py`

```py
from django.forms import ModelForm, ValidationError # Import ValidationError
from .models import Person

class PersonForm(ModelForm):
    
    def clean(self):
        # Uzmi korisnički unešena imena iz cleaned_data rečnika
        cleaned_data = super().clean()
        first_name = cleaned_data.get("first_name")
        last_name = cleaned_data.get("last_name")

        # Proveri da li je prvo slovo imena i prezimena isto
        if first_name[0].lower() != last_name[0].lower()
            # Ako nije podigni grešku
            raise ValidationError("The first letters of the names do not match")

        return cleaned_data
    
    class Meta:
        model = Person
        fields = ["first_name", "last_name"]
```

Ovo se razlikuje od prethodnog primera gde smo kreirali metodu `clean_<ime polja>` jer sada gledamo više polja umesto jednog polja. Kada zamenjujemo `clean` na `ModelForm`, prvo pozivamo metodu `clean` roditeljske klase da bismo mogli imati pristup `clean_data`.

Ostali tipovi formi možda neće vratiti `cleaned_data` iz metode clean na roditeljskoj klasi. U tom slučaju nazivamo `super().clean()` samostalno bez dodeljivanja i pristupimo očišćenim podacima putem `self.cleaned_data`.

Jednom kada smo dobili `clean_data` možemo dobiti vrednosti "first_name" i "last_name". Tada možemo izvršiti stvarnu proveru valjanosti da li ime i prezime počinju istim slovom. Ako se ne podudaraju, pokrećemo grešku validacije koja se dodaje u formu i prikazuje korisniku.

[Sadržaj](#sadržaj)

### Zaključak

Validacija prilagođene forme vrši se na različite načine, u zavisnosti od toga da li je validacija potrebna na nivou polja za jedno polje forme ili na nivou forme za više polja.

Da bi se dodala validacija za jedno polje, u formu se dodaje metoda `clean_<ime_polja>` za željeno ime polja koje treba potvrditi. Ovom metodom treba dobiti podatke o polju izrečnika `self.cleaned_data` i vratiti podatke o polju bez obzira da li su izmenjeni ili ne. Uslovni izrazi se dodaju metodi za izvođenje željene provere valjanosti i podizanje ValidationErrors ako provera valjanosti ne uspe.

Za validaciju odnosa između više polja poziva se metoda `clean` na formi. Podaci polja postaju dostupni pozivom metode roditeljske klase clean putem `super().clean()` koji vraća očišćene podatke polja za `ModelForm`. Sada se može pristupiti svim podacima polja iz forme i izvršiti željena provera valjanosti da bi se po potrebi pojavila `ValidationError`.

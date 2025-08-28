
# Rad sa Django ModelForm-ama

Django dolazi sa opterećenim ugrađenim funkcijama koje olakšavaju brzo da izgradite veb aplikacije iz nule.To je, ako tačno znate kako da izgradite ono što vam treba.

Dok Django dolazi sa velikom dokumentacijom sa primerima, i dalje otkrijem da je često lakše naučiti pravu (ili barem uobičajenu) sintaksu za funkciju koja mi je potrebna u postovima bloga i tutorijalima. Čini se da je mnogo izostavljeno (ili jednostavno pretpostavljeno da je to opšte znanje) u dokumentaciji Django. Naravno, veličina dokumentacije ne olakšava pronalažemju stvari.

SLUČAJ U TOČKU: ModelFor-me. Dokumentacija za ModelForme ima prilično objašnjenje osnovnih upotreba i nekih konkretnijih slučajeva upotrebe, ali im je nedostajala u nekim važnim oblastima. Preći ću osnove da daju novopridošli su malo pozadine, a zatim detaljno objasniti delove koje sam morao da istražim za sebe.

Kao i uvek, možete dobiti kod koji sam napisao za ovaj članak o svom GitHub repou.

## Modeli

Django modeli su najlakši način za kreiranje tabela baze podataka sa ugrađenim CRUD funkcionalnošću.Modeli su paralelno sa tabelama baze podataka i definisani su kao klase koje nasleđuju iz Django.DB.Models.Model. Atributi modela (ekvivalentno poljima) mogu se definisati kao atributi klase koristeći odgovarajuću klase polja modela. Lista svih dostupnih polja možete pronaći u Django dokumentaciji. Možemo da stvorimo novi model koji opisuje članak vesti, sa atributima naslov i telo, kao:

```py
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=200)
    body = models.TextField()
```

Jedina značajna razlika između `Charfied` koji smo koristili za naslov i `TextField` koji smo koristili za telo je da Django preporučuje tekstualno polje za velike tekstove.

Pomoću našeg novog modela možemo stvoriti članke i skladištiti ih u našoj bazi podataka.Postoji nekoliko načina na koje to možemo učiniti, ali onaj koji nas ovde zanima je sa klijentom bočnim oblikom. Pre nego što pogledamo `ModelForm`, brzo pogledajmo kako Django rukuje redovnim `Form`-ama.

## Forme

Ako nikada ranije niste koristili `Form`-e Djanga, predlažem da pogledate moj članak "Ronjenje u forme Django". Bilo kako bilo, daću kratak rezime sa primerom da bismo mogli nastaviti.

Recimo da želimo da stvorimo formu za korisnike da kreiraju nove članke.Moraćemo imati polja za svaki atributi u našem modelu ako želimo da ih korisnik snabdeva: naslov i telo. Možemo definisati sopstvenu klasu forme koji nasleđuje klase formi. Obrazloženje je da lako kreiranje i učini HTML, postavlja atribute i potvrđuje podatke.

```py
class ArticleForm(forms.Form):
  title = forms.CharField(
    label='Title: ', 
    widget= forms.TextInput(attrs={'maxlength': 50, 'class':'form_input'})
  )
  body = forms.CharField(
    label='Body: ', 
    widget= forms.Textarea(attrs={'class':'form_input'})
  )
```

Ovde govorimo Django da želimo da naša forma ima CharField (koji je prikazan kao `<input type = "Text">`) sa maksimalnom dužinom od 50 i klase
`Form_input`. Takođe definišemo `Charfield` za telo, ali kažemo Djangu da želimo da koristimo Textarea za ovo, jer telo članka može biti mnogo duže od naslova.

Primetićete da su oba polja u našoj formi vrlo slična onima koji se koriste u našem modelu. Ovaj kod je suvišan i ako je naš model bio složeniji, to bi bio vrlo neefikasan način za rad. Videćemo kako možemo da izbegnemo da se ponavljamo, u odeljku `ModelForm`.

Sada kada imamo svoju formu, možemo da ga učinimo u šablonu stvarajući ga instancu u našem mišljenju, prenoseći ga na funkciju rendera koristeći kontekst rečnik pod ključem "obrazac", gde nam treba u šablonu.

Postoje i drugi načini da se to učini u šablonu, od kojih ćemo se kasnije prikazati.

Ako bilo ko od njih zbunjuju slobodno da proverite odeljak prikazivanja mog oblika, povezan gore.

Jednom kada se sve bude prikazano, još uvek moramo da obradimo i potvrdimo podatke primljen od obrasca, koji ćemo uraditi u pogledu.Nakon toga želimo da stvorimo novi predmet članak i dodamo ga u bazu podataka koristeći podatke koje je korisnik isporučio.Sve ovo možemo učiniti ručno koristeći klase oblika i modela koji smo napisali, ali to nije baš efikasno.U narednim nekoliko odeljcima videćemo kako to možemo da učinimo pomoću Modelforms, ali prvo, tvrdi način:

```py
if request.method == 'POST':
    # create a form instance and populate it with data from the   request:
    form = ArticleForm(request.POST)
    # check whether it's valid:
    if form.is_valid():
        # Create a new Article instance using the data
        new_article = Article(
        title=form.cleaned_data['title'], 
        body=form.cleaned_data['body'])
        # Save the article to the database
        new_article.save()
        # return some response:
        return HttpResponse("Thanks for your data!")
# If the form was invalid send the user back to fix it
else:
  return render(request, 'formyapp/formPage.html',
  context
)
```

Kao u svakom pogledu koji deluje sa formom, imamo odvojeno ponašanje za nadoknadu i postavljanje zahteva. Jednom kada primimo podatke u zahtevu POST, potvrđujemo obrazac koristeći `is_valid()` funkciju i ako je obrazac validan, stvaramo predmet članak sa podacima obrasca. Konačno, mi je novi objekt spasili u bazu podataka.

Napomena: `Form.cleaned_data` je rečnik koji sadrži podatke nakon što je očišćen (na primer pravilno iskorištavajući naslov) i potvrđen.Više o tome možete pročitati u dokumentaciji Django.

Primjetićete da u Kodeksu napisali smo ni na koji način ni na koji način napisali nismo potvrdili ili očistili instancu modela (članak).Postoje mnoge situacije u kojima bismo mogli imati posebna ograničenja za naše oblike koji nisu relevantni za modele i obrnuto.To je jedan od razloga da postoje odvojene metode za potvrđivanje oblika i modela koji potvrđuju.Neki primeri:

Mogli bismo da imamo model za korisnike naše stranice i obrazac za njih da se koriste kako bi se prijavili.Dok se ovako formira često imaju 2 polja lozinke (jedan za unos lozinke i drugi za potvrdu) Nema potrebe da se oboje čuvaju u bazi podataka.Obrazac potvrđuje da se lozinke podudaraju, kako bi se osigurao da korisnik nije pogrešio, a zatim čuva jednu lozinku za tog korisnika.

S druge strane, možda bismo želeli da osiguramo da su svi naslovi članaka jedinstveni.Obrazac nema načina (dok se drži na principima OOP-a i ostavljajući zabrinutost od strane članaka sa klasom članka) znanja da li je članak sa određenim naslovom već već postojati.Provera jedinstvenosti podataka bila bi posao validacije modela.

Takođe možemo da koristimo čistu metodu obrasca za potvrđivanje međuzavisnih polja ili da koristite metodu `Clean_fieldName ()` da biste očistili određeno polje.Isto tako, možemo da koristimo čistu metodu modela da rade stvari poput postavljanja podrazumevanih vrednosti za polja ostavljena prazna.

## ModelForm

Kao što ste videli u prethodnom odeljku stvaranje forme koja će se koristiti sa našim modelima može dodati ponavljajući kod i dodatnu složenost na naš projekat. Django prepoznaje potrebu za kreiranjem objekata baze podataka iz korisnika podataka i pruža nam zgodna klasa koja se naziva `ModelForm` koji olakšava mnogo ponavljajućih delova.

Django `ModelForm` klasa daje nam sve vrste ugrađene funkcije obrasca koje dobija iz redovne klase obrasca (kreiranje i donošenjem obrasca, lako validaciju obrasca, jednostavne validažne obrasce itd.) I kombinuje da sa funkcijom možemo da koristimo i da lako kreiramo i sačuvamo objekt baze podataka.Redomorajmo naš primer naših artikala, ovaj put pomoću Modelforms.

Možemo da stvorimo klasu forme za naš model člana stvarajući novu klasu koja nasleđuje klase `ModelForm`.Django `ModelForm` koristi ugniježđenu `Meta` klasu da biste definisali mnoge atribute i ponašanja forme.

Kreirajmo Modelform za članke:

```py
class ArticleForm(ModelForm):
    class Meta:
        model = Article
        fields = ‘__all__’
```

Atribut `model` definiše koji je model forme, a atribut `fields` definiše koji od atributa modela želimo da koristimo kao polja u našoj formi.

Možemo odabrati sve, kao što imamo gore, ili dati listu polja da biste je uključili:

```py
class ArticleForm(ModelForm):
    class Meta:
        model = Article
        fields = ['title', 'body']
```

ili lista polja `exclude` da se isključe:

```py
class ArticleForm(ModelForm):
    class Meta:
        model = Article
        exclude = []
```

Zatim moramo da stvorimo novi instancu forme:

```py
def formRender(request):
    context = {}
    form = ArticleModelForm()
    context['form'] = form
    return render(request, 'formyapp/formPage.html', context)
```

Zatim možemo preneti formu na više načina.Objašnjavam glavne detalje u svom članku sa formama, zato slobodno proverite ako želite više detalja.Formu ćemo pisati sa `{{form.as_p}}` u šablonu Django i uzimmamo sledeći HTML:

```html
<form action="" method="post">
    <input type="hidden" name="csrfmiddlewaretoken"
    value="BwpXGRXCdUC5p9f0FeuRKzQdQHDANpTVX2EuzLqFup60EU93BsAb">
    <p>
        <label for="id_title">Title:</label>
        <input type="text" name="title" maxlength="200" 
            required="" id="id_title">
    </p>
    <p>
        <label for="id_body">Body:</label>
        <textarea name="body" cols="40" rows="10" required="" 
            id="id_body"></textarea></p>
        
    <input type="submit" value="Submit">
</form>
```

Da bismo obradili podatke, dodaćemo sličnu logiku kao i pre, proveravanje načina zahteva i postupanje u skladu s tim.Za zahtev za dobijanje, učinićemo kako smo radili gore. Za POST zahtev kreiraćemo formu zasnovanu na podacima koje smo dobili, potvrđujemo ih koristeći metodu ModelForm-a `is_valid()`, a zatim sačuvajte predmet u bazi podataka tako što ćete uštedeti obrazac. Da vidimo kako izgleda kod i onda ću objasniti na šta mislim.

```py
def formRender(request):
    context = {}
    if request.method == 'POST':
        # create a form instance and populate it with data from the rquest:
        form = ArticleModelForm(request.POST)

        # check whether it's valid:
        if form.is_valid():
            form.save()
            return HttpResponse("Thanks for your data!")

        # If the form was invalid send the user back to fix it
        else:
            context['form'] = form
            return render(request, 'formyapp/formPage.html', 
            context)

    # if a GET (or any other method) we'll create a blank form
    else:
        form = ArticleModelForm()
        context['form'] = form
        return render(request, 'formyapp/formPage.html', context)
```

Spremanje ModelForme na ovaj način radi nekoliko stvari:

1. Ako obrazac još nije potvrđen, sada će biti potvrđen.(Ako je obrazac nevažeći, postavi se greška)
2. Polja koja su bila neobavezno u formi daju njihovu zadanu vrednost ako postoji.
3. Nova instanca modela je sačuvana i dodata u bazu podataka.

> [Note]
>
> `save()` prihvata obavezu parametara koji podrazumevano je istinito i određuje da li je instanca odmah dodata u bazu podataka. Ako postavimo da se posvećujemo lažno, `save()` vraća instancu, možemo dalje da obradimo dalje u kojem slučaju moramo ponovo da nazovemo `save()` kada budemo spremni da promenimo instancu.

Metoda save() takođe uzima opcionalni parametar instance (koji bi trebalo da bude instanca modela) koji se može koristiti za ažuriranje podataka o određenim primjeri.

### Prilagođavanje obrasca

Mnogo je razloga da bismo želeli da promenimo formu sa zadanih vrednosti
Dao Django.Možda bismo želeli da postavimo sopstvene etikete ili dodamo časove koje možemo da koristimo za stil.Sve se to može učiniti unutar Meta klase Modelform-a.Možemo podesiti atribut vidgeta da sadrže rečnik sa imenama polja koje želimo da promenimo i vidžet koji želimo da koristimo.

```py
class ArticleForm(ModelForm):
    class Meta:
        model = Article
        fields = ‘__all__’
        widgets = {
            'body': forms.Textarea(attrs={'class':'form_input'})}
```

Ovo je očigledno vrlo slično način na koji smo to uradili za redovnu klasu formi, ali mislio sam da ću vam uštedjeti probleme da se osvrnem na dokumentaciju. Ako želite da vidite koje se druge karakteristike mogu prilagoditi, ipak, imaju izgled.

## Zaključak

Pronalaženje tačne sintakse koju tražite često se osećate kao lov na blago i da budemo iskreni: Ponekad sve što stvarno želite je najbrži odgovor, tako da možete da se vratite na stvaranje neverovatne aplikacije na kojoj radite.Nadam se da sam mogao da vas uštedim neko vreme ili u najmanju ruku pružim zanimljivo čitanje.Vidimo se uskoro!

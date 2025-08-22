# Prilagodjena polja za pretragu

[Sadržaj](00_sadrzaj.md)

Dublje prilagodjenje pretrage može biti postignuto sa prilagodjenim `search` poljima. Prilagodjena `search` polja mogu biti korišćenja:

- za pretragu po annotacijama,
- za definisanje `suggest_options`, ili
- definisanju pune prilagodjene search logike.

U `djangoql.schema`, DjangoQL definiše sledeća polja bazne klase koje mogu biti nasledjene za definisanje sopstvenog ponašanja:

- IntField
- FloatField
- StrFieldBoolField
- DateField
- DateTimeField
- RelationField

Ovde su primeri za česte slučajeve upotrebe:

[Sadržaj](00_sadrzaj.md)

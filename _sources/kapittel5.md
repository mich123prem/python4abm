---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
mystnb:
  number_source_lines: True
kernelspec:
  display_name: Python 3
  language: python
  name: python3 
---

# 5. Løkker, lister, tupler, og dict'er

I forrige kapittel så vi hvordan et program beslutter hvilke
kodelinjer som skal og som ikke skal utføres, ved å teste på 
**betingelser**.
Slike betingelser er mer eller mindre
kompliserte **logiske uttrykk** som "koker ned" til verdiene
`True` (sant) eller `False` (usant).

Løkkeprogrammering er en
videreføring av dette, ved at et program gjentar en sekvens med
handlinger så lenge en *betingelse* er oppfylt.

Dataprogrammer behandler ofte mange forekomster av
noe. Mange studenter ved en avdeling, mange ansatte ved en
institusjon, mange poster i en database, osv. Slike forekomster behandles
ofte ganske likt (med vekt på "ganske"), slik at det går an å skrive
en kodesnutt (noen linjer) èn gang, og la programmet utføre den mange
ganger. En slik kodesnutt kaller vi en **løkke** (engelsk: "loop").

Databaser og datasett vil videre ofte bestå av samlinger av variabler. Slike **samlinger** kan være **lister, tupler og dict'er**. Disse vil også bli behandlet i dette kapitlet.

## Løkker

Programmeringsspråket *Python* inneholder flere **løkkestrukturer**, hvorav de mest brukte er 
`for` og `while`-løkkene. I denne boken fokuserer vi på
`for`-løkken, men starter med noen eksempler på 
`while`-løkker, da de har mye til felles med `if`.


Alle oppgaver som involverer løkker kan, i
prinsippet, gjennomføres ved hjelp av begge de ovennevnte
strukturene. Forskjellige situasjoner er likevel best tjent med bruk av ulike løkketyper.

### while-løkken

En `while`-løkke gjentar en handling uttrykt ved
noen linjer med kode, så lenge et _logisk uttrykk_ (for eksempel
`i < 5`) er sant. Ordet "while" er
semantisk litt misvisende. På norsk kunne man heller si:
_så lenge_ `i` er mindre enn 5, gjenta disse
instruksjonene.

Eksempelet nedenfor viser en meget enkel `while`-løkke.

```{code-cell}
i=0 # initialisering
while i<5:
    print("i er nå lik",i)
    i = i + 1   # Oppdatering 
print("Nå er løkken slutt")
```

Linje  1 i eksemplet ovenfor tilordner
variabelen `i` en oppstartverdi. 
Dette kalles **initialisering**, og i vårt tilfelle intialiseres `i` til 0. 
Linjene 3 - 4 utføres gjentatte ganger, så lenge
"fortsettelsesbetingelsen" (det logiske uttrykket på linje 2) stemmer med virkeligheten. 
Linje 3 er vår "nyttehandling". Den printer verdien som **output**. 

Linjen 5 oppdaterer variabelen `i`
for hver omgang. Uten denne linjen blir betingelsen aldri usann
(`False`), og løkken går evig. Etter fem omganger, når `i` har fått verdien `5`, er
ikke betingelsen sann lenger, og løkken avslutter. Da fortsetter cellekjøringen, 
og linjen 5 kjøres.

✍️ **Oppgave:** Kopier koden fra cellen over inn i en celle i din egen notebook. Erstatt 5 med et annet tall.

⚠️ **Merk!** Ved å skrive `#` (etterfulgt av mellomrom) på en linje kan vi skrive inn kommentarer direkte i koden, uten at det påvirker hva selve programmet gjør. Det er ganske greit, særlig hvis man vil holde styr på hva hver del gjør. 



 
### Bruk av logiske uttrykk i if-tester og while-løkker

Legg merke til at `if`-tester og `while`-løkker bruker nøyaktig samme
type logiske uttrykk, `if`-tester, for å finne ut om en gruppe med
instruksjoner i det hele tatt skal utføres, og `while`-løkker for å 
finne ut om noen linjer skal utføres én gang til (og én gang til... 
og én gang til...).

Linjene i et program utføres èn
etter èn. Koden i `while`-eksemplet kan forklares ved å "nøste opp" løkken;
det som skjer bak kulissene er faktisk det samme som det her:

```{code-cell}
i = 0
if i < 5:
    print("i er nå lik ",i);
    i=i+1 # i blir 1
if i < 5:
    print("i er nå lik ",i)
    i=i+1 # i blir 2
if i < 5:
    print("i er nå lik ",i);
    i=i+1 # i blir 3
if i < 5:
    print("i er nå lik ",i)
    i=i+1 # i blir 4
if i < 5:
    print("i er nå lik ",i)
    i=i+1 # i blir 5
print("Nå er løkken slutt, fordi i er lik ", i) # 5   
```

## for-løkker

`for` er den andre løkkestrukturen vi presenterer i dette kapitlet. I motsetning til `while`, som er en "enkel" løkkestruktur som eksplisitt sjekker verdier i enkle variabler, er `for`-løkken avhengig av -- og fungerer naturlig med -- en **flerverdivariabel**.

Eksempelet nedenfor utfører samme oppgave som vårt første `while`-eksempel. 

```{code-cell} ipython3
for i in range(0,5):
    # En eller flere Python instruksjoner
    print("i er nå lik",i)
print("Nå er løkken slutt")
``` 

```{sidebar} tilsvarende while-løkke
<img src="./while_sidebar.png" alt="while-versjon" >
```
Det opprinnelige `while`-eksemplet er gjengitt i margen for sammenligning.

Det første du ser, er at `for`-koden er mye kortere. Vi hopper over både initialiseringen (førstelinjen `i=0` og oppdateringen (linje 4 `i = i + 1`).    

Legg merke til strukturen `range(0,5)` på linje 1. `range()` er en *Python*-struktur som konstruerer en **flerverdivariabel**. Denne består nå av verdiene 0, 1, 2, 3 og 4[^range],
og `i` antar alle disse etter tur. `for` gjør at den kjører samme type løkke som `while`, hvor den går gjennom hver innførsel og printer verdien (se innrykk) til den når slutten.

[^range]: `range(m, n)` gir en sekvens på n tall. Hvis *m* er 0 og *n* er 5, betyr det at den starter med 0 og lager en sekvens på totalt 5 tall. Sekvensen blir verdiene 0,1,2,3 og 4.

### Bruk av for-løkker



Både `for`- og `while`-løkker  kan, i prinsippet, brukes i alle
situasjoner som krever løkker. Vi bruker ofte `for`-løkker når vi vet
hvor mange ganger koden skal kjøre, for eksempel lengden på en liste. Altså hvor mange verdier en flerverdivariabel har. `while`-løkker brukes derimot når
antallet ganger koden skal kjøres avhenger av ting som skjer underveis i koden. 

I denne boka bruker vi for det meste `for`-løkken, da den faller naturlig (både teknisk og semantisk) i arbeidet med flerverdivariabler.

## Flerverdivariabler

En flerverdivariabel er en variabel med navn, som kan holde flere verdier samtidig, ved at verdiene er referert til med **indekser**[^indeks]. I en **liste** eller **tuppel**, er indeksen et tall (0,1,2 osv.), mens i **dictionaries** (som vi ser på senere) kan indeksene være tekststrenger, eller andre typer variabler. 

[^indeks]: Sier hvor noe er plassert i en **samling**, som for eksempel en liste. 

### Lister

En liste er en struktur som tillater flere variabler å dele ett
variabelnavn. Enhver slik variabel kaller vi en **liste-innførsel**
("list item" eller "list entry" på engelsk).

Lister tillater oss å behandle grupper av studenter, tall, matvarer
osv. på en *systematisk* måte. 
I en liste er hver liste-innførsel assosiert med et ordenstall. Liste-innførslene
kan skrives slik: `studenter[0]`, `studenter[1]`,
`studenter[2]` ... osv.

Hver liste-innførsel kan få tilordnet en verdi, uavhengig av de
andre innførslene. I eksemplet nedenfor oppretter vi en liten liste og fyller den
med noen navn, og deretter printer vi disse. 

I eksemplet under gir vi navn til en liste (`studenter`), samtidig som vi legger tre innførsler i den. 

```{code-cell}
studenter = [ "Arne", "Gerd", "Mona" ]
print("Studentene heter", studenter[0], studenter[1], "og", studenter[2])
```

Rekkefølgen innførslene er angitt i bestemmer deres posisjon i listen. Dvs. at verdiene ("Arne", osv.) er assosiert med indeksene (0, 1, 2) i og med rekkefølgen de er satt inn i skarpklammen.
Hvis vi nå ønsker å bytte ut "Gerd" med "Khan", kan vi overskrive `studenter[1]`, uten å røre de andre innførslene:

```{code-cell}
studenter[1] = "Khan"
print("Studentene heter", studenter[0], studenter[1], "og", studenter[2])
```

### kommandoen len()

Hvis vi forholder oss til listen `studenter` fra eksempelene ovenfor,
og antar at vi har sett hele listen,
ser vi at den har en lengde på 3, dvs. at den inneholder 3
uavhengige variabler som kan tilordnes 3 uavhengige verdier. Men
stort sett vet  ikke programmereren hvor lang en liste er. I tillegg
er lister dynamiske, og lengden på en liste kan forandre seg i
løpet av programmets levetid. Det er av
og til viktig å vite hvor lang en viss liste er *i et gitt øyeblikk*.


*Python*-kommandoen `len(listenavn)` sørger for at innførslene i
listen `listenavn` telles opp. `len()` er et eksempel på en
**funksjon**. Funksjoner forklares nærmere i
neste kapittel, men akkurat nå holder det å forholde seg til hva som skjer når vi bruker denne kommandoen.

I eksemplet nedenfor finner vi ut størrelse (også kalt **lengden**) på listen `studenter`.

```{code-cell}
studenter = [ "Arne", "Gerd", "Mona" ]
antall_studenter = len(studenter)
print(antall_studenter, "studenter er registert i listen 'studenter'")
```
✍️ **Oppgave:** Kopier koden inn i en celle i din egen notebook. Legg til navn på en student til etter "Mona", og kjør cellen. 

### Kommandoen list.append()

I forrige avsnitt hintet vi til at listene er "dynamiske", i den forstand at lengden på 
listen typisk endrer seg underveis i programmets levetid. Det er flere måter å endre 
en listes lengde på. Her skal vi se på `list.append()`. Denne er relevant i de tilfellene vi ikke så enkelt har innførselene i en liste tilgjengelig og kan legge til et nytt navn direkte inn i lista, slik vi gjorde i oppgaven over. 

I eksempel under, linje 3, med `studenter.append("Leif")` legger vi en ny 
student "bakerst" i listen `studenter`. Punktumet mellom `studenter` og `append` 
forteller at `append` forholder seg til listen studenter. På norsk sier vi *studenter sin 
append "Leif"*. Noe av det som skiller `append()` fra `len()` er at `append()` 
ikke er en uavhengig kommando, men forholder seg til listen som skal suppleres. 
Funksjonen `append()` er en **oppførsel** av variabeltypen `list`[^metoder]. På norsk leses 
linje 3 slik: *studenter sin append "Leif"*, hvor ordet *sin* uttrykker at 
`append()` er noe som anvendes på en bestemt liste, i dette tilfellet `studenter`. 

Når vi konstruerte `studenter` som en *Python*-liste, fikk vi med på kjøpet muligheten 
til å anvende `append()` (og andre nyttige funksjoner) på den. Andre funksjoner vi kan anvende på lister vil vi komme tilbake til i senere kapitler.

[^metoder]: Slike funksjoner, som er en del av **oppførselen** til en variabeltype,  kalles av og til "metoder".

```{code-cell}
studenter = [ "Arne", "Gerd", "Mona" ]
print("lengden før append:", len(studenter))
studenter.append("Leif")
print("lengden etter append:", len(studenter))
studenter
```

Merk at ved å bare skrive `studenter`, kommer det en egen **output** som oppgir innholdet i denne variabelen. 

✍️ **Oppgave:** Kopier koden i cellen over inn i en celle i din egen notebook. Erstatt `studenter` med `print(studenter)` istedet. Kan du tenke deg til hvorfor det blir forskjellig?

### Lister og løkker

Den virkelige fordelen med både flerverdivariabler og løkker kommer til syne når disse brukes 
sammen. Se spesielt på linje 5 i eksempel under. `for student in studenter` gjør at Python, etter tur, besøker 
hver innførel i `studenter`, tilordner den til løpevariabelen `student`, og bruker `print(student)` 
til å skrive innførselverdien. 

```{code-cell}
studenter = [ "Arne" , "Gerd" , "Mona" ]
antall_studenter = len(studenter)
print(antall_studenter, "studenter er registert i listen 'studenter'")
for student in studenter:
    print(student)
print("Disse var de", antall_studenter, "studentene våre.")
```

Hvis vi nå legger en ekstra student til listen, kommer programmet til å telle og 
liste riktig, uten at vi innfører andre forandringer enn å legge Vetle til listen. 

```{code-cell}
studenter = [ "Arne" , "Gerd" , "Mona", "Vetle" ]
antall_studenter = len(studenter)
print(antall_studenter, "studenter er registert i listen 'studenter'")
for student in studenter:
    print(student)
print("Disse var de", antall_studenter, "studentene våre.")
```

For å avslutte dette lille underavsnittet besøker vi igjen eksemplet med 
`for`-løkke fra tidligere i kapittelet. 
Den eneste endringen er at vi lagrer `range(1,5)` i en listevariabel, istedenfor å opprette 
den unavngitte listen[^range] "on the fly" i `for`-strukturen. 
variabelen `i` får her samme rolle som `student` i eksemplene ovenfor.

[^range]: I uttrykket `for i in range(1,5):` lager `range(1,5)` en tallsekvens i 
minnet, uten å legge den i en variabel med navn, derfor omtales den som en *unavngitt sekvens*, vi 
bruker ikke orde "liste" om range'en fordi den teknisk sett ikke er en liste, selv om den funksjonelt 
gjør samme nytten her.

<!-- Images/Lokker/rangeliste.png -->

```{code-cell}
rangeliste = range(1,5)
for i in rangeliste:
    print("i er nå lik", i)
print("Nå er løkken slutt")
```

## Tupler

Tenk på en excel-tabell vist som tabell under. Enhver av dataradene i den 
har tre celler, hver med sin fastlagte rolle. Rollen bestemmes av posisjonen i raden.

| Vare      | Pris | Antall |
| ----------- | ----------- | ----------- |
| Fotballer     | 95.00 | 5 |
| Shorts   | 89.00 | 12 |
| Trøyer   | 74.00 | 15 |





I eksemplet er den første cellen i hver rad av datatypen tekststreng (varenavnet), den andre et flyttall (stykkprisen) 
og det tredje et heltall (antallet). En slik **dataorganisering** (typisk også for en rad i en databasetabell) 
kaller vi en **tuppel**. I *Python* lager vi en tuppel med hjelp av parenteser.

<!-- Images/Lokker/enkel_tuppel.png -->

```{code-cell}
sportsvare=("Fotballer", 95.0, 5)
print(sportsvare)
```

Dersom vi ønsker å avlese en av verdiene, må vi angi dens posisjon i tuppelen (Se eksempel under). 
Som det meste annet i *Python* (og ellers i programmering), er den første telleposisjonen 0. 0,1,2 ... 

<!-- Images/Lokker/tuppel_pos.png -->

```{code-cell}
print(sportsvare[0])
```

På denne måten ligner tuppelen en liste, men med et par forskjeller. Viktigst er at listeinnførsler er som 
oftest av samme datatype (ikke strengt nødvendig, men vanlig), innførslenes posisjon er i utgangspunktet 
tilfeldig men kan sorteres på forskjellige måter, og lengden på listen er variabel. Tupler har fast lengde, 
innførslene har ofte forskjellig datatype og deres posisjon er fast og rollebestemt. Det er for eksempel 
ikke mulig med *tuppel.append*, da det ikke er naturlig å endre en tuppel midt i et program. Mer enn det, 
en tuppel kan faktisk ikke forandres etter at den er laget!

Se under hva som skjer når vi prøver: 

<!-- Images/Lokker/immutable.png -->

```{code-cell}
sportsvare[0] = "Håndballer"
```
### Forskjellene mellom liste og tuppel

Som det ble sagt i siste avsnitt, er den viktigste forskjellen mellom disse to datatypene, deres rolle. Herunder listes noen strukturelle forskejjer på dem:

|   | Liste  | Tuppel |
|:--|:------------------|:-------|
| innførsel datatype | lik (vanligvis)       | forskjellig (vanligvis) |
| innførsel posisjon | tilfeldig eller stigende/synkende sortert     |  fast, rollebasert |
| lengde | variabel | fast  |
| avgrensning | [] | ()  |

Mens en tuppel beskriver ofte et objekt ved å liste *attributter*, (eksempelvis en vare for hvilken det angis navn, pris og antall) i en fast rekkefølge, er en liste gjerne en "samling" av ettellerannet, for eksempel en samling av varer. Dette ser vi et eksempel på i neste avsnitt.   

## Lister av tupler

Flerverdivariabler kan lagre (samlinger av) forskjellige typer verdier. De kan til og med lagre samlinger 
av flerverdivariabler. Lister av lister, dict'er av lister, osv. Alt går, alt etter hvor det passer. 
I neste eksempel representerer vi exceltabellen fra tidligere som en 
liste med tupler.  Denne bygger vi opp ved å bruke `append()`. Det ytre par parenteser hører til 
`append()` og det indre paret til tuppelstrukturen.  

**Legg merke til at vi starter med en tom liste.** dette gjør vi ofte, for eksempel når vi bygger lister fra eksterne datakilder. Her må nye innførsler legges med `append()`. 
Vi legger til en ny tuppel til lista for hvert produkt.

<!-- Images/Lokker/liste_med_tupler.png -->

```{code-cell}
sportsvarer = [] #Liste med tupler
sportsvarer.append(("Fotballer", 95.0, 5))
sportsvarer.append(("Shorts", 89.0, 12))
sportsvarer.append(("Trøyer", 74.0, 15))
print(sportsvarer)
```

✍️ **Oppgave:** Kopier inn koden i en celle i din egen notebook. Legg til en ny tuppel med varenavn, pris og antall. 

Eksemplet under bruker en løkke for å skrive ut verdiene i listen 
`sportsvarer`. Legg merke til at variabelen `vare` antar hver av tuplene etter tur, og kan da 
brukes til å hente ut innførselverdiene fra tuppelen. Det vil si at vi her også gir hver tuppel variabelnavnet `vare` i linje 1 når vi bruker `for`-løkken.

<!-- Images/Lokker/skrive_liste_tupler.png -->

```{code-cell}
for vare in sportsvarer:
    print(vare[0], vare[1], vare[2])
```

✍️ **Oppgave:** Kopier inn koden i en celle i din egen notebook. Endre `vare` til et annet navn. Gjør i tillegg en endring i print slik at du kun printer en vare. 


De neste to eksempler beregner den samlede prisen på sportsvarene. Til dette oppretter 
vi en **"samlevariabel"** ved navn `samletpris`, som skal "akummulere" innkjøpsprisen. For hver vare 
legges det til innkjøpsprisen basert på antall varer ganget med stykkprisen til denne varen. To versjoner 
av dette eksemplet vises, hvor forskjellen ligger i hvordan oppdateringen av summen uttrykkes. 
På linje 3 i den nederste cellen bruker vi operatoren `+=` (**addisjonstilordning**, engelsk: 
"Addition assignment") som betyr egentlig det samme som linje 3 i den øverste cellen. Vi adderer en ny verdi til verdien som allerede er i variabelen fra før.

<!-- Images/Lokker/addition_assignment.png -->

```{code-cell}
samletpris = 0.0
for vare in sportsvarer:
    samletpris = samletpris + (vare[1] * vare[2])
samletpris
```

```{code-cell}
samletpris = 0.0
for vare in sportsvarer:
    samletpris += (vare[1] * vare[2])
samletpris
```


## Dict'er
**Dict'er** er em betegnelse for noe som egentlig har et lengre navn, nemlig **dictionaries**.
Mens lister er tallindeksert og har en naturlig rekkefølge, kan dict'er indekseres på hva som helst, men typisk er tekststrenger. Dette gjør dict'en til en meget kraftig struktur som kan lagre mer enn èn type data om hver innførsel. Betrakt eksemplet under. Som du ser i første linje, markeres en dict med `{}`-klammer.

<!-- Images/Lokker/dict.png -->

```{code-cell}
fodselsdager = {}
fodselsdager["Arne"] = "12. juni"
fodselsdager["Gerd"] = "10. oktober"
fodselsdager["Mona"] = "10. oktober"
fodselsdager["Vetle"] = "12. juni"
for k,v in fodselsdager.items():
    print(k, "har fødselsdag", v)
```

Hver av innførslene holder både studentens navn som **nøkkel** og vedkommendes fødselsdag som 
*verdi*. Derfor er det en konvensjon å bruke `k` (eng. *key*) og `v` (eng. *value*), som vi gjør på 
linje 6 (teknisk sett kunne vi brukt andre navn, og gjør det av og til når det passer). 


`items()` er en funksjon som er hendig å bruke med dict'er. 
Her gir `fodselsdager.items()` oss både nøkkelen og verdien i alle dictens innførsler, 
slik at `for` kan "besøke" dem etter tur, og `k` og `v` antar hhv. nøkkelen og 
verdien for hver besøkt innførsel for å gjøre noe med de.

Hvis du er kjent med databaser, kan det hjelpe å tenke på indeksene som primærnøkler 
i en entitetstabell. Dette innebærer, for både lister og dict'er, at indeksen (også kalt 
nøkkelen) er unik.  

✍️ **Oppgave:** Kopier inn koden i en celle i din egen notebook. Legg inn ditt eget navn og din egen fødselsdag i denne dicten. Kjør cellen.  

## Flerverdivariabler oppsummert
   

Flerverdivariabler er altså variabler som har ett navn med kan få flere verdier. 
De tre typene er lister, tupler og dicter.

| Tegn  | Navn | Beskrivelse |
|:--|:------------------|:-------|
| `[]` | Liste        | en rekkefølge av innførsler , som oftest av samme type, og som typisk endrer størrelse (og gjerne rekkefølge) i løpet av et programs levetid, `[innførsel0, innførsel1, innførsel2, osv.]` |
| `()` | Tuppel     |  en rekkefølge av innførsler hvor innførslene typisk har faste roller og gjerne annerledes datatyper, for eksempel `(tekst0, heltall1, flyttall2, tekst3, osv.)`.  |
| `{}` | Dict | En dict er en mengde innførsler hvor nøklene er unike. `{nøkkel_a:verdi_a, nøkkel_1:verdi_1, nøkkel_x:verdi_x, osv.}`  |


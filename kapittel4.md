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
# 4. Betinget programmering 
I dette kapitlet skal vi se litt nærmere på hvordan man ved programmering kan få **output** som er valg foretatt av dataprogrammet på bakgrunn av at du ber om noe ("gir input"). Å forstå hvordan dette skjer gjør det lettere å forholde seg til større datasett der man er ute etter bestemte utvalg som ikke så lett kan sees og hentes ut manuelt. I dette kapitlet lærer du altså enda et sett med grunnstener. Nå går vi fra datatyper til **operatorer**.

## Om et dataprograms oppførsel

Vi har sett at dataprogrammer kan betraktes som "levende" vesener
(anførselstegnene er viktige!). Et program kjører linje etter linje,
og hver linje kan forandre på programmets
status (verdiene i programmets variabler).
Dersom linje 5 er en kopi av linje
3, er det ikke sikkert at begge linjer har
nøyaktig samme utfall. Sannsynligvis ikke. 
I eksempel under oppstår det
forskjellige verdier i variabelen `i` (hhv. 2 og 3), etter at hver av de
to identiske linjene, 2 og 4, er kjørt.

<!-- Images/Betinget/linjer_kopi.png -->

```{code-cell}
i = 1
i += 1
print("linje 3:", i)
i += 1
print("linje 5:", i)
```

Det er heller ikke slik at våre programmer
behøver å oppføre seg likt hver gang de kjøres. Det
avhenger av omstendigheter som programmet er ment å reagere på, og
det er dette som gjør programmer så nyttige. 

    
### Tenker datamaskiner?

Nei. Datamaskiner tenker ikke. Allikevel er programmene som kjøres
på dem ofte meget avanserte. Hemmeligheten er at våre programmer
foretar *beslutninger* basert på valgalternativer. Disse
valgalternativene er ofte uttrykt ved **variabelverdier**. Alle
programmeringsspråk, *Python* inkludert, er utstyrt med
**beslutningsstrukturer**. Med disse strukturene kan
programmet "velge" èn av flere kjøreveier.

Å kjøre linje for linje er den enkleste formen for beslutningsstruktur. Men i hver linje kan vi legge til veivalg, til og med valg som gjør at avlesingen går tilbake til linje 1 igjen. Det er dette vi skal gi en introduksjon til her. 

<!-- Doublebox -->

⚠️ **Merk!** Dataprogrammer "tenker" ikke.
    De tar beslutninger (foretar valg), basert på klart formulerte betingelser.


## Enkle logiske uttrykk og beslutningsstrukturer

### Logiske uttrykk

Under ser vi et kort program. 
Uttrykket `i == 0` på linje 2 kaller vi et **logisk uttrykk**. Dette er noe litt annet enn å tilordne en variabel en verdi. 
På norsk lyder den  `i` *er lik* `0`. Dette uttrykket kan ha èn av to verdier, 
`True` eller `False`. 

Legg merke til `=` på linje 1  og `==` på linje 2. 
`=` (`i` *settes lik* `0`) er en **tilordning**, og forandrer variabelens 
verdi (dersom den ikke var `0` fra før). `==` er et sammenligningsuttrykk, 
og sjekker at verdiene på høyre og venstre side er like, *uten å forandre verdien*. 
*Python* har flere slike sammenligningsuttrykk, og vi ser på disse litt senere.

<!-- Images/Betinget/logisk1.png -->

```{code-cell}
i = 0
print(i == 0)
```

```{code-cell}
print(i == 1)
```

### if-testen

If-testen er den mest grunnleggende beslutningsstrukturen i både *Python* og
de fleste andre programmeringsspråk. La oss starte med et enkelt eksempel. 
Under ser vi et kort program. Linje 2, `if i == 0:` er en test, et spørsmål 
til systemet. Vi spør "om `i` er lik `0`", og videre ber vi om at den printer noe hvis det er riktig. Dersom det 
logiske uttrykket er `True`, kjøres line 3, hvis `False` kjøres den ikke. 

<!-- Images/Betinget/enkelif1.png -->

```{code-cell}
i = 0
if i == 0:
    print("i er 0!")
print("Takk og farvel.")
```

I neste eksempel ser vi en kjøring hvor `i` ble satt til 
`1`, og linje 3 derfor "hoppes over". Legg merke til at uttrykket `i == 0`, 
som bare er et testuttrykk ikke forandrer verdien i `i`, i motsetning til 
`i = 0` som er en tilordning som kan forandre verdien.

<!-- Images/Betinget/enkelif2.png -->

```{code-cell}
i = 1
if i == 0:
    print("i er 0!")
print("Takk og farvel.")
```

✍️ **Oppgave:** Kopier inn koden i cellen over i en celle i din egen notebook. Gjør en endring hvor du setter `if i == 1:` på linje 2. Kjør cellen. 


### Bruk av kolon (:) og innrykk

I motsetning til de fleste andre programmeringsspråk, er programstrukturen i *Python* 
uttrykt i programkodens[^programkode]  oppsett (engelsk: "layout"), og nivået av en gruppe kodelinjer 
bestemmes av mengden innrykk. Under ser du at kodelinjene 3, 4 og 5, som er rykket inn i forhold til if-testen, 
kjøres eller hoppes over samlet.

[^programkode]: "Programkode" eller "kode" brukes ofte når man snakker om programmets tekst.

<!-- Images/Betinget/innrykktrue.png -->

```{code-cell}
i = 0
if i == 0:
    print("i")
    print("er")
    print("0!")
print("Takk og farvel.")
```
<!-- Images/Betinget/innrykkfalse.png -->

```{code-cell}
i = 1
if i == 0:
    print("i")
    print("er")
    print("0!")
print("Takk og farvel.")
```

⚠️ **Merk!** Kolon og innrykk *må* være på plass. Kombinasjonen gjør at akkurat de print-operasjonene *kun* er koblet til `if`-spørsmålet i linjen over.  Legg merke til følgende feilsituasjonen:

```{code-cell}
i = 1
if i == 0:
print("i")
    print("er")
    print("0!")
print("Takk og farvel.")
```
Prøv gjerne å fjerne eller kolon for å se hva som da skjer.
     
### Logiske uttrykk og testoperatorer

Som tidligere nevnt er et logisk uttrykk et spørsmål,
som besvares med `True` eller
`False`. Et logisk uttrykk kan anta forskjellige former, og
tester ofte på variabelverdier. Variabelverdier blir til på
forskjellige måter. De kan hentes fra databaser eller filer, eller de kan
settes av programmet under kjøring. I eksemplene foran har vi sett
logiske uttrykk som tester om en variabel har en bestemt verdi (`i == 1`).

Disse uttrykkene tester på tallverdier, men både tall- og tekstvariabler kan
brukes i logiske uttrykk. Tegn som `==` og `>` kalles ofte for **testoperatorer**.

Nedenfor tester vi om verdien i en variabel er større enn 5 eller ikke:

```{code-cell}
i = 6
if i > 5:
    print("i er større enn 5!")
print("Takk og farvel.")
```

Kort og godt er vi ute etter å vite om noe stemmer eller ikke, gjerne fordi det er vanskelig å vite ved å bare se på datasettet med det blotte øyet. Denne typen programmering kan vi også bruke hvis vi ønsker å ta ut noen spesifikke rader av et datasett for å lage et nytt datasett. Dette er noe vi vil komme tilbake til i *del III* av denne boka. 

<!-- doublebox -->
⚠️ **Merk!** En beslutningsstruktur bruker en betingelse, uttrykt ved et logisk uttrykk,
for å velge en bestemt kjørevei. Logiske uttrykk bruker variabler og
testoperatorer.

*Python* støtter flere testoperatorer som kan brukes i logiske uttrykk.
Nedenfor vises varianter av logiske uttrykk som bruker forskjellige
testoperatorer.

| Testoperator      | Beskrivelse | Eksempler |
| ----------- | ----------- | ----------- |
| `==`     | om en variabelverdi er lik en bestemt verdi |  A. `tall == 0` er sant (har verdien `True`) dersom `tall` har verdien 0 i testøyeblikket <br><br> B. `student == "ja"` er sant hvis variabelen `student` har tekstverdien `ja` |
| `==`    | om en variabelverdi er lik en annen variabelverdi |   `tall1 == tall2` er sant hvis begge variabler har samme verdi |
| `<`     | om en variabelverdi er mindre enn en annen verdi |  A. `tall1 < 5`  er sant hvis verdien i `tall1` er eksempelvis `-1, 0, 4, 4.9.`, Usant hvis `tall1` har verdien `5, 6, 1000` e.l. <br><br> B. `tall2 < tall3` er sant når verdien i `tall2` er mindre enn verdien i `tall3` |
| `>`      | om en variabelverdi er større enn en annen |  A. `tall1 > 7` er sant når `tall1` er eksempelvis `10, 40, 1000000`. Usant når `tall1` har verdien `7, 6, 0` e.l. <br><br> B. `tall2 > tall1` er sant når verdien i `tall2` er større enn verdien i `tall1` |
| `<=`     | om en variabelverdi er mindre enn eller lik en annen |   `var3 <= 1` er sant hvis `var3` er eksempelvis `-1, 0, 1` |
| `>=`      | om en variabelverdi er større enn eller lik en annen |  `var1 >= 0` er sant når `var1` er eksempelvis `1, 5, 9` sant også når `var1` har verdien `0`. Usant for negative verdier i `var1` |




## Beslutningsstrukturer for flere valgalternativer

`If`-testen er en enkel struktur som bestemmer om et program skal
eller ikke skal utføre en sekvens med handlinger. I mer kompliserte
situasjoner foreligger det flere valgalternativer. Vi snakker da om
en **forgrening**: programmet har flere mulige *grener*, og
kan utføre èn av disse.


### If - else strukturen

Ofte trenger programmet å vite eksplisitt hva det skal gjøre også
dersom det logiske uttrykket er usant (har verdien `False`). 
Dette ordnes med en
else-gren som settes rett etter if-grenen. I eksempelet under ser 
man at programmet velger om det skal kjøre linje 3 eller 5 basert på verdien i 
variabelen `i`.  

<!-- Images/Betinget/ifelseif.png -->

```{code-cell}
i = 0
if i == 0:
    print("i er 0!")
else:
    print("i er ikke 0!")
print("Takk og farvel.")
```

<!-- Images/Betinget/ifelseelse.png -->

```{code-cell}
i = 1
if i == 0:
    print("i er 0!")
else:
    print("i er ikke 0!")
print("Takk og farvel.")
```

### Teste på motsatt (negert) betingelse: !=

Noen applikasjoner krever at vi utfører en handling bare hvis en
betingelse *ikke* er oppfylt. Dette kan i og for seg gjøres med en
else-gren, men det er kanskje mer elegant å
uttrykke betingelsen omvendt, med en **negeringsoperator**. `!=` leses 
på norsk *er ulik*. 
Se eksempel under.

<!-- Images/Betinget/negert.png -->

```{code-cell}
navn = "Michael"
print("Det er sant at du ikke heter Jens?")
if navn != "Jens":
    print("Ja, det er sant.")
```

### if-elif-else-strukturen

`elif` kan brukes når vi skal
ha mer enn to grener i en betingelse. Med
`if-elif-else` -strukturen kan vi tilrettelegge for håndtering av
flere mulige utfall. 

I eksempel under er det lagt opp til tre
alternativer. En `if`-gren og en `elif`-gren som oppgir to distinkte
alternativer, og en `else`-gren som angir  **standardhandlingen** (engelsk: "default"), altså 
hvis ingen av de angitte betingelsene er oppfylt. Vi kan bruke så mange
`elif`-grener vi bare ønsker.

<!-- Images/Betinget/tregjensidige.png -->

```{code-cell}
kategori = "ungdom"
if kategori == "ungdom":
    pris = 200
elif kategori == "senior":
    pris = 250
else:
    pris = 500
print("pris =", pris)
```

## Nøsting av tester

Med **Nøsting** (engelsk: "nesting") av tester menes at vi utfører en test
bare hvis det logiske uttrykket til en annen test har et visst utfall.

Vi
*nøster inn* èn betinget struktur som en gren av en annen struktur. 
I eksemplet under, utgjør linjene 3-6 if-grenen (linje 2). 
If-testen på linje 3 blir ikke kjørt hvis verdien i `barnealder` er 2 eller mindre.
Merk deg også bruken av kolon etterfulgt 4-blank-innrykk[^tab] hver gang et nytt 
nivå innføres (linje 2-3, 3-4, 5-6, 7-8).

[^tab]: I *Jupyter Notebooks* får vi 4-blanke når vi trykker TAB-tasten

<!--Images/Betinget/nosting.png -->

```{code-cell}
barnealder = 5
if barnealder > 2:
    if barnealder < 6:
        print("Barnet går i barnehagen.")
    else:
        print("Barnet går på skolen.")
else:
    print("Barnet er for ungt.")
```

✍️ **Oppgave:** Prøv å skrive om programkoden i if-elif-else-eksemplet
som en nøstet struktur.

<!-- David: Hva med en oppgave om barnealder = 30? -->

## Sammensatte logiske uttrykk

Av og til trenger vi å uttrykke mer kompliserte betingelser enn en
enkel test kan uttrykke. Stort sett er det mulig å løse disse med
de nøstede strukturene omtalt ovenfor, men dette kan resultere i
lang og rotete programkode. Sammensatte logiske uttrykk,
som bruker logiske kombinasjoner, er ofte mer elegante og resulterer
i kortere, mer fokusert kode. Vi ser på to kombinasjoner:

* `and` (logisk OG): to eller flere betingelser som skal stemme samtidig
* `or` (logisk ELLER): minst én av to eller flere betingelser skal stemme.

Disse kombinasjonene kan være byggesteiner i sammensatte logiske
uttrykk. Her ser vi på noen eksempler hvor én forekomst av en
logisk operasjon utgjør testen.

### Logisk OG (and)

Se eksempelet under. Dersom hele det kombinerte logiske
uttrykket i `if`-grenen skal være sant, kreves det at begge
deluttrykk er sanne *samtidig*. Hele det logiske uttrykk på linje 3 er sant 
hvis alderen er større enn 67 *samtidig som* kjønnet er kvinne.  

<!-- Images/Betinget/logiskog.png -->

```{code-cell}
alder = 65
kjonn = "kvinne"
if alder > 67 and kjonn == "kvinne":
    print("Som kvinnelig pensjonist får du en blomst.")
```

En annen, typisk bruk av OG-kombinasjonen
(`and`), er når man skal teste om verdien i en tallvariabel (for
eksempel alder) befinner seg innenfor et gitt intervall. Dèt gjør den
hvis den er **større enn eller lik** minstegrensen samtidig som
det er **mindre enn eller lik** størstegrensen. Se eksempel under.

<!-- Images/Betinget/intervall.png  -->

```{code-cell}
barnealder = 5
if barnealder >= 2 and barnealder <= 5:
    print("Barnet går i barnehagen.")
```

### Logisk ELLER (or)

Et logisk uttrykk med to deluttrykk koblet med `or` er sant
dersom minst ett av deluttrykkene er sant (eller dersom begge er
sanne samtidig). Se eksempel under. Her 
får man rabatt enten man er
en kvinne eller en senior (eller begge deler).

<!-- Images/Betinget/logiskeller.png -->

```{code-cell}
alder = 65
kjonn = "kvinne"
if alder > 67 or kjonn == "kvinne":
    print("Enten kvinner eller pensjonist (eller både og)")
```

## Videre lesning for spesielt interesserte

I dette kapitlet har vi sett på de mest grunnleggende
beslutningsstrukturene i Python. 
Det finnes noen andre `if`-baserte beslutningsstrukturer, og 
de som er særlig interesserte kan lese mer om dise her:

[https://realpython.com/python-conditional-statements](https://realpython.com/python-conditional-statements)
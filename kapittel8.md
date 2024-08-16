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

# 8. Dataanalyse med Pandas 

<div text-align="center">
    <table>
        <tbody>
            <tr>
                <td style="border-width: medium; border-color: black; padding: 5mm">
                    Kapitlet bruker følgende datafiler:<br/>
                    <a href="norge-rundt.csv">
                        norge-rundt.csv
                    </a><br/>
                    <a href="oda-master-thesis.csv">
                        oda-master-thesis.csv
                    </a><br/>
                </td>
            </tr>
        </tbody>    
    </table>
</div>

<br>
<br>

En av de vanligste utvidelsene vi bruker når vi jobber med dataanalyse av digitalisert og digitalt datamateriale er *Pandas*. På mange måter kan det å bruke *Pandas* sammenlignes med å det å jobbe med datasett i *Excel* fordi man jobber med datasett i tabellform. *Pandas* gir deg et arsenal av funksjoner og moduler som man kan bruke og i dette kapitlet vil vi gå gjennom noen av de vanligste. Data vi bruker i dette kapitlet er produsert av NRK, og er metadata om innslag fra TV-programmet 
_Norge rundt_. Filen er publisert på *Felles 
datakatalog*[^pandas_data_norge_norge_rundt]. 

[^pandas_data_norge_norge_rundt]: [https://data.norge.no/datasets/08fb719b-d563-44ac-97e1-f630ba92c507](https://data.norge.no/datasets/08fb719b-d563-44ac-97e1-f630ba92c507)


Siden dette kapitlet vil importere og eksportere filer er det viktig å nevne noe som er selvsagt men som aldri kan bli sagt ofte nok: Det er viktig å gi filene beskrivende navn. Da er det enklere å forstå innholdet i filen uten å måtte åpne den.

## Introduksjon til Pandas og DataFrames

*Pandas* er et funksjonsbibliotek ("library") for å behandle vilkårlig kompliserte tabelldata. *Pandas* tilbyr verktøy for å:

* lese inn data fra filer i utallige formater
* analysere og visualisere dataene med hjelp av *Python*
* trekke ut relevante deler fra disse dataene
* eksportere data til utallige filformater.

For å kunne bruke *Pandas* i *Python* og *Jupyter Notebooks* må vi først importere den.
`as pd` gir oss en **snarvei**, dvs. at funksjoner, metoder og attributer kan hentes fra *Pandas* ved å skrive 
`pd` istedenfor `pandas`.

```{code-cell}
import pandas as pd
```

Hoveddatastrukturen i Pandas er en **dataframe**. Dette er en tabellstruktur med rader, kolonner og celler. Data kan leses inn i en *Pandas*-dataframe fra mange ulike datakilder. En typisk fremgangsmåte er å hente inn data fra en CSV-fil ved å skrive `read_csv()`- funksjonen inn i en nyopprettet dataframe (`df = `).


```{code-cell}
df = pd.read_csv('data/norge-rundt.csv', sep = ';')
```

`pd.read_csv()` innebærer at vi bruker *Pandas*-funksjonen `read_csv()` for å lese inn en csv-fil. 
Tilsvarende funksjoner finnes også for å lese andre filfornater.  Funksjonen har mange parametre. 
Den første parameteren er **adressen**, eller _stien_ til CSV-filen. En annen mye brukt parameter er 
`sep`, som spesifiserer hvilket skilletegn som brukes i CSV-filen. Standardverdien, som brukes hvis 
`sep` utelates, er komma. I eksemplet her brukes `;` som skilletegn.

⚠️ **Merk!**  Å lese inn en fil betyr at inneholdet i filen trekkes ut. Selve filen forblir det samme uansett hva du gjør i *Jupyter Notebooks*. For å eksportere det du har gjort til en ny fil må du ta i bruk en annen funksjon som vi kommer tilbake til senere i dette kapitlet når det blir aktuelt. 


Du kan laste ned CSV-filen øverst i dette kapitlet. Men hvor du lagrer den er avhengig av hvilken adresse du oppgir i adresse-paramteret `.read_csv()` 
Hvis filen finnes i samme mappe som 
notebook'en, trenger vi kun skrive filnavnet. Det vil si at det er lettest for deg å ha både notebook og filer du henter inn data fra i samme mappe. 


✍️ **Oppgave:** Last ned CSV-filen og lagre den i samme mappe som du har dine notebook'er i. Importer Pandas slik det er vist i cellene over i din egen notebook, og les inn CSV-filen.




### Grunnleggende informasjon om en dataframe

Data som er hentet inn fra CSV-filen med metoden `read_csv()` er lagt inn i en variabel med navn 
`df`. Bruk av variabelnavnet `df` er en **konvensjon** når man henviser til en Pandas Dataframe. 
Teknisk sett kan man bruke hvilket lovlig variabelnavn som helst (`a`, `b`, `jens`, osv.).  
Har man flere variabler av typen "Dataframe" i programmet, må man så klart ha forskjellige navn. 
`df1`, `df2` osv. kan brukes, men også andre navn som går på rollen kan brukes. 
Hvis vi har en dataframe med tittelinformasjon og en annen om fofatterinformasjon, 
vil kanskje hhv. `df_titler` og `df_forfattere` funke.

### Datatype

Som vi har vist til i kapittel [Variabler](kapittel5.md), har alle Python-variabler en **datatype**. 
Datatyper kan være enkle: en tekststreng (`str`), et heltall (`int`) eller et flyttall (`float`). Datatyper kan også være 
flerverdivariabler slik som `list` og `tuple`, eller mer kompliserte, slik som 
et xml-tre,  `lxml.etree` (`etree` tilgjengelig fra pakken `lxml`). 

Bruk av pythons 
`type()`-funksjon på variabelen `df` avlører at den er av typen `pandas.core.frame.DataFrame`.  

```{code-cell}
type(df)
```



### Omfang/kompleksitet

**Omfanget** og kompleksiteten til dataene i dataframen uttrykkes ved hvor mange rader og kolonner 
som finnes i dataframe'n. `.shape`-attributtet til en dataframe kan brukes for å finne 
dette. Dataframe om Norge Rundt-innslag består av 10219 rader og 32 kolonner.

```{code-cell}
df.shape
```

Vi kan også få et overblikk over datasettet som en dataframe:

```{code-cell}
df
```

Her ser vi at det har både kolonner og rader. Og vi ser også at de cellene som ikke har en verdi er merket med "NaN". 

### Kolonner ("variabler")

Enten datasettet handler om salgstall eller instagram-poster blir radene som regel
"enheter" (individer, steder, varer), og kolonnene **variabler** (pris, lønn, temperatur, osv.). 


Hver kolonne i en dataframe har en datatype. Metoden `.info()` kan brukes for å vise informasjon om kolonnene i en dataframe. *Dtype* viser datatypen av kolonnen.

* *object* tilsvarer *str* i Python, dvs. tekststrenger
* *int64* tilsvarer *int*, dvs. heltall
* *float64* tilsvarer *float*, dvs. desimaltall.

Metoden viser også hvor mange av radene i kolonnen som har en verdi. 
F.eks. alle 10 219 rader i *aar*-kolonnen har en verdi (ikke er tomme), 
mens kun 4 882 rader i *hva\_er\_spesielt*-kolonnen har en verdi.

```{code-cell}
df.info()
```


Vi bruker videre *columns*-attributtet for å liste kolonnenavnene (om de ikke har navn, blir det automatisk genererte ordenstall). [Se også bruken av disse som overskrifter](my-label).

(pandas_column_index)=
```{code-cell}
df.columns
```

Her ser vi at det som har plass (indeks) i kolonnene blir oppgitt som en **liste**, som til sist inneholder `dtype='object'` som forteller at datatypen er et objekt, altså en tekststreng.



### Radene ("enhetene")

Når vi printer en hel dataframe, vil 
Python stort sett automatisk vise oss noen av de første og noen av de siste radene. De første radene 
i en dataframe kan også vises ved bruk av `.head()`-metoden.  Tilsvarende vises de siste radene med 
`.tail()`-metoden. Metodene `.head()` og `.tail()` har *heltall* som parametertype. F.eks. 
`.head(10)` viser de første ti radene. Standardverdien her (hvis man ikke oppgir antall) er fem. 

```{code-cell}
df.head(10)
```

✍️ **Oppgave:** Finn omfang, kolonner og rader i datasettet i din egen notebook, ved å kopiere inn koden i cellene over. Legg inn et tall i parantesene i `.head()`


## Utdrag av en dataframe

### Utdrag av kolonnene

Noen ganger er vi kun interessert i enten å se, eller å jobbe videre med, bare noen av kolonnene 
i en dataframe.[^pandas_projeksjon] Da kan vi opprette en ny dataframe som utgjør dette utdraget. 
Eksemplet under viser kun verdiene i *kommune*-kolonnen (de fem første 
og de fem siste radene vises):

[^pandas_projeksjon]: Dette kalles på dataspråket en "projeksjon".

```{code-cell}
df['kommune']
```

Vi kan avgrense til noen av kolonnene ved å *liste opp* kolonnenavnene. 
Husk at `[]` brukes for å avgrense lister i Python.

```{code-cell}
df[['aar', 'kommune', 'tittel']]
```

Resultatet her er en dataframe. Hvis vi vet at vi kun er interessert i å jobbe 
videre med noen av kolonnene, kan vi opprette en ny dataframe, som vi legger i en ny 
variabel. Den nye dataframe'n inneholder alle radene fra den opprinnelig dataframe, 
men kun tre kolonner.

```{code-cell}
df_liten = df[['aar', 'kommune', 'tittel']]

df_liten.columns
```

✍️ **Oppgave:**  Kopier inn innholdet i cellene over i din egen notebook. Men velg "tema" istedet for "tittel" som den siste kolonnen. 

### Utdrag av radene (filtrering)

En annen type utdrag fra en dataframe er å vise et utvalg av radene basert på *verdier* 
i èn (eller flere) av kolonnene. Dette kalles filtrering. Under er vi kun interessert i 
innslag fra Bergen, og avgrenser derfor til rader der kolonnen *kommune* har *Bergen* 
som verdi. Under resultatet vises antall rader som oppfyller avgrensningen. Dvs. det 
finnes 777 innslag som er tatt opp i Bergen.

⚠️ **Merk!** For å filtrere må vi først si hvilken dataframe vi vil filtrere og deretter i [] definere hvilken kolonne i den dataframen som skal filtreres. Derfor må vi skrive `df_liten` inne i [] også.

```{code-cell}
df_liten[df_liten['kommune'] == 'Bergen']
```

⚠️ **Merk!**  Radene er fortsatt nummerert med innførselsnummeret de har i det originale datasettet. 


Et filter kan legges i en egen variabel. Dette kan øke lesbarheten av koden. Flere 
filtre kan også kombineres med boolske operatorer. `&` brukes for *OG* mens `|` brukes 
for *ELLER*.[^pandas_boolsk] Kombinasjonen under avgrenser til rader der kommunen er 
Bergen samtidig som årstallet er før 1980.

[^pandas_boolsk]: en OG kombinasjon stemmer når begge betingelser som kombineres stemmer samtidig ELLER-kombinasjonen stemmer når minst en av betingelsene stemmer.

Her vil vi filtrere på *bare* Bergen kommune og *bare* innslag som er før 1980

```{code-cell}
filter1 = df_liten['kommune'] == 'Bergen'
filter2 = df_liten['aar'] < 1980

df_liten[filter1 & filter2]
```

Hvis vi bare skal se på innslag fra Bergen fra en gitt tidsperiode trenger vi ikke ha en kolonne som forteller at innslaget er fra Bergen. Da kan vi fjerne den kolonnen. Rader og kolonner kan avgrenses i samme kommando. Under bruker vi det samme filteret som i forrige skjermbilde, 
men begrenser visningen til to kolonner. Her legger vi listen av kolonner i en egen variabel, `cols`. 
Vi avslutter kommandoen med `.head()` for å kun vise de første fem radene.

```{code-cell}
filter1 = df_liten['kommune'] == 'Bergen'
filter2 = df_liten['aar'] < 1980

cols = ['tittel', 'aar']

df_liten[filter1 & filter2][cols].head()
```

✍️ **Oppgave:**  Gjør det samme i din egen notebook, men velg en annen kommune enn Bergen. Husk at hvis du kopierer må du også endre "tittel" til "tema". 

### Endre navn på en kolonne

Som vi har nevnt tidligere i boken (se f. eks. avsnitt om [variabelnavn](kapittel5.md)), utgjør god navngiving 
(av variabler, funksjoner, osv.) en del av dokumentasjonen til det vi gjør. Dette gjelder i enda 
høyere grad navngiving av kolonner i dataframes. Under definerer vi en dict som konverterer fra gammelt navn til nytt navn på 
to av kolonnene. Funksjonen `.rename()`, hvor parameteren `columns` tilordnes denne dicten, 
endrer navn på disse kolonnene før vi lister opp kolonnene med `.head()`. Legg merke til at 
kolonnennavnene i `df_liten` er fortsatt uendret, da denne navnendringen gjelder en midlertidig 
dataframe som lages "on the fly" for nettopp denne visningen.

```{code-cell}
newNames = {
    'aar' : 'år før 1980',
    'kommune' : 'sted tatt opp' 
}

df_liten[filter1 & filter2].rename(columns=newNames).head()
```

### Eksportere til en ny CSV-fil

Kanskje er det slik at du ønsker å vaske et mindre datasett i *OpenRefine*, eller rett og slett lagre dataframen som et nytt datasett? Du bruker da funksjonen `df.to_csv('ønsketnavnpåfilen.csv')`. Du kan også spesifisere adresse før navnet, om det er et spesielt sted du ønsker å lagre filen. `df` erstattes med navnet på dataframen du ønsker å eksportere til å bli en fil (Ja, det kan også bare være df (hoved-dataframe) om du kun har gjort endringer i den).
 
✍️ **Oppgave:**  Lag en CSV-fil av `df_liten`. Opprett en ny notebook og les inn filen som `df` (hoved-dataframe) for den nye notebooken.



## Flere datatyper i Pandas

### Series

En enkel kolonne utgjør en *Pandas*-`Series`. Når vi foretar redigeringer i en kolonne bruker vi gjerne enten *Pandas*-funksjoner som er definert 
med en **Serie** "i tankene" eller metoder som er definert for datatypen "Series". Vi starter med å se på årstall-kolonnen i df, `df['aar']`
```{code-cell}
df['aar'].head()
```
Vi ser at 'aar' er en tallkolonne.  

Hvis vi ønsker å konvertere alle datoangivelser i en kolonne til datatypen `datetime`, kan vi bruke pandas-funksjonen `to_datetime()`.
```{code-cell}
#Kjør en pandas-funksjon som konverterer en tallkolonne til en datokolonne
df['aar'] = pd.to_datetime(df['aar'])

df['aar'].head()
```
Kolonnen representerer nå årstallene som tidsinnførsler (1. januar er valgt per default som dato).

Vi kan også eksportere serien til en liste for bruk utenfor pandas. Dette gjør vi med metoden `to_list()` definert i Series-klassen

```{code-cell}
#Kjør en pandas _metode_ som gjør series om til en liste.
datoliste = df['aar'].to_list()
datoliste[:5]
```


### Index

En `Index` gjør det mulig å referere til enkeltrader, enkeltkolonner, enkeltceller og grupper av 
slike i en dataframe. Enhver dataframe, selv de enkleste av de, har en `Index`. Det finnes 
forskjellige innebygde indekstyper i Pandas for å svare til forskjellige behov. Som regel opprettes indeksen 
automatisk basert på strukturen i dataene. I [tidligere eksempel](pandas_column_index), 
ser vi en indeks som er basert på kolonnenavnene 
i CSV-filens første rad.  Under oppretter vi en dataframe basert på en liste av tupler fra et tidligere eksempel. 
Denne får automatisk en `RangeIndex`, som er en enkel ordenstallbasert indeks.

```{code-cell}
sportsvarer = [] # Liste med tupler

sportsvarer.append(('Fotballer', 95.0, 5))
sportsvarer.append(('Shorts', 89.0, 12))
sportsvarer.append(('Trøyer', 74.0, 15))

print(sportsvarer)
```

```{code-cell}
df_sport = pd.DataFrame(sportsvarer)

print(df_sport), 
print('kolonneindeksen:', df_sport.columns)
```

Mer kompliserte dataframes, hvor kolonnene har en gruppering vil få  en `MultiIndex`, 
som er en indeks med flere nivåer, hvor det øverste nivået ville være *kolonnegruppen* og 
det nederste nivået *enkeltkolonnen*. Eksempel på en `MultiIndex` vises i avsnittet om gruppering lenger ned i kapittelet.
<!-- avsnitt~\ref{pandas-gruppering}, figur~\vref{pandas-oda-groupby-dataframe}. -->


Fordi *aar* er et heltall kan vi bruke `>` og `<` i filter, se tidligere eksempel i 
skjermbildet 
<!-- \vref{pandas-filter-boolean}. -->

## Analyse av kolonner med tall

For å få oversikt over en kolonne som har en tallmessig datatype kan 
vi bruke `.describe()`-metoden.

```{code-cell}
df[['antall_menn', 'antall_kvinner']].describe()
```

Ønsker du å bare vise en form for tallmessig beskrivelse kan du velge funksjonene hver for seg.

Bruk `.median()`-metoden for å finne median for verdiene i en kolonne.

```{code-cell}
df['antall_kvinner'].median()
```

Under er en oversikt over tallmessige beskrivelser man kan benytte seg av: 

| Metode          | Beskrivelse |
|:----------------|:------------|
| .median()       |  |
| .mean()         | gjennomsnittsverdien |
| .count()        | antall rader for hvilke kolonnen har en verdi (ikke tom) |
| .sum()          | - |
| .min()          | - | 
| .max()          | - | 
| .quantile(0.25) | - |
| .quantile(0.75) | - |

Under bruker vi `.sum()` i en Python-celle for å regne ut prosentvis fordeling 
mellom menn og kvinner. Her tolker vi tomme celler som 0.

```{code-cell}
total_antall_kvinner = df['antall_kvinner'].sum()
total_antall_menn = df['antall_menn'].sum()

total_antall = total_antall_kvinner + total_antall_menn

kvinner_prosent = (total_antall_kvinner / total_antall) * 100
menn_prosent = (total_antall_menn / total_antall) * 100

print('Kvinner prosent:', kvinner_prosent)
print('Menn prosent:', menn_prosent)
```


## Analyse av kolonner med tekst

Hvis vi bruker `.describe()`-metoden på en kolonne som inneholder tekst, 
får vi en annen type statistikk:

```{code-cell}
df['kommune'].describe()
```

Her angir *count* antallet verdier i kolonnen. Dette kan være relevant for å se manglende 
data. Oversikten viser også antall unike verdier og verdien med høyest frekvens. 
I dette eksempel er det *Bergen* som har høyest frekvens med 777 forekomster.
Metoden `unique()` gir oss alle unike verdier i kolonnen.

```{code-cell}
df['kommune'].unique()
```


For å få en liste over unike verdier sortert etter frekvens, kan vi bruke 
`.value_counts()`-metoden. Visningen nedenfor sorteres på 
frekvens, og viser verdiene med de fem høyeste og fem laveste frekvenser.

```{code-cell}
df['kommune'].value_counts()
```

Vi kan legge til `.head()` eller `.tail()` for å se utdrag av verdiene. F.eks. for 
å se de 20 kommunene med høyest frekvens.

```{code-cell}
df['kommune'].value_counts().head(20)
```

✍️ **Oppgave:** Gjør det samme som over med kolonnen "tema". 

Alternativt kan vi vise relativ frekvens
<!-- \footnote{Relativ frekvens er andelen av forekomstene av en verdi 
i kolonnen, på den måten at hvis en viss verdi befolker halvparten av 
radene i kolonnen er dens relative frekvens 0.5 }. --> 
I eksemplet i brukes `normalize`-parameteren med boolsk verdi `True`.

```{code-cell}
df['kommune'].value_counts(normalize = True).head(10)
```

Frekvensene kan lagres som en dataframe ved hjelp av 
`.to_frame()`-metoden. 
```{code-cell}
df_temp = df['kommune'].value_counts(normalize = True).head(15).to_frame()
df_temp
```

✍️ **Oppgave:** Gjør det samme som over, bare for kolonnen "tema". 

Denne dataframen er indeksert med kommunenavnet som nøkkel. Slike dataframes brukes for eksempel i visualiseringer:
Nedenfor oppretter vi en ny konnene i den nylagde dataframen, 
*Procent* som tar prosenttall. Til slutt lages et enkelt stoplediagram.

```{code-cell}
df_temp['procent'] = df_temp['kommune'] * 100
df_temp['procent'].plot(kind = 'bar')
```
Du kan lese mer om visualisering i neste kapittel. Her gir vi kun en smakebit. 

## Gruppering

I denne gjenngangen bruker vi data fra ODA, OsloMets institusjonelle arkiv. Vi har hentet ut data som vi så laget lagret i en CSV-fil.  
 Du finner csv-filen på toppen av dette kapitlet. 
 
 
Dataframen består av 3149 rader (oppgaver) og 15 kolonner:

<!-- Vi må opprett df for dette å fungere. -->
```{code-cell}
df_oda = pd.read_csv('data/oda-master-thesis.csv', sep = ',')

df_oda.columns
```

Kolonnen *Lang* lagrer språket oppgaven er skrevet på:

```{code-cell}
df_oda['Lang'].value_counts()
```

Kolonnen *LangNorm* har en forenklet versjon av språk der koden 'nb' og 'nn' er 
mappet til 'nor'; 'en' er mappet til 'eng'; og andre verdier er mappet til 'oth'. 
Relativ frekvens for denne kolonnen viser at 77 prosent av oppgavene er skrevet på 
norsk, 22 prosent på engelsk, og mindre en 1 prosent på andre språk. 

```{code-cell}
df_oda['LangNorm'].value_counts(normalize = True)
```

Er fordelingen på språk likt mellom fakulteter? Vi kan bruke `.groupby()`-metoden for å vise 
språkfordelingen for hvert fakultet. Vi ser 
at LUI (Fakultet for lærerutdanning og internasjonale studier) har den størst andel av 
oppgaver på norsk, 88\%, mens TKD (Fakultet for teknologi, kunst og design) har 57,5\% på norsk.

```{code-cell}
df_oda.groupby('FacultyShort')['LangNorm'].value_counts(normalize = True)
```

I det følgende lagrer vi analyseresultater i en nyopprettet dataframe. 
Her brukes `.to_frame()` for å legge resultatet i en ny dataframe. Vanligvis 
er indeksen - identifikatoren for raden -  et heltall. Dette er nummeret 0, 1, 2, 
osv. som vises til venstre for radene. Når vi bruker `.groupby()` gjøres verdiene 
i kolonnen  som er parameter til *.groupby()* om til en indeks. Fordi vi grupperer 
*fakultet* kombinert med *språk*, lages det en `MultiIndex` av *FacultyShort* og 
*LangNorm*.
<!--
\footnote{For de som kan relasjonsdatabase, blir det analogt med en kombinert nøkkel.}. -->
Kolonnen med relativfrekvens kalles også *LangNorm*. Vi bruker `.rename()`-metoden for å 
endre navnet på denne kolonnen fra *LangNorm* til *RelativeFreq* 
<!-- (Se også figur \vref{pandas-rename}). --> 
Parameteren til `.rename()` er en dict med det gamle kolonnenavnet som nøkkel 
og det nye kolonnenavnet som verdi. Altså, etter å først gruppere, så oppretter vi en dict vi kaller for col_map. Denne dict'en inneholder nye navn på kolonner.  
Deretter 

`.inplace`-parameter bestemmer om vi 
skal oppdatere dataframe (True), eller kun se resultatet (False, standardverdi). 
Til slutt legger vi inn en ny kolonne hvor andelene er omregnet til prosenter. Da får vi en ny dataframe med nye navn på to kolonner, og en helt ny kolonne med ny data lagt til.
<!-- (figur~\vref{pandas-oda-groupby-dataframe}). -->

```{code-cell}
df2_oda = df_oda.groupby('FacultyShort')['LangNorm'].value_counts(normalize = True).to_frame()
col_map = {
    'LangNorm' : 'RelativeFreq'
}
df2_oda.rename(columns = col_map, inplace = True)
df2_oda['Procent'] = df2_oda['RelativeFreq'] * 100
print(df2_oda)
df2_oda.index
```

I visse situasjoner passer det ikke å jobbe med en MultiIndex. Da kan vi bruke `.reset_index()` 
( se cellen nedenfor <!--figur  ~\vref{pandas-oda-groupby-dataframe-result} -->), 
og gjør MultiIndex-kolonnene til vanlige datakolonner (Se tallene til venstre).

```{code-cell}
df2_oda = df2_oda.reset_index()
```

I neste kapittel skal vi se hvordan disse data kan 
visualiseres med utvidelsen *Plotly*. Her er en enkel visualisering støttet direkte av *Pandas*. 
I cellen nedenfor <!--på figur  ~\vref{pandas-oda-bar} --> ser vi hvordan andelen av de engelskspråkelige 
oppgaver er på tvers av fakultetene ved å avgrense til engelsk språk. 

```{code-cell}
filter1 = df2_oda['LangNorm'] == 'eng'

df2_oda[filter1].plot(kind = 'bar', x = 'FacultyShort', y = 'Procent')
```


Ved å bruke `.sort_values()`-metoden ( se cellen nedenfor <!--figur  ~\vref{pandas-oda-filter-sort} -->) 
kan vi sortere resultatet. Parametere til `.sort_values()` er kolonnen eller kolonnene 
det skal sorteres etter, og om det skal sorteres i stigende (`ascending = True`, 
standardverdi) eller fallende rekkefølge (`ascending = False`).

```{code-cell}
filter1 = df2_oda['LangNorm'] == 'eng'

df2_oda[filter1].sort_values('Procent', ascending = False)
```


## Behandling av kolonner som inneholder flere verdier

En masteroppgave kan ha flere emner. Dette løses i en CSV-fil ved å samle alle emnene i samme kolonne og bruke et internt 
skilletegn for å skille dem. I dette tilfelle <!--(figur  ~\vref{pandas-oda-subjects} )--> 
brukes `|` som skilletegn i kolonnen.

Det kan være at vi trenger å lage kolonner for hvert emne, slik at vi senere lettere kan lage utdrag.
Merk at dette også er mulig å gjøre gjennom *OpenRefine*, og ble gjennomgått i forrige kapittel. Men i de tilfeller der man oppdager at man trenger å gjøre det mens man jobber med et datasett i *Jupiter Notebooks*, kan det være raskere å gjennomføre slik behandling direkte ved bruke av *Pandas*. Men man står fritt til å også gå veien om *OpenRefine*, om man ønsker. 


```{code-cell}
cols = ['Title', 'Subjects']

df_oda[cols].head(5)
```


I cellen nedenfor <!--På figur  ~\vref{pandas-oda-split} --> brukes `.str.split()`-metoden for å dele 
opp kolonnen basert på et gitt skilletegn. Hver verdi legges så i sin egen kolonne. 
Det første emnet legges i kolonnen *0*, den neste i *1*, osv. Nederst ser vi at antallet 
kolonner er 24, dvs masteroppgaven med flest emner hadde 24 emner. Vi ser at masteroppgaven 
med indeks *1*, andre rad, har kun tre emner. Alle andre kolonner (for eksempel 
3-24 for oppgaven med index *1*) får tilordnet spesialverdien *None*.

```{code-cell}
df_oda_splitted = df_oda['Subjects'].str.split('|', expand = True)

df_oda_splitted.head()
```

Hvis vi ønsker å legge dette til i den opprinnelige dataframen kan vi gjøre følgende:
I neste celle <!-- figur  ~\vref{pandas-oda-merged} -->, ser vi et eksempel på hvordan 
*Pandas*-funksjonen `.concat()` kan brukes for å sammenføye den nye `df_splitted` 
dataframen med den opprinnelige `df` dataframe. Parameteren til `.concat()` er 
en liste av dataframes som skal settes sammen. Den andre parameteren, *axis*, 
bestemmer om de skal kombineres som rader (`axis = 0`, altså, nedover) eller som 
kolonner (`axis = 1`, altså bortover). Her vil vi legge inn flere kolonner, 
altså bortover. Den nye dataframen, `df_merged`, har både de opprinnelig kolonnene, 
f.eks. *Title* og kolonner fra de oppdelte emner, f.eks. *0*.

```{code-cell}
df_oda_merged = pd.concat([df_oda, df_oda_splitted], axis = 1)

cols = ['Title', 0]

df_oda_merged[cols].head()
```


Neste trinn er bruk av `.melt()`-metoden. `.melt()` gjør om alle emne-kolonnene til 
en enkelt kolonne, ved å gjenta øvrige data i radene nedover. Metoden tar to 
lister med kolonner som parametre. `id_vars` er kolonnene som skal gjentas for 
hver rad, mens `value_vars` er kolonnene som skal sammensmeltes til èn. I 
vårt eksempel er listen `value_vars` en sekvens med tall fra 0 til 23. *Python*-funksjonen *range()* kan brukes for å generere tallrekken. I vårt tilfelle 
ville `.melt()` gjøre  det enkelt å finne alle dokumenter som har 
(for eksempel) emneordet *Kosthold*  

```{code-cell}
id_vars = ['Title', 'FacultyShort', 'InstituteShort']
value_vars = list(range(24))

df_oda_melted = df_oda_merged.melt(id_vars = id_vars,
                           value_vars = value_vars)

df_oda_melted.sort_values(['Title', 'variable']).head()
```

```{code-cell}
value_vars = list(range(10))

value_vars
```


Resultatet av `.melt()`-metoden plasserer emneordene i en kolonne som får 
navnet `value`. Kolonnen `variable` er det opprinnelige kolonnenavnet, dvs. 0, 
1, osv. I eksemplet under ser mann en enkelt tittel som har tre emner, de øvrige 
21 radene (3, 4, ... 23) er lagt inn med *None*. Bruken av `.melt()` legger dermed 
mange rader uten ny informasjon (se de siste to radene   <!--på 
figur  ~\vref{pandas-oda-melt-filter} -->). 

<!-- Endre eksempel? -->
```{code-cell}
filter1 = df_oda_melted['Title'].str.contains('Når barnet ikke vil')

df_oda_melted[filter1]
```
Antallet rader blir antall rader 
i utgangspunktet (`df`,  3149) ganget med antallet kolonner med gjentatte emner, 
som var 24 (0 til 23). Resultatet er 75 576. Vi kan sjekke dette med å bruke 
`.shape`-attribut på de dataframes som er brukt. <!--(
figur  ~\vref{pandas-oda-melt-shape} )-->
```{code-cell}
print('Dataframe df:', df_oda.shape)
print('Dataframe splitted:', df_oda_splitted.shape)
print('Dataframe merged:', df_oda_merged.shape)
print('Dataframe melted:', df_oda_melted.shape)
```


`.isna()`-metoden kan brukes for å avgrense dataframe til rader der verdier 
mangler. Det motsatte er (og stort sett mer nyttig) er `.notna()` som avgrenser 
til rader som har et verdi. Under anvendes 
`.notna()` på "value"-kolonnen for å fjerne de overflødige radene med tomme emner, 
noe som reduserer antallet rader i dataframen fra 75.576 til 16.769. 

```{code-cell}
df_oda_melted = df_oda_melted[df_oda_melted['value'].notna()]
```

Her sjekkes kolonnen *value* for tomme verdier. Dette reduserer dataframe 
fra 75.576 til 16.769 rader.

For å rydde opp i dataframe fjerner vi *variable*-kolonnen og omdøper 
*value*-kolonnen til *Subject*. `.drop()`-metoden brukes til å fjerne kolonner. 
En liste over kolonnene legges til som parameter. Igjen brukes parameteren `inplace` 
for å utføre oppdateringen på dataframe som dermed forandres 
<!--(figur  ~\vref{pandas-oda-drop-rename} )-->. 

```{code-cell}
df_oda_melted.drop(columns = ['variable'], inplace = True)

col_map = {
  'value' : 'Subject'
}

df_oda_melted.rename(columns = col_map, inplace = True)

df_oda_melted.head()
```

Emnene er nå klare for analyse. I cellen nedenfor <!-- ~\vref{pandas-oda-abi-subjects} --> 
finner vi er de mest populære emnene for masteroppgaver skrevet ved institutt ABI. 

```{code-cell}
filter1 = df_oda_melted['InstituteShort'] == 'ABI'

df_oda_melted[filter1]['Subject'].value_counts().head(10)
```

I neste kapittel skal vi jobbe videre med CSV-filene ved å gjøre om mønstre til visualiseringer i form av grafer.


(my-label)=

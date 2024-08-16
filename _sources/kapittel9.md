---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---
 


# 9. Visualisering med Plotly




<div text-align="center">
    <table>
        <tbody>
            <tr>
                <td style="border-width: medium; border-color: black; padding: 5mm">
                    Kapitlet bruker følgende datafiler:<br/>
                    <a href="oda-master-thesis.csv">
                        oda-master-thesis.csv
                    </a><br/>
                    <a href="folkebibl-2020-kommuner.csv">
                        folkebibl-2020-kommuner.csv
                    </a><br/>
                </td>
            </tr>
        </tbody>    
    </table>
</div>

<br>
<br>

I dette kapitlet går vi igjennom hvordan visualisere aggregerte mønstre i form av grafer, og gir en inngang til enkle statistiske oversikter. 

Det finnes flere Python-moduler som kan brukes for å generere diagramer (stolpediagram, kakediagram, osv.). Noen av de meste kjente er _Matplotlib_[^matplotlib], _Seaborn_[^seaborn] og _Bokeh_[^bokeh]. Her skal vi bruke **Plotly**.
 Det er også mulig å lage og eksportere csv-filer som kan brukes i eksterne visualiseringsprogrammer. Dette er alt fra verktøy som gir tilgang til hundrevis av tilpassbare måter å visualisere data som hos [RawGraphs](https://www.rawgraphs.io/), til verkøy som er laget for spesifikke typer nettverksanalyser og -visualiseringer, som [GephiLite](https://gephi.org/gephi-lite/). 
 
En fordel med *Plotly* er at det lar deg lage interaktive diagramer direkte i *Jupyter Notebooks*.

[^matplotlib]: <a href="https://matplotlib.org/">https://matplotlib.org/</a>
[^seaborn]: <a href="https://seaborn.pydata.org/">https://seaborn.pydata.org/</a>
[^bokeh]: <a href="https://bokeh.org/">https://bokeh.org/</a>
## Innlasting av modulene og tilrettelegging


Data som brukes med *Plotly* kommer fra en *Pandas* dataframe. Vi må derfor importere både *Plotly* og *Pandas*. 

```{code-cell}
---
mystnb:
  number_source_lines: true
---
import pandas as pd
import plotly.express as px
import plotly.io as pio 
pio.renderers.default = 'notebook'
```

*Plotly Express* er en del av *Plotly*. Det er vanlig å referere til modulen som *px*, på samme måte som det er vanlig å referere til *Pandas* som *pd*. De to siste linjene, 3 og 4 trenges i visse situasjoner for at plottene skal vises.
Vi skal nå gå gjennom enkle diagrammer som vi kan generere med *Plotly*


## Stolpediagram


I forrige kapittel så vi på et eksempel av data om masteroppgaver ved OsloMet. Vi starter ved å opprette dataframen fra csv-filen i data-katalogen, og ber den vise oss hvilke kolonner datasettet består av:

<!-- Vi må opprett df for dette å fungere. -->
```{code-cell}
df_oda = pd.read_csv('data/oda-master-thesis.csv', sep = ',')
df_oda.columns
```
For helt grunnleggende statistikk er det viktig å kunne telle antall forekomster av verdier i diverse kolonner.
Nedenfor bruker vi funksjonen `.value_counts()` til å telle antall oppgaver hvert institutt har i datasettet.
```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_oda['InstituteShort'].value_counts()
```

Vårt første plott blir et stolpediagram med disse fordelingen av språk på tvers av alle oppavene. For å bruke dette i Plotly må vi først gjør om resultatet av tellingen (`.value_counts()`) til en dataframe. 

```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_temp = df_oda['LangNorm'].value_counts().to_frame()
df_temp
```

I cellen nedenfor gjør vi om den gamle rad-indeksen (kolonnen med språkene) til en vanlig kolonne og gir deretter kolonnene nye navn. Denne eksisterer nå som en ny dataframe. 


```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_temp.reset_index(inplace = True)

col_map = {
    'index' : 'Språk',
    'LangNorm' : 'Antall'
}

df_temp.rename(columns = col_map, inplace = True)
df_temp
```

Den resulterende dataframen, `df_temp`, kan nå brukes for å lage stolpediagrammet. I cellen nedenfor opprettes en objektvariabel `fig` som tilordnes resultatet av `.bar()`-funskjonen fra Plotly Express. `fig` representerer et objekt av typen _Figure_, som har mange funksjoner (også kalt metoder). Metoden `show()` som brukes til slutt er den som driver mekanismen som viser diagrammet i tegneområdet. Metoden `.bar()` som produserer og returnerer `fig`-objektet kan ta mange parametre. De viktigste listes nedenfor:


    * dataframen i eksempelet: `df_temp`
    * tittelen gitt til horisontalaksen (x) i eksempelet: 'Språk'
    * tittelen gitt til vertikalaksen (y) i eksempelet: 'Antall'


```{code-cell}
---
mystnb:
  number_source_lines: true
---
import plotly.io as pio 
pio.renderers.default = 'notebook'
fig = px.bar(
    df_temp,
    x = 'Språk',
    y = 'Antall'
)

fig.show()
```

Dette resulteter i stolpediagrammet på figuren over.

✍️ **Oppgave:** Gjør det samme som over i din egen notebook. Bytt gjerne ut "Språk" med en annen verdi som du tenker det kan være hensiktsmessig å telle. 


Hvis vi lar musepekeren hvile til høyre over diagramet, vises en knapperad som lar oss manipulere diagramet (figuren nedenfor). Her kan vi lagre diagrammet som et bildefil i PNG-formatet, zoome inn og ut, osv.

![Plotly_knapper](./Images/Plotly/plotly-graph-buttons.png)




## Endre tittel og farger


Vi kan legge til en tittel på diagrammet ved å bruke `title`-parameteren til `.bar()`-funskjonen (Det samme kan vi gjøre hos de andre av *Plotly* sine graf-funksjoner).

Det er stor fleksibitet i hvilke farger som kan brukes. Dette oppgis med `color_discrete_sequence`-parameteren. Det finne flere ferdige fargekombinasjoner, f.eks. `px.colors.qualitative.T10`. Noen er vist på figuren . 

--> Se [Plotly dokumentasjon](https://plotly.com/python/discrete-color/\#discrete-color-with-plotly-express) for en fullstendig liste.

![plotly-colour-palettes](./Images/Plotly/plotly-colour-palettes.png)

Alternativt kan vi bruke fargenavn fra CSS, _AliceBlue_, _AntiqueWhite_, osv. Eller heksadesimalkoder, f.eks. _\#00FFFF_. 

Se f.eks. [W3Schools](https://www.w3schools.com/cssref/css\_colors.asp) for en oversikt over koder. 

Flere, "grunnleggende" farger kan angis ved navn.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
colours = ['green']

colours
```

Når vi legger inn koder slik, selv om det bare er ett element som skal fargelegges (som ovenfor), må disse legges inn i en *Python*-liste. Hvis stolpene skal være grønne og vi har tre stolper kan vi skrive `['green', 'green', 'green']`. 

```{code-cell}
---
mystnb:
  number_source_lines: true
---
colours = ['green', 'green', 'green']
colours
```

Men vi kan også gjøre dette på en annen måte om vi skal ha samme farge for alle kolonner. Legg merke til cellen nedenfor. Her brukes `*`-operatoren til å mangfoldiggjøre listen `['green']` et gitt antall ganger (her 3).

```{code-cell}
---
mystnb:
  number_source_lines: true
---
['green'] * 3
```


Dette gir oss muligheten til enkelt å bruke `len()`-funksjon (dataframens lengde) for å fargelegge alle rader i dataframen likt.


```{code-cell}
---
mystnb:
  number_source_lines: true
---
colours = ['green']
colours * len(df_temp)
```
Og slik brukes det i selve plottet:

```{code-cell}
---
mystnb:
  number_source_lines: true
---
fig = px.bar(
    df_temp,
    x = 'Språk',
    y = 'Antall',
    color_discrete_sequence = ['green'] * len(df_temp),
    title = 'Fordeling av OsloMet masteroppgavene etter språk'
)

fig.show()
```




## Kakediagram


Fordeling av språk kan også vises som et kakediagram ved å bruke `.pie()`-funksjonen. 

Dataframen som brukes er den samme som ovenfor. Istedenfor _x_ og _y_ brukes parameteren `names` for kategoriene og `values` for fordelingen. 

Her ønsker vi i tillegg vise språkenes fulle navn (Dette kan også gjøres i stolpediagrammet, bare prøv!). Til dette anvender vi dict'en `value_map`.



```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_temp = df_oda['LangNorm'].value_counts().to_frame()
df_oda
df_temp.reset_index(inplace = True)

col_map = {
    'index' : 'Språk',
    'LangNorm' : 'Antall'
}

df_temp.rename(columns = col_map, inplace = True)

value_map = {
    'nor' : 'Norsk',
    'eng' : 'Engelsk',
    'oth' : 'Annet'
}

df_temp['Språk'] = df_temp['Språk'].map(value_map)

df_temp
```

Nedenfor ser vi også bruk av parametrene til `.update_layout()`-metoden, som kan brukes for å midtstille overskriften (`title_x`) og endre skriftstørrelse til overskriften (`title_font_size`).

```{code-cell}
---
mystnb:
  number_source_lines: true
---
fig = px.pie(
    df_temp, 
    names = 'Språk',
    values = 'Antall'
)

fig.update_layout(
    showlegend = False
)

fig.update_traces(
    textposition = 'outside',
    textinfo = 'percent+label'
)

fig.show()
```


Resultatet ser vi på figuren over. 


Hvis musepekeren hviler over et kakestykke vises ekstra opplysninger om kakestykke: Som vi ser på figuren nedenfor, kategorien **eng** og absoluttallet **2443**. Prøv også å flytte markøren over diagrammet over.


<div align="center">
	<img alt="plotly-pie-hover" src="_images/plotly-pie-hover.png" width="450">
</div>


Vi kan legge inn opplysninger i selve kaken ved å bruke `.update_traces()`-metoden (se linje 11 i koden over). Vi har fjernet noen av parametrene fra før for å spare plass. Hvis kategoriene legges i kaken kan vi vurdere å fjerne **forklaringen** til diagrammet (eng. "legend"). Dette gjøres ved å sette parameteren `showlegend` til `False`.

Merk at annet språk (_oth_) vises så vidt ikke på figuren nedenfor (en stripe grønt), grunnet størrelsen. 
Fargene som brukes her er standardfargene til *Plotly*.

<div align="center">
	<img alt="plotly-pie-no-legend-result" src="_images/plotly-pie-no-legend-result.png" width="450">
</div>

## Gruppert stolpediagram


For å sammenligne språkfordelingen på fakultetene, bruker vi `.groupby()`-metoden i Pandas. For å visualisere slike analyser kan vi bruke grupperte eller stablete stolpediagram.

I cellen nedenfor grupperer vi språkkodene etter fakultet. Settingen `normalize = True` medfører at tallene vises som relativtall. Resultatet gjøres om til en dataframe.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_temp = df_oda.groupby('FacultyShort')['LangNorm'].value_counts(normalize = True).to_frame()

df_temp
```

Som du ser, har vi her en *multiindex*, en indeks med flere nivåer (her: to). Legg også merke til at `value_counts()`-funksjonen produserer en tallkolonne (på multiindeksens øverste nivå), som heter det samme som kolonnen på annet nivå, noe som må "brytes opp". Dette gjør vi ved å endre førstenivåkolonnens navn til `Prosent`:

```{code-cell}
---
mystnb:
  number_source_lines: true
---
col_map = {
    'LangNorm' : 'Prosent'
}
df_temp.rename(columns = col_map, inplace = True)

df_temp
```
`rename()`-funksjonen forholder seg til det øverste nivået når det lager `Prosent`-kolonnen.
Nedenfor prosentuerer vi denne i praksis, simpelthen ved å gange verdiene med 100:

```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_temp['Prosent'] = df_temp['Prosent'] * 100

df_temp
```



Tilbake til multiindeksen:


```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_temp.index
```

I følgende celle brukes `reset_index` for å gjøre om multiindeksen til vanlige kolonner, og innføre et default-tallindeks. 


```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_temp.reset_index(inplace = True)
df_temp
```


Til slutt omdøper vi kolonnen _FacultyShort_ til _Fakultet_ og kolonnen _LangNorm_ til _Språk_. Dette gjør det enklere å holde styr på hva de ulike kolonnene består av.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
col_map = {
    'FacultyShort' : 'Fakultet',
    'LangNorm' : 'Språk'
}

df_temp.rename(columns = col_map, inplace = True)

df_temp
```


For å lage en gruppert stolpediagram må vi legge til noen parameter til `.bar()`-funskjonen vi brukte tidligere. Vi må legge til `barmode`-parameteren med `group` som verdi (linje 6). Selve gruppene legges inn som x-akse. Kategoriene som grupperer, her språk, legges inn i `color`-parameteren.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
fig = px.bar(
    df_temp,
    x = 'Fakultet',
    y = 'Prosent',
    color = 'Språk',
    barmode = 'group',
    color_discrete_sequence = px.colors.qualitative.T10,
    title = 'Språk av OsloMet masteroppgavene etter fakultet',
)

fig.show()
```

✍️ **Oppgave:** Gjør det samme som beskrevet over, men forsøk å gruppere etter institutt og ikke fakultet. 

## Stablet stolpediagram


Den eneste endring som er nødvendig for å endre fra gruppert til stablet stolpediagram er å endre verdien av `barmode`-parameteret til *relative* (linje 6).



Hvis du klikker på en kategori i forklaringen, fjernes katagorien fra visningen. Klikk en gang til for å hente den tilbake. For eksempel, hvis vi kun vil vise andelen som er på engelsk kan vi klikke på _nor_ og _oth_. 

```{code-cell}
---
mystnb:
  number_source_lines: true
---
fig = px.bar(
    df_temp,
    x = 'Fakultet',
    y = 'Prosent',
    color = 'Språk',
    barmode = 'relative',
    color_discrete_sequence = ['green', 'yellow', 'red'], #px.colors.qualitative.T10,
    title = 'Språk av OsloMet masteroppgavene etter fakultet',
)

fig.show()
```

## Linjediagram


Linjediagrammer er ofte brukt med tidsdata, slik at X-aksen representerer tiden. Hvordan har fordelingen av språk endret seg over tid? Som før lager vi en dataframe med dataene vi ønsker å bruke i visualiseringen. Mye av dette ligner det forrige eksemplet, men her grupperer vi på dato heller enn på fakultet ( se linje 1 i cellen nedenfor.). 



```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_temp = df_oda.groupby('Date')['LangNorm'].\
value_counts(normalize = True).to_frame()

col_map = {
    'LangNorm' : 'Prosent'
}
df_temp.rename(columns = col_map, inplace = True)
df_temp.reset_index(inplace = True)
df_temp['Prosent'] = df_temp['Prosent'] * 100

df_temp.head()
```

```{code-cell}
---
mystnb:
  number_source_lines: true
---

```
For at data skal vises riktig, må verdiene i _Date_-kolonnen (som vi ønsker å omdøpe til _År_) endres fra tekststrenger til datoer. Dette gjøre vha `.to_datatime()`-funksjonen fra *Pandas* (linje 7). Datoer kan skrives på mange ulike måter. Vi kan bruke `format`-parameteren til å angi **formatet**. Formatet _%Y_ betyr at kun årstallet vises. 

```{code-cell}
---
mystnb:
  number_source_lines: true
---

col_map = {
    'Date' : 'År',
    'LangNorm' : 'Språk'
}
df_temp.rename(columns = col_map, inplace = True)

df_temp['År'] = pd.to_datetime(df_temp['År'],  format = '%Y')

df_temp.head()
```



⚠️ **Merk!**  Legg merke til at *Pandas* har lagt _01-01_ til året for å gjøre det om til en dato.


For å vise et linjediagram bruker vi `line()`-funksjon i Plotly. Bruk av `color`-parameteren gir oss en linje for hver kategori (linje 4). `markers`-parameter bestemmer om punktene skal markeres på linjen (linje 5). 

```{code-cell}
---
mystnb:
  number_source_lines: true
---
fig = px.line(df_temp, 
    x= 'År', 
    y= 'Prosent',
    color = 'Språk', # Hva er det fargene skiller på
    markers = True
)

fig.show()
```

Resultatet ser du på plottet over. Legg merke til at vi mangler data for noen tidspunkter. Dette er et eksempel på at når vi jobber med å få frem mønstre i data vil vi også oppdage noe det kan være interessantå dykke dypere ned i.





Hva skjedde i 2005? Her bør vi se i grunnlagsdataene. Vi lager et filter for å kun se rader fra 2005 (linje 1). Vi avgrenser visningen til kolonnene for tittel, årstall, språk og institutt (linje 3-8).

```{code-cell}
---
mystnb:
  number_source_lines: true
---
filter1 = df_oda['Date'] == 2005

cols = [
    'Title',
    'Date',
    'Lang',
    'InstituteLong'
]

df_oda[filter1][cols]
```

Over ser vi at vi har kun 11 oppgaver fra 2005. 10 var skrevet på engelsk og alle var skrevet på instituttet for informasjonsteknologi. Hva dette skyldes kan ikke dataene vi har alene svare på.




## Spredningsdiagram


Her bruker vi data fra statistikk for norske folkebibliotek. Vi er interessert i å vise forholdet mellom folketall og besøkstall. Er det slik at bibliotek i kommuner med høyere folketall har mer besøk?

```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_bib = pd.read_csv('data/folkebibl-2020-kommuner.csv', sep = ',')
df_bib.columns, df_bib.shape
```
Over viser vi (et utdrag av) kolonnenavnene og størrelsen på datatabellen. CSV-filen med bibliotekstatistikk har over 200 kolonner med statistikk. 

Vi ser så nærmere på hva som er i dette datasettet ved å be den vise frem filen som en dataframe:

```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_bib
```

Her er vi interessert i kolonnen _Besøk totalt_ og _Totalt per 1.1.2020_. Imidlertid kan det virke som det som står i kolonnen "Besøk totalt" er tekst. En indikasjon på dette er at det er et mellomrom mellom de tre første tallene og de tre siste. 

Kolonnen _Totalt per 1.1.2020_ må vaskes før vi kan bruke den videre. 

Vi vasker direkte i notebooken (men dette kan også gjøres i *OpenRefine*). Vi fjerner først mellomrom fra tallene (eks. _307 528_ til _307528_). Nå som mellomrom er borte kan vi endre datatype i kolonnen til å være tall og ikke tekst. Til dette bruker vi i begge cellene, ovenfor linje 8 og nedenfor linje 5, *Pandas*-funksjonen `to_numeric`. Vi omdøper også _Totalt per 1.1.2020_ til _Folketall_:

```{code-cell}
---
mystnb:
  number_source_lines: true
---
# definer funksjonen vi skal anvende
def vaskMellomRom(verdi):
    return verdi.replace(' ', '')

#fjern mellomrom i tall. (typisk dataproblem)
df_bib['Totalt per 1.1.2020'] = df_bib['Totalt per 1.1.2020'].apply(vaskMellomRom)

# Go kolonne nytt navn
col_map = {
    'Totalt per 1.1.2020' : 'Folketall'
}
df_bib.rename(columns = col_map, inplace = True)

# Endre datatype
df_bib['Folketall'] = pd.to_numeric(df_bib['Folketall'])
```
Her brukte vi metoden `Series.apply()`, som anvender en funksjon (her `vaskMellomRom`) på hver av innførslene. Se på syntaksen på linje 6: `apply()` bruker funksjonens navn alene (uten parentes og argument). Bak kulissene brukes hver av innførslene, etter tur, som argument til funksjonen, og vasket verdi returneres. 


✍️ **Oppgave:** Gjør det samme som over i din egen notebook. Sjekk så `df_bib` for at endringene er blitt gjort og for å få en oversikt over hvordan dataframen nå ser ut. 


En annen måte å fjerne mellomrom på er gjort med den andre kolonnen i cellen under. Her skjer mer bak kulissene, men produserer samme resultat; mellomrom fjernes. 

```{code-cell}
---
mystnb:
  number_source_lines: true
---
df_bib['Besøk totalt'].fillna('', inplace = True)

df_bib['Besøk totalt'] = df_bib['Besøk totalt'].apply(lambda x: x.replace(' ', ''))

df_bib['Besøk totalt'] = pd.to_numeric(df_bib['Besøk totalt'])

```



Utgangspunktet for diagrammet kodes i cellen nedenfor. På linje 3 oppretter vi en ny kolonne som inneholder besøk per person. Vi viser data for de ti bibliotek med høyest besøkstall.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
cols = ['Kommune', 'Folketall', 'Besøk totalt', 'Besøk per person']

df_bib['Besøk per person'] = df_bib['Besøk totalt'] / df_bib['Folketall']

df_bib.sort_values('Besøk totalt', ascending = False)[cols].head(10)
```

Nedenfor bruker vi `scatter`-funksjonen fra *Plotly* for å lage spredningsdiagrammet. Igjen, parametre for tittel osv. er utelatt for å fremheve de viktigste parametrene. Vi bruker `hover_name`-parameteren for å legge ekstra data til boksen som vises når vi sette musepekeren over et punkt på diagrammet.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
#Vil vise sammenheng i småsteder

fig = px.scatter(
    df_bib,
    x = 'Folketall',
    y = 'Besøk totalt',
    hover_name = 'Kommune'
)

fig.show()
```


Ovenfor vises resultatet. De alle fleste bibliotek er klumpet nederst til venstre i diagrammet. 

Vi kan bruke et filter for å fjerne de fire største komunnene. På linje 2 i kodecellen nedenfor oppretter vi en liste med de kommunene som vi ikke vil ha. På linje 4 bruker vi `.isin()`-metoden for å lage et filter som kun tar med disse fire kommune. Når vi bruker filteret på dataframe (linje 7), setter vi _~_-tegnet foran for å si at vi vil ha alt som _ikke_ matcher filteret.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
#Vil vise sammenheng i småsteder
fjern = ['Oslo', 'Bergen', 'Trondheim', 'Stavanger']

filter1 = df_bib['Kommune'].isin(fjern)

fig = px.scatter(
    df_bib[~filter1],
    x = 'Folketall',
    y = 'Besøk totalt',
    hover_name = 'Kommune'
)

fig.show()
```

Resultatet ser du ovenfor.
<!--
-->

## Boksplot

Blir bibliotek i større kommuner oftere besøkt (per innbygger) enn bibliotek i mindre kommuner? Eller er det kanskje omvendt? Èn måte å finne ut av det, er å dele kommunene i _kvantiler_: Altså et antall like store grupper bibliotek, i stigende antall innbyggere, og se gruppenes fordelinger av relative besøkstall (herunder kalt _besøksrate_) opp i mot hverandre. Linje 1 nedenfor deler vi kommunene i 4 folketallskvantiler, slik at de minste kommunene havner i det nederste kvantilet (0), de litt større i neste kvantilet (1), osv. Linje 2 legger til en kolonne i data-framen (kalt _kvantil_) hvor hver kommune får "sitt" kvantil (0, 1, 2 eller 3) tilordnet.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
kvantiler = pd.qcut(df_bib['Folketall'], 4, labels=False)
df_bib = df_bib.assign(kvantil=kvantiler.values)
```

Nå er vi klare til å tegne _boksplottet_ for hvert kvantil. Boksplot er en god måte å vise relative tendenser på, samt å isolere "outliers". 

```{code-cell}
---
mystnb:
  number_source_lines: true
---
fig = px.box(
    df_bib,
    x = 'kvantil',
    y = 'Besøk per person',
    hover_name = 'Kommune',
    color='kvantil',
)
fig.update_traces(quartilemethod="linear") # or "inclusive", or "linear" by default

fig.show()
```

Hold musepekeren over boksene. Et nøye blikk på boksene kan tyde på en svak tendens til at større kommuner har flere besøk per innbygger. Men er dette en virkelig trend? i statistikkspråk: Er den **signifikant**? eller med andre ord: Hva er sannsynligheten for at utviklingen vi ser er tilfeldig? 
Boksene vi ser tyder på at fordelingen er meget skjev. Altså: besøk-ratene i alle kvantilene klumper seg ikke likt til høyre og til venstre for et gjennomsnitt. 

Hvis vi vil undersøke disse aggregerte mønstrene nærmere med utgangspunkt i **statistiske analyser** kan vi gjøre følgende beskrevet under:

Cellen nedenfor genererer en **tetthetfordelingsplot**, et slags glattet histogram, for hver av kvantilene. Her bruker vi *figure_factory*-modulen til *Plotly*.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
import plotly.figure_factory as ff


kv0 = list(df_bib[df_bib['kvantil']==0]['Besøk per person'].fillna(0))
kv1 = list(df_bib[df_bib['kvantil']==1]['Besøk per person'].fillna(0))
kv2 = list(df_bib[df_bib['kvantil']==2]['Besøk per person'].fillna(0))
kv3 = list(df_bib[df_bib['kvantil']==3]['Besøk per person'].fillna(0))

hist_data = [kv0, kv1, kv2, kv3]

group_labels = ['kv0', 'kv1', 'kv2', 'kv3']
colors = ['green', 'blue', 'red', 'black']

# Create distplot with curve_type set to 'normal'
fig = ff.create_distplot(hist_data, group_labels, show_hist=False, colors=colors)

# Add title
fig.update_layout(title_text='Besøksrate fire kvantiler')
fig.show()
```
Ved å se på figuren over er det lett å miste håpet om at folketallet er forklarende til forskjell i besøksraten.
Det er også lett å se at hovedtyngdene av kvantilene har en tilnærmet normal fordeling, hvor de lange halene representerer relativt få verdier. Da vi også har relativt mange datapunkter, drister vi oss til å kjøre en T-test mellom kv0 og kv3, som er kvantilene hvis gjennomsnitt ligger lengst unna hverandre i plottet.

En **T-test** tester om to datasett "kommer fra" den samme fordelingen (hypotesen h0) eller ikke (h1). Vi avviser stort sett h0 (default-hypotesen) hvis p-verdien er mindre enn 0,05.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
from scipy.stats import ttest_ind
kv0 = list(df_bib[df_bib['kvantil']==0]['Besøk per person'].fillna(0))
kv3 = list(df_bib[df_bib['kvantil']==3]['Besøk per person'].fillna(0))
ttest_ind(kv0, kv3, equal_var=False)
``` 

Med en så stor p-verdi er vi langt fra å kunne avvise h0, dvs foreslå at besøksraten har noe med folketallet å gjøre.

Du har nå fått en smakebit på hvordan undersøke sammenheng mellom variabler i aggregerte mønstre nærmere. Merk at vi ikke skal gå nærmere inn på slike statistiske analyser i dette kapitlet. 


## Ordsky

Det siste vi skal ta for oss er en metode som gir deg en alternativ oversikt til en lang liste over ordfrekvenser. Her er istedet størrelsen på ordet en indikasjon på forekomst, mens andre variabler som farge og plassering er arbitrære. 


For å generere en ordky bruker vi _wordcloud_-modulen. For å installere moduelen åpner vi *Anaconda Prompt* i *Anaconda*. Skriv inn kommandoen `pip install wordcloud`.

I cellen nedenfor importeres modulen og en del variabler derfra. Deretter lager vi teksten som skal vises som et ordsky. Vi bruker emne-kolonnen fra ODA-datasettet om masteroppgavene. Først erstatter vi manglende verdier med blank (linje 2). Deretter oppretter vi en tom variabel, `text` (linje 4) som skal senere "fylles inn" med emneordene etter hvert som de hentes ut fra dataframens rader. For å avgrense til masteroppgaver skrevet ved ABI, lager vi et filter (linje 6). Vi går gjennom alle radene (oppgavene) som oppfyller filteret, og henter innholdet i _Subjects_-kolonnen (linje 9). Hvis en oppgave har flere emner, er emnene adskilt med `|`-tegnet. Vi bruker `.replace()`-metoden til å erstatte tegnet med et mellomrom (linje 8). På linje 11 sammenføyes emne(ne) *bakerst* i teksten. vi tilføyer en ekstra mellomrom for at siste ord i nåværende ikke sammenføyes direkte med førsteord i neste.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
from wordcloud import WordCloud, ImageColorGenerator, STOPWORDS
df_oda['Subjects'].fillna('', inplace = True)

text = ''

filter1 = df_oda['InstituteShort'] == 'ABI'

for idx, row in df_oda[filter1].iterrows():
    subject = row['Subjects'].replace('|', ' ')
    
    text += subject+ " "
```

Koden som lager ordskyet er vist under. I linjer 3 til 8 sette vi parameter til ordskyet. Teksten som vi har generert over brukes for å opprette ordskyen i linje 10.


```{code-cell}
---
mystnb:
  number_source_lines: true
---
stopwords = set(STOPWORDS)

wc= WordCloud(background_color="black", 
              random_state=1,
              stopwords = stopwords,
              max_words = 200, 
              width = 1500, 
              height = 1500)

wc.generate(text)
```


Vi bruker *Plotly* for å vises bildet. Parametere i linjene 3-9 brukes for å fjerne aksene.

```{code-cell}
---
mystnb:
  number_source_lines: true
---
fig = px.imshow(wc)

fig.update_xaxes(
    showticklabels=False
)

fig.update_yaxes(
    showticklabels=False
)

fig.show()
```

Resultatet vises ovenfor.


Du har nå gått gjennom en innføring i det å visualisere aggregerte mønstre gjennom ulike grafer. Dette har vi gjort med utgangspunkt i datasett som allerede eksisterer som CSV-filer.


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
<!-- DPX API-key 5f4tsj4og9l6snv : 817nd001bsoqzbh-->
# 1. Introduksjon 
> “In academic research, but also in many sectors of business and other areas of society at large, data analysis unfolds via computer interfaces that display results that users often mistakenly regard as objective assessments. Such environments need knowledge workers who can grasp the processes of knowledge generation, from data collection through the various stages of analysis to visualization.” - Van Es & Schäfer i *The Datafied Society*[^kilde1]

Denne læreboka gir en introduksjon - og er en praktisk guide - til det å bruke programmeringsspråket *Python*, å jobbe i brukergrensesnittet *Jupyter Notebooks*, samt å bruke programmet *OpenRefine*. Alle nødvendige verktøy for behandling og analyse av data. 

Boka fungerer særlig godt som en introduksjon til databehandling og analyse slik det gjøres innen **digital humaniora og digital samfunnsvitenskap** (på engelsk: *DHSS*). Boka er særlig rettet mot studenter som jobber med data som kommer fra -- eller er relevante for -- institusjoner innenfor informasjons- og kulturforvaltning (arkiver, biblioteker og museer), samt de som ønsker å studere digital kultur gjennom data fra nettet. Innføringen i programmeringen du får her skal gjøre deg i stand til å håndtere data, dette inkluderer hente, behandle, vaske, analysere og visualisere data.  Videre er boka ment for deg som enten ikke har noen erfaring med programmering eller som har litt erfaring men trenger et litt mer stabilt fundament.  

## Hva er programmering?
Med programmering mener vi det å skrive programmer («å kode») som en datamaskin kan tolke og utføre. Slike programmer skrives i et **programmeringsspråk**. Et programmeringsspråk er et språk som mennesker kan bruke (skrive og lese) og som en datamaskin kan tolke og utføre. Et programmeringsspråk er ikke et «ekte» språk på linje med norsk eller engelsk, men vil allikevel ofte inneholde flere engelske ord. I tillegg er et programmeringsspråk kjennetegnet av en **rigid syntaks** (rekkefølgen ord og tegn forekommer i). Ved å bruke programmeringsspråket kan vi derfor spesifisere for en datamaskin hvordan en bestemt oppgave skal utføres. Dette kan være oppgaver av svært ulik art; fra å gjøre en enkel matematisk utregning (2+2=4), til å lage et stort og komplisert dataspill. De digitale delene av våre liv er satt sammen gjennom programmeringsspråk, og det å forstå hvordan dette i praksis skjer gir oss en større innsikt i og forståelse av vår samtid.  

Denne boka skal ikke dekke alle mulige oppgaver man kan utføre med programmeringsspråk. Den skal primært dekke oppgaver relatert til behandling og analyse av data og datasett som man vil støte på innenfor digital humaniora og samfunnsvitenskap. Allikevel ser vi også behovet for at de første kapitlene fokuserer på noe av det grunnleggende ved hvordan programmeringsspråk fungerer slik at det vil bli lettere å skjønne hva som skjer «under overflaten». Boka introduserer derfor viktige grunnleggende aspekter ved *Python*-språket i del 2, før man tar steget videre i del 3 for arbeid direkte med data og datasett. 

## Formålet med denne boka
Boka har som formål å vise hvordan *Python* kan brukes til å behandle og analysere **strukturerte data**[^definisjon]. Slike datasett kan bestå av metadata innsamlet fra sosiale medier, datasett med metadata om artefakter og samlinger, tekstkorpus, eller andre typer datasett som inneholder store mengder med informasjon, hvor man trenger hjelp fra en datamaskin til å foreta analysene. 

Her ønsker vi å gi en inngang til en verden som blir mer og mer digital, hvor informasjon først og fremst eksisterer som digitale data. På engelsk snakker man om en *«datafication»* av samfunnet vårt, og på grunn av dette har det blitt mer og mer viktig i sektorer som jobber med informasjons- og kulturforvaltning å kjenne til det å jobbe med data og datasett. *Python* (og særlig *Pandas*-pakken), *Jupyter Notebooks* og *OpenRefine* er alle verktøy som er i aktiv bruk i sektorene for informasjons- og kulturforvaltning, samt i humanistisk og samfunnsvitenskapelig forskning. Kunnskap og ferdigheter innen programmering er derfor en nødvendighet.

Boka plasserer seg derfor i et felt som allerede har flere innganger. Den største og mest etablerte er digital humaniora, tett etterfulgt av digital samfunnsvitenskap. Deler av dette kalles også *«cultural analytics»* på engelsk fagspråk. Vår inngang overlapper med disse, noe annet hadde vært rart. Men vi henter også inspirasjon fra den lange tradisjonen med [Software Carpentry](https://software-carpentry.org/) og [Library Carpentry](https://librarycarpentry.org) som har utviklet seg fra ulike klynger i fag- og forskingsbiblioteker de siste 25 årene. *Library Carpentry* handler for eksempel om teknikker for å samle inn, vaske, representere og behandle data. **Carpentry** referer derfor til teknikker som kommer forutfor selve analysene (selv om man nok i praksis ser at man vil gå frem og tilbake). Dette er nødvendig kompetanse å inneha for å kunne gjøre seg kjent med et datasett før man faktisk foretar analyser.

## Hvorfor Python og Jupyter Notebooks?
Det finnes hundrevis av ulike typer programmeringsspråk. I denne boka er det *Python* som er programmeringsspråket det gis opplæring i. Grunnen er at Python er både relativt enkelt å lære seg, samtidig som det er meget vanlig å bruke. *Python* har også meget gode verktøy for å håndtere innhenting av data fra andre databaser eller datakilder. I tillegg kan språket brukes på alle tenkelige datamaskintyper og operativsystemer. En viktig tilleggspakke er *Pandas*. Denne paken gjør det særlig enkelt å jobbe med den type datasett og data som boken tar ugangspunkt i. Tenk på *Pandas* som en utvidelse til *Python*.

*Jupyter Notebooks* brukes som et grensesnitt for kjøring av *Python*. Hovedfordelen med *Jupyter Notebooks* er nettopp det at brukergrensesnittet er lett forståelig, og gjør det oversiktlig å se hva man har gjort og når. Her kan man både programmere og kommentere det man gjør. Fil-formatet kan videre enkelt sendes og åpnes av andre.

*Jupyter Notebooks* vil for mange kjøres gjennom et program (teknisk sett et større rammeverk) som heter *Anaconda*.

## Oppbygning av boka
### Del 1: Kom i gang!

- **Kapittel to** gir en introduksjon til, og en gjennomgang av, hvordan man installerer og tilrettelegger *Anaconda*. Kapitlet gir videre en introduksjon til *Jupyter Notebooks* og har en gjennomgang av hvordan skrive og kjøre en veldig enkel programsnutt ved bruk av *Python*-språket i en slik notatbok. 

### Del 2: Grunnleggende Python

- **Tredje, fjerde, femte og sjette kapittel** introduserer de viktigste elementene i *Python* som et programmeringsspråk. Disse kapitlene gir deg fundamentet som du trenger for å kunne bruke og forstå programmeringsspråket.

### Del 3: Databehandling og -analyse

- **syvende kapittel** tar for seg *OpenRefine* som er et datavaskingsverktøy, og viser hvordan man kan ta ut datasett for vasking i *OpenRefine* og hente de inn igjen etterpå for å behandle og analyse de videre i *Jupyter Notebooks*.  

- **Kapittel åtte** tar for seg hvordan analysere data ved hjelp av *pandas*, og går gjennom hvordan gjøre enkle univariate (deskriptive) analyser og statistiske beskrivelser av strukturerte data.  

- **Kapittel ni** introduserer *Plotly* , et visualiseringsredskap for *Python*. Her skifter fokus fra det å analysere til det å visualisere data.

### Del 4: Innhenting av data fra ulike kilder 

- **tiende kapittel** ser på noen eksempler av relevante datasamlinger som finnes digitalt og hvordan disse kan hentes inn som datasett og jobbes med i *Jupyter Notebook*. Her inngår også det å høste inn data fra nettet.

> Del 4 vil bli publisert høsten 2024

## Supplerende kilder 
Under er en liste over andre engelske introduksjoner og digitale bøker vi anbefaler.

[Introduction to Cultural Analytics & Python](https://melaniewalsh.github.io/Intro-Cultural-Analytics/welcome.html)

[Humanities Data Analysis - Case Studies With Python](https://www.humanitiesdataanalysis.org/)


[^kilde1]: [Schäfer, M. T., & Van Es, K. (2017). The datafied society: Studying Culture Through Data.](https://library.oapen.org/handle/20.500.12657/31843)
[^definisjon]: Data ordent i Excel-kolonner er et enkelt eksempel på strukturerte data, der hver “celle” - tall eler tekst - har en rolle bestemt av kolonnen den står i.
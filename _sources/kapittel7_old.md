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

# 7. Innhenting av data 

<!--
Data finnes på nettet mange steder og i forskjellige former, alt fra strukturerte filer (i XML og JSON-formatene), halvstrukturerte filer (for eksempel CSV-formattert), og til data integrert i websider som må evt. \anf{skrapes} med hjelp av spesiell programvare. Noen data er lagret i databaser, slik at utdrag kan lastes ned basert på brukerstyrt avgrensning (også kalt filtrering). En slik avgrensning kan skje gjennom søk, tidangivelse eller andre parametre. Filtrert data lastes da typisk ned som XML/JSON, generert automatisk der og da.  Fokuset i dette kapitlet er sistnevnte, og vi henter \textit{bibliotekmetadata} for viderebehandling og analyse. Eksempelet som brukes er bibliografiske metadata fra bibliotekkataloger og vitenarkiver.
-->

## SRU-protokollen

<!--
SRU (Search/Retrieve via URL) protokollen brukes for å søke i bibliotekkataloger. Eksempelet som brukes her kan modifiseres for å hente data fra en hvilken som helst bibliotekkatalog som støtter SRU-protokollen.

Hva er en protokoll? Store norske leksikon definerer en protokoll slik\footnote{https://snl.no/protokoll\_-\_IT}:

\begin{quote}
"Protokoll er formater og fremgangsmåte som kreves for å få datamaskiner til å kommunisere. Protokollen gir regler for dataformat, sending og mottak av data, timing, feilsjekking og datakomprimering."
\end{quote}

SRU lar et dataprogram utfører et søk i en bibliotekkatalog. Dataprogrammet som utfører søket kalles gjerne for en \textit{klient}. Katalogen med data som svarer på søket kalles for \textit{tjerneren}. Søket utformes som en URL. Følgende URL utfører en søk i katalogen til Bærum bibliotek etter dokumenter med ordet \textit{Python} i tittelen:

\begin{verbatim}
https://brmbib.bib.no/cgi-bin/sru?version=1.2&operation=searchRetrieve&
maximumRecords=10&recordSchema=dc&query=dc.title=Python&
startRecord=1
\end{verbatim}

Som du ser, er dette en URL, og den kan (foruten i et python-program) også brukes direkte i en nettlesers adressefelt. Prøv gjerne.

URLen begynner med adressen til tjerneren, i dette eksemplet \textit{https://brmbib.bib.no/cgi-bin/sru}. Ulike bibliotek har hver sine adresser. Leverandøren til biblioteksystemet Bibliofil har en liste over SRU-tjenere\footnote{https://bibsyst.no/z3950tabell.html} som bruker Bibliofil. For å utføre det samme søket i Moss bibliotek kan vi bytte \textit{https://\underline{brmbib}.bib.no/cgi-bin/sru} med \textit{https://\underline{presbib}.bib.no/cgi-bin/sru}.
Etter \textit{sru} følger tegnet \textbf{?}, som brukes for å skille adressen til tjerneren fra \textit{parametrene} som sendes. Disse sendes parvis med navn og verdi. Parameternavnet kommer før tegnet \textit{=} og parameterverdien kommer etter. Parameterparene er separert fra hverandre med tegnet \textbf{\url{&}}. Her er en oversikt over parameterparene i vårt eksempel:

\begin{center}
\begin{tabular}{ |l| l | l | } 
 \hline
 \textbf{Parameternavn}  & \textbf{Parameterverdi} \\ 
 \hline
 version        & 1.2 \\ 
 operation      & searchRetrieve \\
 maximumRecords & 10 \\ 
 recordSchema   & dc \\ 
 query          & dc.title=Python \\ 
 startrecord    & 1 \\ 
 \hline
\end{tabular}
\end{center}

Parametrene \textit{version} og \textit{searchRetrieve} endres ikke. Parameter \textit{maximumRecords} kan endres for å øke eller redusere antallet resultater som returneres fra tjerneren. Prøv å endre parametrene i nettleseren. Tjerneren vil ha en standardverdi som benyttes hvis parameteren utelates. Prøv å fjerne parameteren og se hvor mange poster som kommer tilbake. 

\begin{verbatim}
https://brmbib.bib.no/cgi-bin/sru?version=1.2&operation=searchRetrieve
&recordSchema=dc&query=dc.title=Python&
startRecord=1
\end{verbatim}

Tjernern vil også sette en øvre grense for hvor mange resultater som kan returneres. 
Parameteren \textit{recordSchema} bestemmer hvordan resultatene returneres. Et vanlig skille er mellom MARC-baserte formater (se eksempel~\vref{hent-data-bibliofilmarc}) og Dublin Core (eksempel~\vref{hent-data-dc-xml}). (Oppgave: Endre verdien fra \textit{normarc} til \textit{dc} i URLen og se på resultatet). Biblioteksystemer har en liste over lovlige formater for Bibliofil SRU-tjerneren \footnote{https://dok.bibsyst.no/web/webapi/webapi-uthenting.html}. Prøv de ulike formater.

Den viktigste parameteren er \textit{query}, selve søket. Søket \textit{query=Python} søker i alle felt. For å avgrense søket til f.eks. tittelen bruker vi følgende syntaks \textit{query=dc.title=Python}.Denne syntaksen uttrykker at vi søker (query) i et bestemt felt (dc.title) etter forekomst av ordet "Python". Hvilke feltsøk som er mulig vil variere fra tjerner til tjerner. Leverandørene av biblioteksystemer som er i bruk i Norge har utformet en liste over alle feltsøk som tjernerene skal støtte. Dette kalles \textit{The NorZIG Profile for SRU}. Den nyeste versjon (1.2) er fra 2013\footnote{https://norzig.no/sru/profile/1.2/}. 

Noen vanlige felt:

\begin{itemize}
    \item dc.title
    \item dc.date
    \item dc.creator
    \item dc.subject
    \item dc.language
    \item ...
\end{itemize}

Oppgave: Klarer du å finne ut hvilke bøker av Jo Nesbø Bergen offentlige bibliotek har?

Boolske uttrykk kan brukes for å uforme mer avanserte søk, f.eks. \textit{query=Python and dc.date=2009}:

\begin{verbatim}
https://brmbib.bib.no/cgi-bin/sru?version=1.2&operation=searchRetrieve&
recordSchema=dc&query=dc.title=Python%20and%20dc.date=2009
&startRecord=1
\end{verbatim}
\begin{itemize}
\item Parameterparet \url{query=Python} innebærer at vi søker dokumenter hvor ordet Python forekommer i hvilkensomhelst av postens søkbare felt. 

\item \underline{and} i \url{query=Python%20}\underline{and}\url{%20dc.date=2009} betyr at begge kriteriene må stemme samtidig.
\end{itemize}
Du kan lese mer om dette i \textit{An Introduction to the Search/Retrieve URL Service (SRU)} av Eric Lease Morgan\footnote{http://www.ariadne.ac.uk/issue/40/morgan/}
-->


## Bruk av Python for å hente poster med SRU

<!--
Først hentes inn modulene vi trenger. De tre moduler brukes for sending av URLen (requests), behandling av XML (lxml) og
lagring av data (pandas).

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-moduler.png}
\end{center}
\caption{Hent inn moduler} 
\label{hent-data-moduler}
\end{figure}

I neste celle lager vi URLen som skal sendes til tjerneren. I de første tre linjer lager vi søket. Hvilke felt skal vi søke i (linje 1)? Hva skal vi søke etter (linje 2)? I linje 3 settes søkefeltet og søketerm sammen. Parameterene som sendes settes i en Python dictionary. Navnet til parameter brukes som nøkkel og verdien til parameter er verdien. I linje 16 opprette vi en variabel (sru\_server) for addressem til tjerneren. I linje 20 brukes \textit{get()}-funskjonen fra \textit{requests}-modulen for å sende URLen til tjerneren og tar imot svaret. Adressen til SRU-tjerneren og parametere er input til funksjonen.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-sru-url.png}
\end{center}
\caption{Lag SRU URL} 
\label{hent-data-sru-url}
\end{figure}

Linje 20 i eksempel~\vref{hent-data-send-url} lyder
\begin{center}
!r = requests.get(sru_server, params = params)!. 
\end{center}
Dette er en vanlig måte å angi \textit{funksjonsparametre} på. 
\begin{itemize}
\item Den første !params! angir at her skal vi spesifisere !requests.get()! sin parameter ved navn \textit{params}\footnote{Se gjerne dokumentasjonen til requests-pakka \url{https://requests.readthedocs.io/en/latest/api/}, særlig .get-funksjonen, \url{https://requests.readthedocs.io/en/latest/api/#requests.get}}. 

\item Den andre !params! er navnet på dict'en !params! fra linje 7. 
\end{itemize}

I den neste cellen (Eksempel~\vref{hent-data-send-url} ) sjekker vi svaret som er motatt fra tjerneren. Tjerneren sender en statuskode tilbake som en del av svaret. statuskode for at alt er OK vil alltid være 200, og det er derfor denne vi spør etter. Men det finnes statuskoder for andre ting\footnote{Se her: \url{https://en.wikipedia.org/wiki/List_of_HTTP_status_codes}  eller her: \url{https://www.w3schools.com/tags/ref_httpmessages.asp}}. 
Statuskoden finnes i attributtet !status_code!. Legg merke til at dette er et attributt til variabelen \textit{r} som var opprettet i linje 20 i forrige celle. !r! er en variabel av type !Response!\footnote{Se her:\url{https://requests.readthedocs.io/en/latest/api/#requests.Response}}, og pakker inn alt som kommer tilbake fra serveren etter at !Request!'en var sendt. Her ligger både statuskoder, XML-teksten og annen relevant informasjon. Hvis alt er OK, dvs. !r.status_kode == 200!, skjer en \textit{deserialisering}. Det betyr at xml (som kommer som en lang, \textit{seriell} tekst (karakter etter karakter) \anf{spres inn i} maskinens minne, slik at programmet på en effektiv måte kan finne fram til nødvendig informasjon i den. Variabelen !tree! representerer en slik \anf{trestruktur}, og linjen 
\begin{center}
    !tree = etree.fromstring(r.content)!
\end{center}
er ansvarlig for at tekststrengen !r.content! gjøres om til trestrukturen !tree!. Funksjonen !fromstring()! finnes i lxml-modulen som vi importerte tilbake i eksempel~\vref{hent-data-moduler}. 
 
 Da ligger mottatte XML-data i minnet, klare til behandling. Hvis noe har gått galt utføres linje 4 og en feilmelding skrives ut.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics[width=0.8\linewidth]{Images/Hent-data/hent-data-send-url.png}
\end{center}
\caption{Sjekk statuskoden og klargjør XML-data for parsing} 
\label{hent-data-send-url}
\end{figure}
-->

## XML


### Om XML-format. hierarki og nøsting

<!--
XML står for eXtensible Markup Language og er et dataformat som brukes for å markere data. XML er en W3C standard fra 1998. Aalberg og Hegna beskriver XML som "[det] universelle språket for strukturerte dokumenter og data på World Wide Web" (Fra Aalberg og Hegna, s. 80). Data ordnes i \textit{elementer}, markert med \textit{tagger}. Elementer har navn(tagg) og innhold. 

\begin{verbatim}
<element_navn>element-innhold</element_navn>
\end{verbatim}
 
 eller
 
 \begin{verbatim}
<tagg>element-innhold</tagg>
\end{verbatim}

Innholdet markeres altså vha. start- og slutttagger. Elementer kan nøstes innenfor hverandre, f.eks. nedenfor er \textit{tittel} og \textit{forfatter} nøstet innenfor \textit{bok}.:

\begin{verbatim}
<bok>
    <tittel>Python</tittel>
    <forfatter>
        <etternavn>Donaldson</etternavn> 
        <fornavn>Toby</fornavn>
    </forfatter>
</bok>
\end{verbatim}

Et XML dokument som følger standarden sies å være \textit{velformet}, dvs. det er et lovlig XML-dokument. Hovedreglene er at XML-dokumentet:

\begin{itemize}
    \item må inneholder ett eller flere elementer
    \item har kun ett rotelement. Slik at alle andre elementer er nøstet innenfor rotelementet. I eksemplet ovenfor er \textit{bok} rotelementet.
    \item består av lovlige nøstede elementer
\end{itemize}

Nedenfor vises et eksempel på XML returnert fra et SRU-søk.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-dc-xml.png}
\end{center}
\caption{XML-data fra en SRU-søk i Dublin Core-format} 
\label{hent-data-dc-xml}
\end{figure}

Nøsting er en viktig del av XML. Nøsting gjør at et XML dokument har en hierarkisk trestruktur. I eksempel~\vref{hent-data-xml-tree} ser vi at elementet \textit{SRU:searchRetrieveResponse} har fire elementer \textit{nøstet} under seg (nøsting representert ved piler): \textit{SRU:version}, \textit{SRU:numberOfRecords}, \textit{SRU:resultSetId} og \textit{SRU:records}. Av disse er den kun \textit{SRU:records} som har elementer nøstet under seg igjen. Direkte under \textit{SRU:records} finnes \textit{SRU:record}. Tre-strukturen er vist i tegningen under. Den øverste elementen, det som alle andre elementer kommer under kalles for \textit{rotelement}. Her er rotelementet \textit{SRU:searchRetrieveResponse}.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics[angle=90, width=0.5\linewidth]{Images/Hent-data/hent-data-xml-tree.png}
\end{center}
\caption{XML trestruktur} 
\label{hent-data-xml-tree}
\end{figure}

Et element som er nøstet rett under et annet element kalles for et barn. F.eks. \textit{SRU:numberOfRecords} er et barn av \textit{SRU:searchRetrieveResponse}. Omvendt er \textit{SRU:searchRetrieveResponse} foreldre til \textit{SRU:numberOfRecords}. Elementer på samme nivå i treet kalles for søsken, f.eks. \textit{SRU:numberOfRecords} har tre søsken: \textit{SRU:version}, \textit{SRU:resultSetId} og \textit{SRU:records}.

XML gjør det mulig å organisere data på en systematisk og stringent måte. Dersom vi kjenner den konkrete XML-strukturen våre data er uttrykt i, kan vi finne frem i de og trekke ut deler av de som interesserer oss. Det finnes forskjellige standarder / språk for analyse av XML-dokumenter, hvorav Xpath er en av de mer kjente.    
-->

### XPath

<!--
"XPath er et språk for å adressere deler av et XML-dokument" (Aalberg og Hegna, s. 82). XPath er i likhet med XML en W3C standard\footnote{https://www.w3.org/TR/xpath/}.

La oss anta at vi ønsker å finne antallet treff i XML-dokumentet som var vist tidligere. Vi vet at antallet treff fra søket finnes i elemententet \textit{SRU:numberOfRecords}. For å spesifisere adressen til dette elementet tar vi utgangspunkt i dokumentets rotelement(\textit{SRU:searchRetrieveResponse}). Det første /-tegnet markerer at dette er rotelementet. De øvrige /-tegn skiller nivåene i dokumentettreet.

\begin{verbatim}
/SRU:searchRetrieveResponse/SRU:numberOfrecords
\end{verbatim}

Vi kan tolke dette slik: finn \textit{SRU:numberOfrecords} som finnes innenfor \textit{SRU:searchRetrieveResponse}. Under er stien markert i trestrukturen:

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-xpath-tree-example1.png}
\end{center}
\caption{XML trestruktur - XPath for å finne antallet treff} 
\label{hent-data-xpath-tree-example1}
\end{figure}

Et annet eksempel kan være å finne alle tittelene. Dette er lengre ned i XML-treet. XPath-stien er derfor lengre:

\begin{verbatim}
/SRU:searchRetrieveResponse/SRU:records/SRU:record/SRU:recordData/dc/dc:title
\end{verbatim}

I ord kunne vi skrive: Finn \textit{dc:title} som er innenfor \textit{dc} som er innenfor \textit{SRU:recordData} som er innefor \textit{SRU:record} som er innenfor \textit{SRU:records} som er innenfor \textit{SRU:searchRetrieveResponse}. Her har vi nøsting på seks nivåer. I trestrukturen ser stien slik:

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-xpath-tree-example2.png}
\end{center}
\caption{XML trestruktur - XPath for å finne tittelen} 
\label{hent-data-xpath-tree-example2}
\end{figure}

Her har vi kun èn post, men svar på et søk kan inneholde flere poster. Dette betyr at flere elementer kan ha samme XPath.

Det finnes tjenester på nettet som lar deg teste XPath. F.eks. FreeFormatter\footnote{https://www.freeformatter.com/xpath-tester.html} har en slik tjeneste. Nedenfor vises en skjermdump. For å teste tjenesten med vårt SRU-eksempel, gjør følgende:
\begin{itemize}
    \item Start tjenesten i et nettleservindu  
    \item I et annet nettleservindu, lim søket inn i adressefeltet, og kjør.
    \item I tjenestevinduet, Option 1: Copy-paste your XML here
    \item kopier dokumentet fra SRU-søkresultatet og lim inn i feltet under \textbf{Option 1: Copy-paste your XML here}
    \item lim XPATH-spørringen inn i feltet under \textbf{XPath expression}, klikk \textbf{Evaluate Xpath} og se resultatet nederst.
\end{itemize}

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-test-xpath.png}
\end{center}
\caption{Test XPath med FreeFormatter} 
\label{hent-data-test-xpath}
\end{figure}

Vi kan også lage enkle eksempler for å teste XPath som vist under.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-test-xpath2.png}
\end{center}
\caption{Test XPath med FreeFormatter - eksempel 2} 
\label{hent-data-test-xpath}
\end{figure}

 Endre gjerne eksemplet og tilpass XPathene. Mer informasjon og eksempler av XPath spørringer finnes på W3Schools\footnote{https://www.w3schools.com/xml/xpath\_intro.asp}.


Hvordan vi utfører XPath-spørring i Jupyter vises i neste celle. Her skal vi finne ut hvor mange treff vi har fått på SRU-spørringen. Først må vi definere noen navnerom (engelsk: namespace).
-->

### XML navnerom

<!--
Når XML-elementer hentes fra ulike skjema, er det en fare for at samme elementnavn (tagg) finnes i flere av disse (<title> er et eksempel på en populær tagg). For å unngå konflikter, oppgir vi for visse elementer hvilket skjema elementet er hentet fra. Dette kalles for \textit{navnerom}. Et navnerom har en identifikasjon og en forkortelse (prefiks). Når vi, øverst i XML-dokumentet skriver:
\begin{center}
    xmlns:SRU=\anf{http://www.loc.gov/zing/sru},
\end{center}  
kobler vi forkortelsen \textit{SRU:} til det universelle navnerommet som er identifisert ved uri'en \url{http://www.loc.gov/zing/sru}\footnote{uri'en er ikke nødvendigvis en URL det går an å åpne på nettet, men bare en unik identifikasjon. Noen navnerom har informasjon som kan leses ved å gå inn i denne adressen, andre har det ikke.}
Mens URI'en er sentralt definert et sted, er det XML-forfatteren som kan velge prefikset (som har en del til felles med et variabelnavn). I utskrif~\vref{hent-data-xml-namespaces} markerer vi hvor i XML-filen navnerommene  \textit{SRU} og \textit{dc} er definert. 

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-xml-namespaces.png}
\end{center}
\caption{Definisjon av navnerom i et XML-dokument} 
\label{hent-data-xml-namespaces}
\end{figure}


Hvis XML-dokumentet bruker navnerom, må vi bruke navnerom også i våre XPath-spørringer. En Python dictionary brukes for å definere koblingen mellom prefikset og URIen. dict-Variabelen kaller vi !ns!. Nøkkelen i dictionary er prefikset, mens verdien er URIen. I eksempelet definerer vi to navnerom med prefiksene \textit{sru} og \textit{dc}. Prefiksene må ikke nødvendigvis være det samme som er brukt i XML-dokumentet, men URIen \textit{må} være det samme. Det er lurt å kopiere URIen fra dokumentet for å unngå feilstaving.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-namespaces.png}
\end{center}
\caption{Opprett navnerom} 
\label{hent-data-namespaces}
\end{figure}

Deretter kan vi anvende navnerommene i våre XPath-spørringer. På figur~\vref{hent-data-xpath-hits} finner vi antallet treff. På linje 1 oppretter vi en variabel som skal \anf{holde} XPath-spørringen vår (ha spørringsteksten som verdi). Legg merke til at prefikset  vi bruker her, \textit{sru}, er lik det vi definerte i forrige celle. På linje 3 utføres XPath-spørringen. Spørringen utføres på  variabelen !tree! (see omtale i avsnitt~\vref{hent-hente-poster}), som holder hele XML-dokumentet. Vi finner deen igjen da vi definerte den i en tidligere celle. Vi bruker !.xpath()!-metoden for å utføre spørringen. Metoden har to parametre: variabelen som inneholder XPath-spørringen (!xpath_query!) og variabelnavnet til dictionary som definerer navnerommene, (!ns!). Resultatet av XPath-spørringen er en \textbf{liste} av noder\footnote{I XML brukes ordet noder til å omtale hva som helst: elementer, attributter eller dokumenter.}. I vårt tilfelle er nodene elementer. En XPath-spørring \textbf{kan} resultatere i flere treff, f.eks. hvis vi har et XML-dokument med data om flere bøker, kan vi ha flere tittel-elementer (noder). Resultater er derfor \textbf{alltid} en liste med noder (i vårt tilfelle her, har denne listen bare èn node, men koden bør alltid åpne for muligheten at det er flere noder, derfor !for!-løkken). 

På linjene 5 og 6 går vi gjennom nodene i listen. Hver node (i sin tur) tilordnes til variabelen !hit_node!, som vi må trekke data ut av og behandle der og da. Dette gjør vi på linje 6, altså henter innholdet i noden, i dette tilfelle, antall treff på spørring etter bøker med ordet \textit{python} i tittelen. Dataverdien ligger i elementets innhold ($<tag>\textbf{dataverdi}</tag>$) som vi trekker ut fra nodens !.text!-egenskap (!hit_node.text!). Innholdet tilordnes til variabelen !number_of_hits!. Til slutt, i linje 8, skrives ut innhold i variabelen, dvs. antall treff. Legg merke til at eksemplet er litt \anf{utypisk}, da numberOfRecords bare oppstår èn gang, og er den eneste noden som svarer til Xpath-søket. Det beryt at listen !hit_nodes! bare holder en eneste node. Så i dette tilfellet, kunne vi sløyfe løkken og si !hit_node = hit_nodes[0]! på linje 5. Men dette er et spesialtilfelle.

\smallskip
\begin{figure}[H]
\centering
\includegraphics{Images/Hent-data/hent-data-xpath-hits.png}
\caption{XPath for å hente ut antall treff} 
\label{hent-data-xpath-hits}
\end{figure}

I neste celle skriver vi ut de ni titlene. Strukturen ligner på den i forrige celle. På linje 1 oppretter vi en variabel, !xpath-query!, som skal \anf{holde}  XPath-spørringen (ha spørringsteksten som verdi). Her tar vi det over to linjer for å unngå en altfor lang linje. !+=! på line 2 betyr legg påfølgende tekststreng til slutten av den tidligere strengen i variabelen (sammenføy). På linje 4 uføres XPath-spørringen og resultatene tilordnes variabelen !title_nodes!. På linje 6 går vi gjennom listen over nodene som samstemmer med spørringen. For hver av disse nodene tilordnes verdien til variabelen !title! (linje 7), som deretter skrives ut (siste linje).

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-xpath-dc-title.png}
\end{center}
\caption{XPath-spørring for å hente ut titlene på tittelsøk på Python} 
\label{hent-data-xpath-dc-title}
\end{figure}

Trestrukturen i XML-dokumentet vises nedenfor. Her vises kun to poster på grunn av plass. I vårt eksempel skulle det ha vært ni poster (\textit{SRU:record})-elementer under \textit{SRU:records}-elementet.

\smallskip
\begin{figure}[H]
\centering
\includegraphics{Images/Hent-data/hent-data-xpath-tree-example3.png}
\caption{Forneklet XML-trestruktur med to poster} 
\label{hent-data-xpath-tree-example3}
\end{figure}

Hvis vi ønsker å behandle flere noder for hver post, for eksempel, skrive ut tittel og forfatter, må vi gjøre dette i to trinn. Først må vi finne alle postene, og deretter hente og skrive ut tittel og forfatter for hver post. Dette vises i cellen i eksempel~\vref{hent-data-xpath-dc-records}. Linje 1-5 er trinn èn - finn postene. Linjene 6-18 er trinn to - for hver post finn og skriv ut tittelen og forfatteren. Som vi ser av resultatet under cellen, har ikke alle poster en forfatter. Et viktig punkt her, er at Xpath-spørringene to og tre i linjene 7 og 13 utføres på !record_node!, altså noden assosiert med en enkel metadatapost,  og ikke på !tree! (noden som "holder" hele dokumentet). Legg merke til at XPathene i linje 6 og 12 begynner \textbf{ikke} med \textit{/}-tegnet. Disse er \textit{relative} adresser, som tar utgangspunktet i \textbf{hvor vi  i trestrukturen \anf{befinner oss}}, og ikke i rotelementet som tidligere.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-xpath-dc-records.png}
\end{center}
\caption{To trinn XPath spørring for hver post} 
\label{hent-data-xpath-dc-records}
\end{figure}

På figur~\vref{hent-data-xpath-tree-step1}, ser vi XPath-spørringen markert på dokumentet-noden. I trinn én finnes flere poster, avhengig av antall som er funnet med SRU-søket.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-xpath-tree-trinn1.png}
\end{center}
\caption{To trinn XPath spørring for hver post - trinn 1 - finn postene} 
\label{hent-data-xpath-tree-step1}
\end{figure}

I trinn to tar vi utgangspunkt i de enkelte postene og henter ønsket data fra posten, for eksempel titler og forfattere. Dette gjentas for hver post som er funnet. I nåværende eksempel ni ganger.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-xpath-tree-trinn2.png}
\end{center}
\caption{To trinn XPath spørring for hver post - trinn 2 - finn postens tittel} 
\label{hent-data-xpath-tree-step2}
\end{figure}
-->

## MARCXML

<!--
Frem til nå har vi hentet data i Dublin Core-formatet. Dette gir de grunnleggende data som biblioteket har lagret om sin samling. For å få en fullstendig beskrivelse av hvert dokument kan vi etterspør data i et MARC-format. Bibliofil tilbyr \textit{bibliofilmarc}. For å få data i dette formatet, endrer vi verdien på \textit{recordSchema}-parameter fra \textit{dc} til \textit{bibliofilmarc}:

\begin{verbatim}
https://brmbib.bib.no/cgi-bin/sru?version=1.2&operation=searchRetrieve&
maximumRecords=10&recordSchema=bibliofilmarc&query=dc.title=Python&
startRecord=1
\end{verbatim}

Her er utdrag av data som vi får tilbake:

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-bibliofilmarc-xml.png}
\end{center}
\caption{Bibliofilmarc} 
\label{hent-data-bibliofilmarc}
\end{figure}

Legg merke til at rammen er den samme som i Dublin Core eksemplet som vi har brukt frem til nå. Forskjellen er innenfor \textit{SRU:recordData}-elementen. Legg også merke til at vi har et nytt navnerom. Den er definert med prefix=uri i \textbf{xmlns:marcxchange-attributtet} til
elementet \textit{marcxchange:record}:

\begin{verbatim}
xmlns:marcxchange="info:lc/xmlns/marcxchange-v1"
\end{verbatim}

For å behandle bibliofilmarc må vi først be om å få postene formattert i bibliofilmarc, og deretter endre navnerom. Viktige elementer her er \textit{marcxchange:controlfield}, \textit{marcxchange:datafield} og \textit{marcxchange:subfield}. Selve MARC-taggen (082, 100, 245, osv.) finnes i \textit{tag}-attributtet til \textit{marcxchange:datafield}-elementet. Mens delfeltkoden finnes i \textit{code}-attributtet til \textit{marcxchange:subfield}-elementet. F.eks. et tittelfelt (245 a) er kodet slik:

\begin{verbatim}
<marcxchange:datafield tag="245" ind1="1" ind2="0">
    <marcxchange:subfield code="a">Python</marcxchange:subfield>
</marcxchange:datafield>
\end{verbatim}

Det eneste vi endrer i forhold til det første eksemplet, er linje 11 hvor (\textit{recordSchema}) endres fra \textit{dc} til \textit{bibliofilmarc}:

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-sru-url-bibliofilmarc.png}
\end{center}
\caption{Endre recordSchema til bibliofilmarc} 
\label{hent-data-sru-url-bibliofilmarc}
\end{figure}

Vi sjekker at returkoden er 200, og oppretter XML treskrukturen som før (se figur \ref{hent-data-send-url}). Men vi må legge til et nytt navnerom. I linje 4 legger vi til det nye navnerommet. Vi bruker ikke dc-navnerommet så vi kunne fjernet den, men det ikke er nødvendig. Legg merke til at vi kan velge prefiks selv, vi har valgt \textit{marc}, men URIen må være lik den som står i XML-dokumentet.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-namespaces-marc.png}
\end{center}
\caption{Legg til navnerommet for bibliofilmarc} 
\label{hent-data-namespaces-bibliofilmarc}
\end{figure}

XPathene må også tilpasses. Fordi rammen i XML-dokumentet er uendret er linje 1-5 som før. XPath-spørring er over to linjer (6 og 7) for å unngå lange linjer i skjermdumpen. Her ser vi bruk av navnerommet \textit{marc}. Vi ser etter en \textit{marc:subfield} som finnes innenfor en \textit{marc:datafield}, som finnes innenfor en \textit{marc:record}. Det som er nytt her er at vi vil kun ha ett datafield-element som har et tag-attributt med verdi !245!. Syntaksen her er generell:

\begin{verbatim}
[@attributt = "verdi"]
\end{verbatim}

og i dette tilfellet:

\begin{verbatim}
[@tag = "245"]
\end{verbatim}


\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-xpath-marc-records.png}
\end{center}
\caption{Finn titler fra MARC data med XPath} 
\label{hent-data-xpath-marc-records}
\end{figure}

I den neste cellen bruker vi en XPath-spørring for å plukke ut emnedata. Utgangspunktet her er en SRU-spørring etter dokumenter utgitt i 2021 (dc.date=2021). Generelle emner er lagret i 650 \$a. Vi ser fra resultatene som skrives ut, at dokumenter kan har flere emner. Den første posten har fire emner (\textbf{Biler, Båter, Fly} og \textbf{Jernbaner}). Den andre posten har to emner (\textbf{Postapokalyptisk verden} og \textbf{Framtidssamfunn}). Og den tredje post har ingen emner. 

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-xpath-marc-subjects.png}
\end{center}
\caption{Finn emner fra MARC data med XPath} 
\label{hent-data-xpath-marc-subjects}
\end{figure}
-->

## Lagring av data som CSV

<!--
Vi vil ofte ønske å ta vare på data som vi har hentet ved hjelp av SRU eller andre metoder. Her skal vi lagre data i et format som heter CSV. Dette egner seg bra for viderebehandling i andre verktøy som vi bruker i denne boka, som OpenRefine og Pandas-modulen i Python. Det kan også tas inn i Excel og andre regneark. CSV er en tekstformat der data lagres i rader og kolonner.

CSV står for \textit{Comma Separated Values}. Dette er tekstfiler som har en tabell-struktur med rader og kolonner. Hver linje i filen er en rad. Kolonnene (datacellene) i hver rad skilles fra hverandre med et skilletegn. Skilletegnet kan være et komma, men også en semikolon. Et ligende format er TSV (Tab Seperated Values). Her er skilletegnet en TAB. Ofte inneholder første linjen i slike filer kolonnenavn. CSV (og TSV) filer er tekstfiler som kan åpnes og leses i alle teksteditorer. Eksempel på teksteditorer er Notepad++, TextPad, VS Code, Atom, Sublime Text, osv.

Her skal vi bruke Pandas-modulen for å lagre data som en CSV. Hvordan skal vi behandle kolonner når vi har flere verdier, f.eks. når vi har flere emner. En løsning er å bruke et skilletegn mellom verdiene. Her bruker vi !|! (vertikal strek) som skilletegn. Følgende celle produserer en tekststreng som samler alle emnene for et dokument. På linje 10 oppretter vi en tom tekststreng (!emner!) som skal ta vare på emnene. På linje 15 sjekker vi om variabelen er tom (ingen emner funnet ennå),  Hvis svaret er ja (tom), legger vi inn emnet uten skilletegn foran (16). Hvis svaret er nei, er dette \textbf{ikke} det første emnet, og vi setter inn ett skilletegn og sammenføyer emnet (linje 18).

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-xpath-marc-subjects-sep.png}
\end{center}
\caption{Finn emner fra MARC data med XPath og lagrer med skilletegn} 
\label{hent-data-xpath-marc-subjects-sep}
\end{figure}

I den neste cellen lagrer vi emnene i en CSV-fil vha av Pandas. Husk at Pandas var en av modulene vi lastet inn tidligere med følgende linje:

\begin{verbatim}
import pandas as pd
\end{verbatim}

Dataene vi etter hvert skal lagre i filen tar vi underveis vare på  i en liste av tupler. Dette vil ha følgende struktur:

\begin{verbatim}
[
(tittel, forfattere, dato, emner),
(tittel, forfattere, dato, emner),
(tittel, forfattere, dato, emner),
...
]
\end{verbatim}

Første tuppel har tittel, forfattere, dato og emner til første post i resultatlisten. Neste tuppel har tilsvarende data om neste post, osv. 

For å illustrere og samtidig spare størrelse på skjermbildet, tar vi \textit{i første omgang} kun vare på emner. Dataene vi skal  legge inn, ser altså slik ut:
\begin{verbatim}
[
(emner),
(emner),
(emner),
...
]
\end{verbatim}

I linje 1 oppretter vi en tom liste som heter !all_data!. I linje 22 legge vi emnene inn i en tuppel som legges inn i listen. Det er derfor det er to parantespar. Det innerste paret angir tuppelen, det ytreste tilhører !append()!-metoden til listen. Dette gjøres for hver post i XML-dokumentet (linjene 8-22). Når vi har behandlet alle postene oppretter vi en Pandas dataframe i linje 24 som kalles !df!. En dataframe en datatype som har en tabellstruktur (Mer om dette i kapitlet om Pandas). !DataFrame()! fra Pandas brukes for å opprette et nytt dataframe. Første parameter er data som skal legges inn i dataframe. Neste parameter er navn på kolonnene. I linje 25 opprettes en CSV-fil med data fra dataframe. Funksjonen \textit{to\_csv()} brukes for å opprette filen. Første parameter er navn og adressen til filen. Her er det bare en filnavn (\textit{baerum-2021.csv}). Dette betyr at CSV-filen legges i samme mappe som notebooken.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-marc-2-csv.png}
\end{center}
\caption{Lagrer data som CSV vha Pandas} 
\label{hent-data-marc-2-csv}
\end{figure}

Vi kan åpne CSVen i Jupyter for å se resultatet. Første linje er kolonneoverskriften(e) disse bestemte vi når vi opprettet dataframe i forrige bildet (linje 24). De øvrige linjer er dataene fra postene.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-marc-2-csv-resultat.png}
\end{center}
\caption{Se på CSV-filen i Jupyter} 
\label{hent-data-marc-2-csv-result}
\end{figure}

Neste skritt er å utvide dataframe med flere kolonner utover emner. Her legger vi kun inn tittel (linjer 11-16) og emner (linjer 18-28). Samme mønster kunne brukes for å legge til data fra flere felt.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-marc-2-csv-bigger.png}
\end{center}
\caption{Lagrer data som CSV flere kolonner} 
\label{hent-data-marc-2-csv-bigger}
\end{figure}

Her er en CSV-filen med tittel (245-a), forfatter (100-a), emner (650-a) og mediatype (336-a). Overraskende har ingen av de første 100 poster en personlig forfatter i felt 100.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-marc-2-csv-result-big.png}
\end{center}
\caption{CSV-fil med fire kolonner} 
\label{hent-data-marc-2-csv-result-big}
\end{figure}
-->

## OAI-PMH-protokollen

<!--
OAI-PMH står for Open Archives Initiative Protocol for Metadata Harvesting. Mens SRU er en protokoll for å søke i bibliotekkataloger, er OAI-PMH en protokoll for å innhøste metadata fra bibliotekkataloger. OAI-PMH brukes også  for å innhøste fra  andre kilder for metadata, f.eks. fagarkiver. Med OAI-PMH høster vi inn metadata som er registert innen et gitt tidsrom (fra dato til dato).

På samme måte som med SRU, sendes forespørselen til tjerneren vha av en URL, f.eks.:

\begin{verbatim}
https://oda.oslomet.no/oda-oai/request?verb=ListRecords&
from=1970-01-01&until=2021-11-15&metadataPrefix=qdc&
set=col_10642_6809
\end{verbatim}

Her bruker vi OAI-PMH-tjerneren til OsloMets fagarkiv ODA. \textit{https://oda.oslomet.no/oda-oai/request} er adressen til tjerneren. Etter \textit{?}-tegnet kommer parameterparene:

\begin{center}
\begin{tabular}{ |l| l | l | } 
 \hline
 \textbf{Parameternavn}  & \textbf{Parameterverdi} \\ 
 \hline
 verb        & ListRecords \\ 
 from        & 1970-01-01 \\
 until       & 2022-05-01 \\ 
 metadataPrefix   & qdc \\ 
 set          & col\_10642\_6809 \\ 
 \hline
\end{tabular}
\end{center}

\textit{verb} tilsvarer \textit{operation} i SRU. Hva vil vi gjøre? Her ønsker vi å liste metadataposter. De andre kommandoene (verb) brukes for å utforske funksjonaliteten til tjerneren. Eksempler kommer i fortsettelsen.

\textit{from} og \textit{until} brukes for å oppgi tidsrommet metadata er registrert. Formatet her er: ÅÅÅÅ-MM-DD (dvs., år, måned, dato).

\textit{metadataPrefix} tilsvarer \textit{recordSchema} som brukes i SRU. Dette angir hvilket format metadata skal returneres i. Vi kan bruke verbet \textit{ListMetadataFormats} for å finne ut hvilke formater tjerneren støtter:

\begin{verbatim}
https://oda.oslomet.no/oda-oai/request?
verb=ListMetadataFormats
\end{verbatim}

Hvis vi slår opp URLen i en nettleser, ser vi at tjerneren har støtte for tolv formater.

Tjerneren kan dele opp katalogen / databasen inn i deler. Disse kalles \textit{set} i OAI-PMH. F.eks. Alma har sett for hvert bibliotek som inngår i samarbeidet. ODA har sett for metadata om hvert institutts masteroppgaver. For å undersøke hvilke sett en tjerner tilbyr kan vi bruker verb \textit{ListSets}:

\begin{verbatim}
https://oda.oslomet.no/oda-oai/request?
verb=ListSets
\end{verbatim}

Her ser vi at identifikatoren til ABIs masteroppgaver er \textit{col\_10642\_6809}.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-oda-listsets.png}
\end{center}
\caption{Resultat av ListSets kommando mot ODA} 
\label{hent-data-oda-listsets}
\end{figure}

Fordi vi bruker URLene fra en nettleser vises resultatet som HTML. Hvis vi høyreklikker på nettsiden og velg \textit{View Page Source} (eller lignende) vises den underliggende XMLen. Det er dette XML vi skal prosessere i Jupyter. Kommandoer \textit{ListMetadataFormats} og \textit{ListSets} bruke vi når vi undersøker formatene og sett som en tjerneren tilbyr. Det er kommandoen \textit{ListRecords} som vi skal bruke i Jupyter for å hente metadata.
-->

## Bruk av Python for å hente poster med OAI-PMH

<!--
Her bruker vi de samme modulene som vi brukte med SRU: \textit{requests} for å sende en forespørsel til tjerneren og motta data; \textit{lxml} for å behandle XMLen og \textit{pandas} for å lagre data som en CSV-fil.

Etter at vi har hentet inn modulene, må vi lage URLen som skal sendes til tjerneren. Dette igjen ligner arbeidet med SRU. Men paramterene har ulike navn og verdier. Her kan vi endre i linjer 3 og 4 for å endre tidsrommet for innhøsting. Formatet kan endres i linje 5. Husk at dette må være et lovlig format i følge \textit{ListMetadataFormats}. Vi kan også endre sett, f.eks. for å hente metadata om masteroppgaver fra et annet institutt. Husk at identifikatoren for sett må være lovlig i følge \textit{ListSets}. Hvis vi skal hente metadata fra en annen tjerner som støtter OAI-PMH-protokollen, kan vi endre adressen til tjerneren i linje 9.

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-oai-url.png}
\end{center}
\caption{Konstruksjon og sending av URLen ti ODA OAI-PMH tjerneren} 
\label{hent-data-oai-url}
\end{figure}

I neste celle sjekker vi om statuskoden er 200 (alt er OK) og leser XML-data inn i en variabel som heter !tree!. Dette er helt likt det vi gjorde med SRU.

OAI-PMH bruker navnerom (namespaces). Vi må undersøke XML-data i nettleseren for å finne ut hvilke namespaces som brukes. Her har jeg kopierte starten av XML-dokumentet inn i Notepad++ for å forenkle og fremheve strukturen. I linje 2 ser vi eksempel av et \anf{default} navnerom. Her er det ikke oppgitt noen prefix, men når vi bruker XPath må vi gi navnerommet en prefix. Alle elementer under elementet som ikke har sin egen prefiks tilhører dette navnerommet, inkludert elementet hvor det er definert, dvs. \textit{OAI-PMH}. I tilegg til denne er det tre namespaces med prefikser, lenger ned under !<metadata>!:

\begin{itemize}
    \item qdc (http://dspace.org/qualifieddc/)
    \item dc (http://purl.org/dc/elements/1.1/)
    \item dcterms (http://purl.org/dc/terms/)
\end{itemize}

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-oai-xml.png}
\end{center}
\caption{Forenklet XML-data fra OAI forespørsel} 
\label{hent-data-oai-xml}
\end{figure}

Dette resulterer i følgende fire namespaces i Jupyter:

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-oai-namespaces.png}
\end{center}
\caption{Opprett prefikser og namespaces} 
\label{hent-data-oai-namespaces}
\end{figure}

Her har jeg valgt å gi "default" navnerommet prefikset \textit{oai}.

I neste celle bruker vi navnerom sammen med XPath for å skrive ut titler av ABIs masteroppgaver. Igjen, gjøres dette i to trinn. Først finner vi postene (linje 1-3) og deretter, for hver post (linje 5), finner vi tittelen (linjer 8-12) og skriver den ut (linje 15). Det er viktig å legge merke til at den første XPath kjøres på hele XML-dokumenter (dvs. variabelen \textit{tree}), mens den andre kjøres kun på posten (dvs. variabelen \textit{record\_node}).

\smallskip
\begin{figure}[H]
\begin{center}
\includegraphics{Images/Hent-data/hent-data-oai-xpath.png}
\end{center}
\caption{XPath-spørring mot OAI-PMH data} 
\label{hent-data-oai-xpath}
\end{figure}

Cellen kan bygges ut ved å hente flere felt fra posten. Data kan tas vare på og lagres i en CSV-fil. Dette tilsvarer det som er gjort tidligere.

\begin{comment}
\section{Annet}

Hvor mye av dette rekke vi?

\begin{itemize}
    \item Control fields?
    \item Paging
    \item Andre APIer
    \item JSON
\end{itemize}
\end{comment}
-->
# feltlengdeFv2019

Rekonstruerer utregning av feltlengde på fylkesveg per 1.7.2019 så godt det lar seg gjøre. SVV er bedt om å regne ut feltlengder på fylkesveg per 1.7.2019, summert per fylke slik fylkene så ut den gang. 

# Metode 

I stedet for objekttype 616 Feltstrekning (slik det står i Vianova-oppskriften) må vi bruke såkalt "segmentert vegnett" slik det så ut i per 2019-07-01. I vegkryss så vil såkalte konnekteringslenker mangle data for kjørefelt. Dette henter vi dels fra dagens 616 Feltstrekningobjekt (der dette matcher 2019-vegnettet), ellers henter vi fra tilstøtende veglenke. Deretter regner vi ut antall kjørefelt per vegsegment. Så finner vi hvilke vegsegment som er innafor de gamle fylkene, og "klipper" de vegsegmentene som krysser en fylkesgrense. Alle vegsegment får samtidig riktig fylkesnummer og fylkesnavn fra de gamle fylkene. Deretter er det å summere lengde av "klippede" vegsegment multiplisert med antall kjørefelt, aggregert per 2019-fylke. 

Fylkesgrenser for 2019 er lettvint tilgjengelig fra http://geonorge.no. 

# Vianova-metoden og utfordringer

Feltlengde i denne sammenhengen skal tas ut etter den såkalte "Vianova-oppskriften", som baserer seg på å summere antall kjørefelt multiplisert med lengden på vegnettet, med utgangspunkt i objekttypen 616 feltstrekning. Før summering skal man gjøre en viss filtrering: Vi regner kun på vegnett med fase = V (Veg som er del av operativt vegnett), på fylkesveg og for trafikantgruppe "Kjørende". Faglig sett har denne oppskriften en del svakheter, men det blir irrelevant i denne sammenhengen: Oppdraget er ikke å argumentere for en mer nøyaktig oppskrift, men følge oppskrift slavisk, så godt det lar seg gjøre. 

Vegobjekttype 616 har fra gammelt av vært brukt til å definere hvilke kjørefelt som finnes i NVDB. For noen år siden gjorde vi en modellendring i NVDB, der kjørefelt er en egenskap på veglenkene. 616-objektet er dermed overflødig, men blir videreført av hensyn til gamle klienter. Informasjon om kjørefelt blir dermed kopiert fra veglenkene til maskinelt genererte 616-objekter.

Av forskjellige årsaker var vi nødt til å "rydde opp i" en del uregelmessigheter i objekttypen 616 Feltstrekning. Dessverre var den beste løsningen å fjerne alle gamle data NVDB og opprette nye objekt. Dermed har vi ikke full historikk på 616 Feltstrekning. I stedet må vi ta ut NVDB vegnett (såkalt "segmentert vegnett"). En utfordring med dét er at såkalte konnekteringslenker ikke har noe kjørefeltinformasjon. En konnekteringslenke kan være for eksempel cirka  hundre meter lang strekning der to kjørefelt flettes sammen til ett, eller det kan være tre meters avstand fra asfaltkant til senterlinje i et T-kryss. På objekttypen 616 Feltstrekning har man syntetisk "lagt på" kjørefelt-informasjon fra tilstøtende veglenke. 

# Usikkerhet

Utregning av feltlengde per 2019-07-01 er beheftet med en viss usikkerhet, men min vurdering av usikkerheten er at den både er både lav og håndterbar. Jeg har ikke finregnet på det, men magefølelsen tilsier en usikkerheten lavere enn 2%. Samtidig er det greit å være tydelig på at ulike summeringsmåter garantert aldri gir eksakt samme resultat. "Vianova-oppskriften" summerer lengden av objekttypen 616 målt langs vegnett der data er satt sammen i regneark i en såkalt "V4-rapport". Vår metode baserer seg på å bruke kartverktøy til å regne ut lengder basert på geografiske koordinat til veglenkenene (mer presist "segmentert vegnett" fra NVDB api LES, som er en mer finkornet oppdeling av veglenkene). 

Den viktigste kilden til usikkerhet er såkalte "korreksjoner", det vil si at endringene får tilbakevirkende kraft. Man kan argumentere for at korreksjoner er såkalt feilretting, og at data dermed er riktigere og med høyere kvalitet, også i de historiske spørringene. Men noen ganger ønsker man et fasitsvar på hva forvaltningen hadde tilgjengelig av data. Slike svar kan vi gi når versjon 4 av NVDB api LES er ferdig utviklet, men per i dag (februar 2024) er det tilnærmet umulig. (Det er en teoretisk mulighet å rekonstruere ut fra NVDB endringslogg, men dette er ikke gjennomførbart i praksis). 

En slik korreksjon der vi har mulighet til å regne på differansen er en modellendring i NVDB der man gikk vekk fra å beskrive alle kjørefelt på  ferjeoppstillingsplass detaljert i selve vegnettet i NVDB. I stedet angir man lengde og antall kjørefelt i objekttypen 41 Ferjeoppstillingsplass. Denne modellendringen er utført som en korreksjon med tilbakevirkende kraft. Når vi regner på feltlengder etter den såkalte "Vianova-oppskriften" så ville man i 2019 ha telt med alle felt på ferjeoppstillingsplass, men dette er ikke lengre en del av vegnettet i 2024 - også når vi "stiller klokka" tilbake til 2019 i datauttaket. Man kan komme rundt dette problemet ved å analysere data for objekttype 41 Ferjeoppstillingsplass. Det har jeg gjort, og det utgjør totalt ca 11 km, med Troms (ca 6km) på topp, etterfulgt av Nordland (ca 2km). Dette anser jeg som så bagatellmessig at det ikke er hensiktsmessig å bruke tid på å "justere" de tallene jeg fant.   

En usikkerhet vi ikke tar høyde for er at vegnettet "korrigeres". I hovedsak så gjøres dette for å erstatte unøyaktige data med nyere og mer presis geometri, men noen ganger blir det også gjort av andre årsaker. Men den nye vegen som er redigert inn er neppe så veldig ulik i lengde og antall kjørefelt enn den gamle. Den normale arbeidsoperasjonen er tross alt at vegnettet redigeres slik at historikken blir bevart (såkalt "oppdatering"), og da er det trivielt å få ut de historiske dataene. 


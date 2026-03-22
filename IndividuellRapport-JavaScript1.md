# Individuell Rapport - Väderapplikation
**Namn:** Sanel Osmanovic
**Kurs:** JavaScript 1
----------------------------------------------
## DEL 1 - Teori: Web API:er

### Fråga 1: Vad är ett API? - *Förklara vad ett API (Application Programming Interface) är och varför API:er används inom webbutveckling. Ge minst ett konkret exempel på ett web-API och vad det kan användas till.*

Ett API (**Application Programming Interface**) fungerar som en bro eller en förmedlare mellan två olika program. Vi använder API för att kommunicera med en server.

T.ex. om jag sitter på en restaurang och ska äta mat. Jag kan inte bara gå in till köket (servern) och hämta min mat utan troligtvis kommer det en servitör (API) som tar emot min beställning och sedan går till köket (servern) som vet hur man lagar min mat och kommer tillbaka med det jag beställt till mig.

I vår väderapp t.ex. så använde vi oss av `WeatherAPI` för att hämta realtidsdata om temperatur och prognoser.
Vår app skickar ett anrop (en beställning) med en stad t.ex. Stockholm. API:et tar emot det, hämtar rätt data från sina servrar och skickar tillbaka ett svar som vi visar för användaren.

Anledningen till att man använder API är för att man kan återanvända data. Jag behöver t.ex. inte äga en egen satellit för att visa väder.
Det gör att utvecklare kan fokusera på att bygga en bra användarupplevelse istället för att lägga tid på att samla in rådatan själv.

### Fråga 2: HTTP-metoder - *Beskriv följande HTTP-metoder och förklara när varje metod används: GET, POST, PUT och DELETE.*

Dessa metoder talar om för servern vilken typ av handling vi vill utföra och de brukar benämnas som CRUD.

* **GET (Read):** Används för att hämta data från en server.
Exempel: Vår väderapp hämtar den aktuella temperaturen för en stad.

* **POST (Create):** Används för att skicka ny data till servern.
Exempel: Om vi hade haft en funktion för att skapa ett nytt konto med användarnamn och lösenord.

* **PUT (Update):** Används för att uppdatera eller ersätta något som redan finns på servern.
Exempel: Om användaren vill ändra sitt namn i sin profil eller liknande.

* **DELETE (Delete):** Precis som det låter. Används för att ta bort något från servern.
Exempel: I vår app så kan man lägga till en stad som favorit. Genom att ha ett `DELETE`-anrop så vet servern att just den staden ska inte vara favoritstad längre.

### Fråga 3: JSON - *Förklara vad JSON (JavaScript Object Notation) är och varför det är ett vanligt format för att skicka data mellan klient och server. Visa ett eget exempel på ett JSON-objekt och förklara dess struktur.*

När jag gör ett `GET`-anrop för att t.ex. hämta aktuell temperatur för en specifik stad så får man oftast tillbaka ett svar och det svaret skickas som ett JSON-format. Det är ett universellt format som gör att olika system kan prata med varanda oavset vilket programmeringsspråk de är byggda i.
Det är lättläst eftersom det liknar vanliga objekt i JavaScript.
Den innehåller inte heller en massa onödig kod vilket gör att överföringen går snabbt.

**Exempel på hur data från vår väderapp kan se ut när den skickas tillbaka till oss från servern:**

```json
{
    "city": "Göteborg",
    "temperature": 12.5,
    "isRaining": false,
    "coordinates": {
        "lat": 57.7,
        "lon": 11.9
    },
    "tags": ["Scandinavia", "Coastal"]
}
```

* Dessa "måsvingar" {} omsluter hela objektet.
* Data sparas i par som t.ex. "city" som är nyckeln och "Göteborg" som är värdet.
* JSON stöder olika typer av data som t.ex. Strängar, Numbers, Booleans Objekt(inuti objektet) eller Arrays.

### Fråga 4: HTTP-statuskoder - *Förklara vad HTTP-statuskoder är och beskriv vad följande koder innebär: 200, 201, 400, 404 och 500.*

HTTP-statuskoder är serverns sätt att svara på vår förfrågan som talar om hur det gick. Alltså om allt gick bra eller om något gick fel i samband med vårt anrop. Det görs med en siffra.

Man skulle kunna dela in det i fem grupper av statuskoder:

* 100 - 199 : Informationssvar
* 200 - 299 : Framgångssvar
* 300 - 399 : Omdirigering
* 400 - 499 : Klientfelsvar
* 500 - 599 : Serverfelsvar

De jag ska berätta mer specifikt om är:

* 200: Detta är det vanligaste framgångssvaret. Det betyder att allt gick bra och servern har levererat det vi bad om.
Exempel: När vi söker på "Göteborg" och får tillbaka JSON-data med vädret.

* 201: Då kan det t.ex. stå 201 Created. Och betyder att anropet lyckades och servern har skapat något.
Exempel: Om vi hade skapat en ny användare i vår väderapp och servern bekräftar att kontot nu finns.

* 400: Servern förstår inte vad vi vill och beror oftast på att vi skickat in dålig syntax och kan inte behandlas av servern.

* 404: Indikerar att vi letar efter någonting som inte finns på servern.
Exempel: Om användaren söker efter en stad som inte finns då svarar API:et med 404 för att säga att staden inte hittades.

* 500: Detta indikerar att ett fel inträffade på servern. Det kan bero på att servern är trasig eller överbelastad även om vi gjort allt rätt i anropet.

### Fråga 5: Fetch i praktiken - *Förklara steg för steg vad som händer när en webbsida använder fetch() för att hämta data från ett API. Beskriv flödet från att anropet skickas till att datan visas på sidan.*

I vår app använder vi funktionen fetch() för att hämta väderdata. Och jag ska försöka berätta om processen i vår loadWeather-funktion.

Vi använder nyckelordet async framför själva funktionen och await när vi anropar API:et. Detta gör vi för att hämtningen tar tid och await pausar körningen i just den funktionen tills vi fått ett svar eller ett promise (tänk dig att ett promise är en sådan puck som du får på en restaurang efter att du beställt mat, så medan du sitter vid bordet och pratar med din sambo så lagar de maten åt dig och sedan piper den till när maten är färdig. Själva pucken är ett promise om att du kommer få din mat.) utan att webbläsaren låser sig.

Själva anropet ligger inuti ett try {...} och catch (error) {...}-block
Try: Med try så försöker vi hämta datan
Catch: Om någonting skulle gå fel (t.ex. att servern ligger nere) fångas felet upp. Då visas ett felmeddelande "Could not fetch weather data".

Och sedan när getWeatherForecast har gjort sitt jobb och omvandlat JSON-svaret till ett JavaScript-objekt kan vi börja plocka ut det vi behöver med hjälp av const.
T.ex.
const currentWeather = weatherData.current;
const location = weatherData.location;

Det sista steget är att skicka vidare datan till olika "render"-funktioner. Dessa funktioner tar värdena från objektet och placerar ut dem i vår HTML så att användaren kan se temperatur, ort och prognos.

----------------------------------------------

## VG-Nivå

### Fråga 6: REST-principer - *Förklara vad ett RESTful API är. Vilka principer bygger REST på? Ge ett exempel på hur en resurs (t.ex. "användare" eller "produkter") kan representeras med RESTful endpoints och olika HTTP-metoder.*

Ett RESTful API är en samling regler för hur API ska byggas för att det ska vara effektivt och lätt att förså. Och för att kunna kommunicera använder man klienter och server till det. Klienten är till för att prata med servern och servern innehåller API:et och vi använder HTTP som protokoll.

RESTful API bygger på tre principer:

* Klienten (vår väderapp) och servern (weatherAPI) är helt fristående. Så länge de följer REST-reglerna kan servern uppdatera sin kod utan att vår app går sönder.

* Servern sparar inte någon information om tidigare anrop. Varför gång vi ber om vädret måste vi ha med all information.

* I ett RESTful API så ser man all data som "resurser" och varje resurs har en unik adress, en så kallad endpoint.

Om jag t.ex. skulle ha en webshop skulle endpointsen kunna se ut såhär.

* Hämta alla produkter = GET (HTTP-metod) /produkter (Endpoint)
* Hämta en specifik produkt = GET (HTTP-metod) /produkter/101 (Endpoint)
* Lägg till en ny produkt = POST (HTTP-metod) /produkter (Endpoint)
* Ta bort en produkt = DELETE (HTTP-metod) /produkter/101 (Endpoint)

Vår WeatherAPI är RESTful eftersom vi använder tydlig endpoint för att hämta just den resursen vi vill ha alltså väderprognosen.

### Fråga 7: Synkron vs asynkron kommunikation - *Förklara skillnaden mellan synkron och asynkron kod i JavaScript. Varför är API-anrop asynkrona? Beskriv hur async/await och Promises fungerar och hur de hänger ihop med varandra.*

Synkron kod = Den körs uppifrån och ner, en rad i taget. Varje rad måste bli helt klar innan nästa rad påbörjas.

Exempel: Om jag står i en fika kö. Jag står först i kön och beställer en macka, kanelbulle och en kopp kaffe. Jag lämnar inte disken förens jag fått det jag beställt så om det tar 15 min att få det jag beställt så kommer de människor som är bakom mig i kön få vänta 15 minuter. Kanske en dålig liknelse, hehe.

Asynkron kod = Den tillåter tidskrävande uppgifter att köras i bakgrunden medan resten av programmet fortsätter att köras.

Exempel: Den liknelsen jag berättade om för att förklara ett promise. Jag beställer mat och får en puck (ett promise) och sätter mig vid bordet med min sambo. Medan maten lagas kan kassan fortsätta ta emot andra beställningar och jag kan ta det lugnt med min sambo vid bordet.

När man förklarar skillnaderna på asynkron kod vs synkron kod så kan man snabbt konstantera varför API-androp är asynkrona. Vi vet ju inte hur lång tid det tar för en server (som vi inte vet var den befinner sig) att svara. Det kan ta 100 millisekunder eller 5 sekunder. Om API-anropet skulle vara synkrona så skulle hela sidan stanna medan vi väntar på att få ett svar på vad vädret är i Stockholm.

* Promise: Det är ett objekt som representerar ett framtida svar. Det kan vara Pending (väntar), Fullfilled (lyckat) eller Rejected (fel). Fulfilled är det som händer när vi hamnar i try-blocket och får vår data. Rejected är det som händer när vi hamnar i catch-blocket för att något gick fel.

* Async/Await: Detta är ett modernt sätt att skriva kod som hanterar promises.

- Async talar om för JavaScript att den här funktionen kommer innehålla asynkrona steg.
- Await säger till JavaScript att vänta till promise är klart, hämta datan och fortsätt sedan till nästa rad.

### Fråga 8: Felhantering och säkerhet - *Diskutera varför felhantering är viktigt vid API-anrop. Ge exempel på vad som kan gå fel och hur man som utvecklare kan hantera det. Resonera även kort kring säkerhetsaspekter vid API-användning (t.ex. API-nycklar, CORS, rate limiting).*

Eftersom att vi kommunicerar med en extern server vid API-anrop så kan man aldrig garantera att servern svarar. Därför är felhantering jätte viktigt, utan den kan appen/sidan sluta svara på klick eller visa en tom skärm.

Några exempel på saker som kan gå fel är:

* Nätverksfel - Användaren tappar anslutningen eller har instabilt nät.

* Serverfel - 500-statuskod fel som jag pratade om tidigare. Servern kanske ligger nere av någon anledning.

* Klientfel - 400-statuskod. Användaren söker på en stad som inte finns eller kanske till och med att API-nyckeln är ogiltig.

* Rate Limiting - Att man gör för många anrop på kort tid.

Som utvecklare är man tvungen att hantera dessa fel och därför använder man `try` och `catch` runt våra funktioner.

I `try` så kör vi själva anropet och om något går fel så fångar `catch` upp felet. Där kan man som utvecklare "logga" felet för oss själva via `console.error`, men kanske framför allt ge användaren feedback istället för att inte visa någonting. T.ex. "Oops, något gick fel, försök igen!"

När man ska skapa en hemsida eller en app så vill man ju gärna inte att information om andra användare (om man har så att man kan skapa ett konto på en sida) ska hamna i fel händer, likaså en API-nyckel. En API-nyckel är ju som ett lösenord för vårt konto på aktuell server eller hos den tjänsten man använder. Om man laddar upp nyckeln till GitHub så kan vem som helst stjäla den. I vårt projekt så löste vi det genom att lagra API-nyckeln i en separat fil `(config.js)` lokalt som vi sedan exkluderade från Git med en `.gitignore`-fil.

Det finns också någonting som heter CORS (Cross-Origin Resource Sharing). Det är en säkerhetsfunktion i webbläsaren som bestämmer vem som får hämta data från ett API. Om ett API inte tillåter anrop från min lokala Live Server får vi ett CORS-fel. Tanken med det är att det ska skydda servern från att svara på anrop som kommer från fel ställe, t.ex. en sida som försöker stjäla din data.

En sak som vi råkade ut för i vår grupp i samband med denna uppgift var MIME-type. Och det fick vi lära oss att webbläsaren använder MIME-typer som ett säkerhetsfilter för att kontrollera att rätt sorts fil levereras.

----------------------------------------------

## DEL 2

### 1. Din roll i gruppen - *Beskriv vad du ansvarade för i projektet. Vilka delar av koden eller projektet arbetade du med?*

I detta projekt var det min uppgift att ordna favorithanteringen, localStorage samt felhantering.

Användaren ska kunna spara sina favoritstäder, det ska också synas vilka favoritstäder som användaren valt samt att det visar tydligt när en stad är en favorit. Jag arbetade i storage.js och main.js för att implementera funktionerna som sparar och hämtar data från webbläsarens localStorage. Även om användaren laddar om sidan så ska informationen sparas och då använde jag JSON.stringify och JSON.parse för att behålla datan.

En annan uppgift jag hade var att tilgodose sidan med felhantering. Jag implementerade `try - catch`-block i de asynkrona anropen i main.js och utils.js. Detta för att även om ett API-anrop misslyckades, så kraschar inte sidan utan användaren får ett tydligt felmeddelande via showError.

Jag fick samarbeta med Albrim (som ansvarade för dropdown funktionen) för att säkerställa att logiken mellan sökfältet, API-anropet och dropdown funktionen fungerade korrekt. Att rätt stad skickades vidare till renderingsfunktionerna.


### 2. Samarbete och kommunikation - *Hur fungerade samarbetet i gruppen? Hur kommunicerade ni och hur fattade ni beslut? Ge minst ett konkret exempel.*

Jag vill börja med att skriva att jag är oerhört glad att jag hamnat i denna grupp och jag fortsätter gärna samarbeta med detta gäng framöver vid gruppuppgifter. Jag tycker att samarbetet med gruppen är och går jättebra. Det finns ingen som tar mer plats än någon annan men alla är delaktiga. Vi diskuterar alla beslut tillsammans och vi analyserar fördelar och nackdelar noggrannt innan beslut tas. Det är prestigelöst vilket är oerhört skönt.

Vi kommunicerade via Discord. Med detta projekt bestämde vi oss för att träffas två dagar i veckan för att gå igenom vad som ska göras, hur långt vi kommit och vad som är kvar att göra. Vi använde Google Docs för att dokumentera alla möten så att man kunde gå tillbaka och kolla samt om någon inte kunde närvara på mötet kunde man ta del om vad som diskuterats under själva mötet på det sättet.

Vi var alla aktiva i Discord-chatten för att informera varandra eller ställa frågor till varandra vid behöv. Hade vi någonting som man var tvungen att ta upp till näst kommande möte så kunde man skriva det i chatten. Alla större beslut togs på något av dessa två möten vi hade men var det mindre ändringar eller frågetecken så kunde vi ta gemensamma beslut via chatten också. Vi använde oss av Pull Requests för att granska varandras kod, vilket också gjorde att man hade koll på helheten även om man ansvarade för sin egen funktion eller del.

Ett konkret exempel jag har i huvudet är just dropdown-funktionen som en i gruppen hade ansvar för. Det var jätte många konflikter i den branchen och det var konflikter i filer som inte ens hade skrivits någon kod i. Vi diskuterade på ett möte hur vi skulle göra med den branchen och vi tittade även på alla konflikter tillsammans för att försöka lösa det. Men i detta möte kom vi fram till att vi skulle ta bort branchen och börja om helt. Ingången till den diskussionen var hela tiden att den som ansvarade för dropdown-funktionen också skulle vara den som committar funktionen till Git så att det blir synligt om denna skulle vilja ha med det i sitt "portfolio".

### 3. Tekniska utmaningar - *Beskriv minst en teknisk utmaning du stötte på under projektet. Hur löste du den? Vad lärde du dig av det?*

En teknisk utmaning som jag kämpade med en del var att hantera datan från favoritstäder med hjälp av localStorage.

localStorage kan endast lagra information som textsträngar, men jag behövde hantera en hel array med städer som användaren valt som favoriter. I början hade jag problem med att datan försvann eller inte sparades vilket gjorde att appen inte kunde läsa tillbaka favoriterna efter att sidan laddats om. Dessutom var jag tvungen att ordna så att en stad inte sparades flera gånger i samma lista.

Jag löste det genom att använda JSON.stringify och JSON.parse. När jag skulle spara listan använde jag JSON.stringify för att göra om min array till en textsträng som webbläsaren kan lagra. När jag sedan skulle hämta datan använde jag JSON.parse för att göra om den till en fungerande JavaScript-array igen. Och för att undivka dubletter använde jag .includes() för att kontrollera om staden redan fanns i listan när jag lade till den.

Jag fick en förståelse för hur man konverterar data mellan olika format. Något som jag också lärde mig var att skriva kod som kontrollerar om localStorage är helt tomt första gången användare öppnar appen, så att applikationen inte kraschar utan istället returnerar en tom lista.

### 4. Resultatet - *Är du nöjd med slutresultatet? Motivera varför eller varför inte.*

Jag är nöjd med resultatet och att vi skapat en stabil och snygg väderapp som faktiskt går att använda på riktigt. Jag är nöjd med appens helhet men också de funktioner jag själv ansvarade för.

Vi lyckades hålla koden modulär, ren och tydlig. Vi har separata filer och api.js sköter bara kontakten med vädertjänsten. Vi har storage.js som bara sköter localStorage. Vi har ui.js som sköter hur det ser ut på skärmen. Om vi hade velat ha en databas för hur vi sparar favoriter så behöver vi bara ändra i storage.js utan att påverka någon annan fil, det är modulär kod.

### 5. Gruppdynamik och arbetsprocess - *Analysera hur er arbetsprocess fungerade. Vad fungerade bra och vad fungerade mindre bra? Om du fick göra om projektet, vad hade du velat ändra i hur gruppen arbetade? Koppla gärna till begrepp som agilt arbetssätt eller liknande.*

Vår arbetsprocess var lite inspirerad av ett agilt arbetssätt. Vi hade regelbundna avstämingar två gånger i veckan som fungerade som "stand ups", vi gick igenom var vi står, vad som är kvar och vad som gjorts. Vi diskuterade även så kallade "blockers" om någon upplevde något sådant. Dessa möten dokumenterades i Google Docs för att kunna gå tillbaka och kolla, om man missade mötet kunde man se vad som diskuterats. Vår kommunikation via Discord var effektiv och vi hjälpte varandra direkt när problem uppstod.

Vi använde oss av en Kanban-tavla där vi använde User Stories, och vi tilldelade de som ansvarade för vad samt att man flyttade aktuell storie till "Done" när man var klar.

I Git jobbade vi tydligt med branches samt att de är döpta likadant (feature/namn-på-funktionen) och att vi jobbade med Pull Request för att granska varandras koder innan merge.

Som Maryam nämnde i vår presentation är vi överlag vädligt nöjda med vår arbetsprocess och skulle gärna jobba ihop igen. Om vi skulle ändra på något, så vore det att vara ännu mer detaljerade i våra User Stories från början. Det hade hjälpt oss att säkerställa att inga smådetaljer missades.

### 6. Din egen utveckling - *Reflektera över din egen utveckling som webbutvecklare under detta projekt. Vilka kunskaper hade du innan, och vad kan du nu som du inte kunde förut? Var specifik och ge konkreta exempel på kod eller koncept du har lärt dig.*

Innan detta projekt hade jag grundläggande kunskaper i HTML och CSS, samt en teoretisk förståelse för variabler och lite enklare funktioner. Jag var lite osäker på hur man faktiskt skapar en dynamisk webbsida där data hämtas, bearbetas och sparas.

-Jag har lärt mig arbeta med fetch och async/await. Det var lite luddigt i början men nu förstår jag hur man hämtar data från en annan källa t.ex. WeatherAPI och hanterar väntetiden innan datan presenteras.

-Nu är det också lite tydligare för mig hur man skriver logik för att spara och hämta arrays genom att använda JSON.stringify och JSON.parse. Ett exempel är hur jag skapade funktionen som kontrollerar om en stad redan finns i favoriter innan den sparas:

if (!favorites.includes(city)).....

-För att användarupplevelsen ska vara bra måste man ju ge användaren tydlig feedback. Till exempel om en stad inte hittas. Att använda try-catch-block och if-satser för detta är oerhört viktigt för användaren istället för att väderappen bara slutar fungera.

-Jag har också lärt mig värdet av att dela upp koden i moduler och använda export/import. Det gör det enklare att både läsa och underhålla koden.

### 7. Koppling till yrkesrollen - *Resonera kring hur de erfarenheter du gjort under projektet kan vara relevanta i din framtida yrkesroll som webbutvecklare. Hur skiljer sig ett skolprojekt från ett projekt i arbetslivet, och vad kan du ta med dig?*

De erfarenheterna från detta projekt är jätterelevanta för min framtida roll som webbutvecklare skulle jag säga. Det jag tar med mig från denna erfarenhet är just att den kod jag skriver ska integreras med en annan funktion som någon annan skriver. Vilket betyder att jag måste skriva kod som mina kollegor kan förstå, underhålla och eventuellt bygga vidare på.

Att samarbeta i/med Git, Pull Requests och code review skulle jag tro är standard i branschen, vilket gör mig ännu mer glad över att ha fått uppleva bara en liten del av hur det hade känts att ha detta som ett jobb.

Jag tar också med mig just det modulära tänkandet. Vi skrev kanske 1/10-del kod i detta projekt jämfört med hur det kan se ut i ett yrkesprojekt där det finns hur mycket kod samt filer som helst.

Skillnaden mellan ett skolprojekt och arbetslivet:

Jag är ju assisterande tränare i ett fotbollslag på min fritid och inom fotbollen så pratar vi om att vår "gameplan" måste vara ett levande dokument. Jag skulle tro att ett projekt i arbetslivet också måste vara ett levande dokument, eller i detta fall en levande produkt, eftersom det utvecklas hela tiden och underhålls kanske i flera år. Jämfört med ett skolprojekt där man kanske får ett fast slutdatum då du ska redovisa eller lämna in och sedan är det klart. Ansvaret tror jag också skiljer sig en del mellan ett skolprojekt och projekt i arbetslivet. I skolan gör man saker för att få ett betyg men i arbetslivet gör jag saker som ska fungera för många människor varje dag. Det ställer helt andra krav på t.ex. min kod.

I arbetslivet har man säkert också UX-designers eller produktägare att förhålla sig till, vilket gör att det kanske är striktare krav på någonting som kanske t.ex. prestanda och/eller säkerhet.

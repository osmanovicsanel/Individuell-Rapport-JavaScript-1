# Individuell Rapport - Väderapp
**Sanel Osmanovic**
----------------------------------------------
## DEL 1 - Teori: Web API:er

### Fråga 1: Vad är ett API?

Ett API (Application Programming Interface) fungerar som en bro eller en förmedlare mellan två olika program. Vi använder API för att kommunicera med en server.

T.ex. om jag sitter på en restaurang och ska äta mat. Jag kan inte bara gå in till köket (servern) och hämta min mat utan troligtvis kommer det en servitör (API) som tar emot min beställning och sedan går till köket (servern) som vet hur man lagar min mat och kommer tillbaka med det jag beställt till mig.

I vår väderapp t.ex. så använde vi oss av WeatherAPI för att hämta realtidsdata om temperatur och prognoser.
Vår app skickar ett anrop (en beställning) med en stad t.ex. Stockholm. API:et tar emot det, hämtar rätt data från sina servrar och skickar tillbaka ett svar som vi visar för användaren.

Anledningen till att man använder API är för att man kan återanvända data. Jag behöver t.ex. inte äga en egen satellit för att visa väder.
Det gör att utvecklare kan fokusera på att bygga en bra användarupplevelse istället för att lägga tid på att samla in rådatan själv.

### Fråga 2: HTTP-metoder

Dessa metoder talar om för servern vilken typ av handling vi vill utföra och de brukar benämnas som CRUD.

* **GET**(Read): Används för att hämta data från en server.
Exempel: Vår väderapp hämtar den aktuella temperaturen för en stad.

* **POST**(Create): Används för att skicka ny data till servern.
Exempel: Om vi hade haft en funktion för att skapa ett nytt konto med användarnamn och lösenord.

* **PUT**(Update): Används för att uppdatera eller ersätta något som redan finns på servern.
Exempel: Om användaren vill ändra sitt namn i sin profil eller liknande.

* **DELETE**(Delete): Precis som det låter. Används för att ta bort något från servern.
Exempel: I vår app så kan man lägga till en stad som favorit. Genom att ha ett DELETE-anrop så vet servern att just den staden ska inte vara favoritstad längre.

### Fråga 3: JSON (JavaScript Object Notation)

När jag gör ett GET-anrop för att t.ex. hämta aktuell temperatur för en specifik stad så får man oftast tillbaka ett svar och det svaret skickas som ett JSON-format. Det är ett universellt format som gör att olika system kan prata med varanda oavset vilket programmeringsspråk de är byggda i.
Det är lättläst eftersom det liknar vanliga objekt i JavaScript.
Den innehåller inte heller en massa onödig kod vilket gör att överföringen går snabbt.

Exempel på hur data från vår väderapp kan se ut när den skickas tillbaka till oss från servern:

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

### Fråga 4: HTTP-statuskoder

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

### Fråga 5: Fetch i praktiken

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

### Fråga 6: REST-principer

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

### Fråga 7: Synkron vs asynkron kommunikation

Synkron kod = Den körs uppifrån och ner, en rad i taget. Varje rad måste bli helt klar innan nästa rad påbörjas.

Exempel: Om jag står i en fika kö. Jag står först i kön och beställer en macka, kanelbulle och en kopp kaffe. Jag lämnar inte disken förens jag fått det jag beställt så om det tar 15 min att få det jag beställt så kommer de människor som är bakom mig i kön få vänta 15 minuter. Kanske en dålig liknelse, hehe.

Asynkron kod = Den tillåter tidskrävande uppgifter att köras i bakgrunden medan resten av programmet fortsätter att köras.

Exempel: Den liknelsen jag berättade om för att förklara ett promise. Jag beställer mat och får en puck (ett promise) och sätter mig vid bordet med min sambo. Medan maten lagas kan kassan fortsätta ta emot andra beställningar och jag kan ta det lugnt med min sambo vid bordet.

När man förklarar skillnaderna på asynkron kod vs synkron kod så kan man snabbt konstantera varför API-androp är asynkrona. Vi vet ju inte hur lång tid det tar för en server (som vi inte vet var den befinner sig) att svara. Det kan ta 100 millisekunder eller 5 sekunder. Om API-anropet skulle vara synkrona så skulle hela sidan stanna medan vi väntar på att få ett svar på vad vädret är i Stockholm.

* Promise: Det är ett objekt som representerar ett framtida svar. Det kan vara Pending (väntar), Fullfilled (lyckat) eller Rejected (fel). Fulfilled är det som händer när vi hamnar i try-blocket och får vår data. Rejected är det som händer när vi hamnar i catch-blocket för att något gick fel.

* Async/Await: Detta är ett modernt sätt att skriva kod som hanterar promises.

- Async talar om för JavaScript att den här funktionen kommer innehålla asynkrona steg.
- Await säger till JavaScript att vänta till promise är klart, hämta datan och fortsätt sedan till nästa rad.

( GÅ IGENOM DENNA FRÅGA IGEN OCH SE OM DET FINNS NÅGOT SÄTT ATT SKRIVA OM DETTA PÅ ETT BÄTTRE SÄTT )

### Fråga 8: Felhantering och säkerhet

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


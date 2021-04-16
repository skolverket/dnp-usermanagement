# dnp-usermanagement
Bibliotek med API-specifikationer och dokumentation om användarprovisionering för digitala nationella prov

## Viktig information
Detta gitHub-bibliotek omfattar resurser om det pågående arbetet runt utveckling av API för användarprovisionering kopplad till Skolverkets e-tjänst för digitala nationella prov. **N.b. materialet som finns här uppdateras kontinuerligt och är inte officiell information från Skolverket, utan ska betraktas som arbetsdokument**.

## Om Skolverkets API:er
### API för användarprovisionering
API:et används för att skapa, uppdatera och ta bort elever och personal hos de skolenheter som genomför digitala nationella prov och/eller använder Skolverkets digitala bedömningsstöd.

Format: API:et följer SS12000:2020 informationsmodell men har en omvänd riktning för dataöverföring. API:et tar emot och returnerar data som JSON enligt OpenAPI Specification (tidigare Swagger-format).

Villkor för användning: Autentisering görs med ömsesidig TLS via Skolfederation kontosynk.
För mer info se [Skolfederation.se](https://www.skolfederation.se/teknisk-information/kontosynk/)

### API för inhämtning av kontrollerade värden
Via detta api kan skolans system inhämta data om skolenheter, studievägar och andra värden som kontrolleras av Skolverket vid användarprovisionering. På så sätt kan data kontrolleras redan i förväg innan värden överförs till Skolverket. Syftet är att bidra till ökad datakvalitet och minska antalet fel.

Format: API:et tar emot och returnerar data som JSON enligt OpenAPI Specification (tidigare Swagger-format).

Villkor för användning: Publikt API

## Referenser
Information om digitalisering av de nationella proven finns på [Skolverkets webbplats](https://www.skolverket.se/om-oss/var-verksamhet/skolverkets-prioriterade-omraden/digitalisering/digitala-nationella-prov/digitalisering-av-de-nationella-proven)

SS12000 API specifikation finns på [https://github.com/girgen/ss12000v2](https://github.com/girgen/ss12000v2)

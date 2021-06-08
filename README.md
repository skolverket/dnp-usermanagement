>[Information på svenska](#dnp-anv%C3%A4ndarhantering)

# DNP User Management
The Swedish National Agency for Education implements digital national assessments (DNP). 

The library “DNP user management” includes API specifications and documentation describing how schools and school organisers manage user provisioning in the Swedish National Agency for Education’s digital national assessments service.

**Important notice: The work to implement a digital national assessments service is an ongoing project and the information material in DNP user management is continuously updated.** The material is not officially published as formal revisions and should be considered as working drafts.


## The Swedish National Agency for Education’s APIs for the digital national assessments service
### API for user provisioning (provisioning-api)
API for user provisioning is used for adding, updating, and deleting pupils and personnel from the digital national assessments service. 

The API is based on part of the data model in the standard SS12000:2020.

API for user provisionering (provisioning-api) require that the client is authenticated.

### API for fetching controlled values (data-api)
By using the API for fetching controlled values school systems can fetch data about school units, education programme, and other values controlled by Swedish National Agency for Education for user provisioning. In this way, data can be verified prior to transferring it to Swedish National Agency for Education. The purpose is to increase data quality and to minimize the number of errors.

The API also offer a feature to fetch an id (UUID) when creating references, such as between person and school unit.

The API for fetching controlled values (data-api) is an open-api.

### API for authentication (authentication-api)
API for authentication is used to authenticate a client that will use the API for user provisioning.

## References
Information about digitalisation of the national assessments is available at the [Swedish National Agency for Education’s web site (in Swedish)](https://www.skolverket.se/om-oss/var-verksamhet/skolverkets-prioriterade-omraden/digitalisering/digitala-nationella-prov/digitalisering-av-de-nationella-proven)

SS12000 API specifikation is available at [https://github.com/girgen/ss12000v2](https://github.com/girgen/ss12000v2)

## Contact
servicedesk@skolverket.se
___
>[Information in english](#dnp-user-management)

# DNP Användarhantering
Skolverket digitaliserar de nationella proven (DNP). 

I Skolverkets bibliotek DNP Användarhantering finns API-specifikationer och dokumentation som beskriver hur skola och huvudman kan hantera användarprovisionering i Skolverkets provtjänst för digitala nationella prov (DNP).

**Notera att arbetet med digitala nationella prov (DNP) är ett pågående projekt och att materialet i DNP användarhantering uppdateras kontinuerligt.** Det är inte officiell och fastställd information från Skolverket, utan ska betraktas som arbetsdokument.

## Skolverkets API:er för digitala nationella prov
### API för användarprovisionering (provisioning-api)
API för användarprovisionering används för att skapa, uppdatera och ta bort elever och personal i Skolverkets provtjänst för digitala nationella prov. 

API:et bygger på en delmängd ur datamodellen i standarden SS12000:2020.

API för användarprovisionering (provisioning-api) kräver att klienten är autentiserad.

### API för inhämtning av kontrollerade värden (data-api)
Genom API för inhämtning av kontrollerade värden kan skolans system inhämta data om skolenheter, studievägar och andra värden som kontrolleras av Skolverket vid användarprovisionering. På så sätt kan data kontrolleras i förväg innan värden överförs till Skolverket. Syftet är att bidra till ökad datakvalitet och minska antalet fel.

API:et erbjuder även en funktion för att hämta id (UUID) när man skapar referenser, exempelvis mellan person och skolenhet. 

API för inhämtning av kontrollerade värden (data-api) är ett öppet API.

### API för autentisering (authentication-api)
API för autentisering används för att autentisera en klient som ska använda API för användarprovisionering.

### API för inhämtning av kontrollerade värden
Via detta api kan skolans system inhämta data om skolenheter, studievägar och andra värden som kontrolleras av Skolverket vid användarprovisionering. På så sätt kan data kontrolleras redan i förväg innan värden överförs till Skolverket. Syftet är att bidra till ökad datakvalitet och minska antalet fel.

## Referenser
Information om digitalisering av de nationella proven finns på [Skolverkets webbplats](https://www.skolverket.se/om-oss/var-verksamhet/skolverkets-prioriterade-omraden/digitalisering/digitala-nationella-prov/digitalisering-av-de-nationella-proven)

SS12000 API specifikation finns på [https://github.com/girgen/ss12000v2](https://github.com/girgen/ss12000v2)

## Kontakt
servicedesk@skolverket.se

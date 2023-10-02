# DNP Användarhantering
Skolverket digitaliserar de nationella proven (DNP).

I Skolverkets bibliotek DNP Användarhantering finns API-specifikationer och dokumentation som
beskriver hur skolhuvudmän kan hantera användarprovisionering i Skolverkets provtjänst.

**Notera att arbetet med digitala nationella prov (DNP) är ett pågående projekt och att
materialet i DNP användarhantering uppdateras kontinuerligt.** Det är inte fastställd
information från Skolverket, utan dokumenten kommer att uppdateras.

## Skolverkets API:er för digitala nationella prov
Skolverket erbjuder tre alternativ för överföring av uppgifter till Skolverkets provtjänst.
* Provisionering via Provisionerings-API – data skickas till Skolverket.
* Provisionering enligt standarden SS 12000 – Skolverket hämtar data
* Provisionering via filinläsning

Läs mer om provisionerings alternativen på [Skolverkets hemsida](https://www.skolverket.se/om-oss/var-verksamhet/skolverkets-prioriterade-omraden/digitalisering/digitala-nationella-prov/tekniska-forutsattningar-for-att-genomfora-digitala-nationella-prov#skvtableofcontent6937).

Skolverket tillhandahåller följande komponenter som möjliggör överföring av uppgifter till
Skolverkets provtjänst.

### Open-data API
Genom Open-data API kan skolans system inhämta data som kontrolleras av Skolverket vid provisionering.
Information som kan hämtas från Open-data API inkluderar:
* Skolenhetsinformation med identifierare i UUID-format
* Studievägsinformation
* Aktuella ämnen och kurser där det genomförs nationella prov och bedömningsstöd.
>[Läs mer om open-data API](./open-data-api/README.md)

### Authentication API (Provtjänstens auktoriseringsserver)
Detta är ett API för autentisering och auktorisering som används för att autentisera klienter inför maskinell
överföring av uppgifter till Skolverkets provtjänst.
>[Läs mer om authentication-api](./authentication-api/README.md)

### Provisioning API
Genom detta API kan huvudman överföra uppgifter till Skolverkets provtjänst genom att skicka data från en klient.

API:et bygger på en delmängd ur datamodellen i standarden SS 12000.

Provisioning API kräver att klienten som skickar data har en auktoriserings-token
som utfärdats av Authentication API (auktoriseringsserver för provisionering till provtjänsten).
>[Läs mer om provisioning-api](./provisioning-api/README.md)

### SS 12000 Klient
För att möjliggöra överföring av uppgifter enligt standarden SS 12000 tillhandahåller Skolverkets provtjänst
en SS 12000 klient.
>[Läs mer om ss12000-client](./ss12000-client/README.md)

### Administration provtjänsten
För huvudman som inte kan överföra uppgifter maskinellt tillhandahåller Skolverkets provtjänst en e-tjänst som
möjliggör manuell överföring av uppgifter via filinläsning.
>[Läs mer om administration-provtjänsten](./administration-provtjansten/README.md)

## Övergripande instruktion för överföring av uppgifter
För en lyckad överföring av uppgifter till Skolverkets provtjänst behöver huvudmän följa nedanstående checklistor
som är grupperade utifrån vald metod för överföring av uppgifter.

**Notera att checklistorna är framtagna som stöd till IT-teknisk personal med ansvar för förberedelser inför överföring
av uppgifter till Skolverkets provtjänst.**

1. >[Checklista för huvudmän som överför uppgifter enligt standarden SS 12000](./checklists/checklista-för-provisionering-enlig-standarden-SS12000.md)
2. >[Checklista för huvudmän som överför uppgifter genom att skicka data till provtjänstens Provisioning API](./checklists/checklista-för-provisionering-via-Provisioning-API.md)
3. >[Checklista för huvudmän som överför uppgifter via filinläsning](./checklists/checklista-för-provisionering-via-filinläsning.md)

## Referenser
Information om digitalisering av de nationella proven finns på
[Skolverkets webbplats](https://www.skolverket.se/om-oss/var-verksamhet/skolverkets-prioriterade-omraden/digitalisering/digitala-nationella-prov/digitalisering-av-de-nationella-proven)

SS 12000 specifikation finns på
[https://www.sis.se/produkter/informationsteknik-kontorsutrustning/ittillampningar/ittillampningar-inom-utbildning/ss-120002020/](https://www.sis.se/produkter/informationsteknik-kontorsutrustning/ittillampningar/ittillampningar-inom-utbildning/ss-120002020/)

## Kontakt
https://www.skolverket.se/kontakt

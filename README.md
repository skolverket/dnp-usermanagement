# DNP Användarhantering
Skolverket digitaliserar de nationella proven (DNP). 

I Skolverkets bibliotek DNP Användarhantering finns API-specifikationer och dokumentation som
beskriver hur skolhuvudmän kan hantera användarprovisionering i Skolverkets provtjänst.

Arbetet med att införa digitala nationella prov (DNP) är ett pågående projekt. Materialet i DNP
användarhantering kan därför komma att justeras. Håll dig uppdaterad om den senaste informationen
här på Github och på [skolverket.se/dnp](https://www.skolverket.se/prov-och-bedomning/nationella-prov/digitala-nationella-prov).

# Viktig information om provtjänsten version 1.3
I provtjänsten version 1.3 som lanseras under hösten 2025 kan huvudmän lämna teknisk kontaktuppgifter
via e-tjänsten Administration provtjänsten. Skolverket kommer använda informationen för att kontakta
ansvariga vid tekniska frågor.

> [Läs mer om viktiga ändringar i provtjänsten 1.3](./CHANGELOG.md)

Det är också viktigt att kontrollera följande innan överföring av personuppgifter till Skolverkets provtjänst.
* Varje huvudman behöver utse och ge fullmakt till minst en person som hanterar skyddade personuppgifter.
  Huvudmän kan skapa fullmakt via [Mina ombud](https://minaombud.se/).
* Huvudmannens representant med behörighet att hantera skyddade personuppgifter behöver se till att det
  finns minst en aktuell e-postadress registrerad för mottagande av meddelanden om viktig information gällande skyddade
  personuppgifter. Detta görs i e-tjänsten [Administration provtjänsten](https://administrationprovtjansten.skolverket.se).
* Huvudmannens representant med behörighet att hantera skyddade personuppgifter behöver välja vilken
  pseudonymiseringsmetod som huvudmannen vill använda vid överföring av skyddade personuppgifter. Om
  ni vill undvika Skolverkets pseudonymisering är det viktigt att ni intygar er pseudonymiseringsmetod
  och följer instruktionerna. Detta görs i e-tjänsten [Administration provtjänsten](https://administrationprovtjansten.skolverket.se).

**Notera att det är viktigt att kontrollera ovanstående punkter även i verifieringstestmiljö innan genomförande
av tekniskt verifieringstest för överföring av uppgifter till Skolverkets provtjänst.**

> [Läs mer om hur skyddade personuppgifter hanteras i Skolverkets provtjänst](https://www.skolverket.se/prov-och-bedomning/provplattformen---tekniska-forberedelser/tekniska-krav-och-vagledning/overforing-av-uppgifter-till-skolverket/hantera-skyddade-personuppgifter-i-skolverkets-provtjanst)

## Skolverkets API:er för digitala nationella prov
Skolverket erbjuder tre metoder för överföring av uppgifter till Skolverkets provtjänst.
* Provisionering via Provisionerings-API – data skickas till Skolverket.
* Provisionering enligt standarden SS 12000 – Skolverket hämtar data
* Provisionering via filinläsning

> [Läs mer om metoderna för överföring av uppgifter](https://www.skolverket.se/prov-och-bedomning/provplattformen---tekniska-forberedelser/tekniska-krav-och-vagledning/overforing-av-uppgifter-till-skolverket)

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
en SS12000-klient.
>[Läs mer om ss12000-client](./ss12000-client/README.md)

### Administration provtjänsten
Huvudmän som inte kan överföra uppgifter maskinellt tillhandahåller Skolverkets provtjänst en e-tjänst som
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

## Miljöer för överföring av uppgifter
Skolverkets provtjänst tillhandahåller två miljöer för överföring av uppgifter.
>[Läs mer om miljöer](./environments.md)

## Referenser
Information om digitalisering av de nationella proven finns på
[Skolverkets webbplats](https://www.skolverket.se/prov-och-bedomning/provplattformen---tekniska-forberedelser/om-digitala-nationella-prov)

SS 12000 specifikation finns på
[https://www.sis.se/produkter/informationsteknik-kontorsutrustning/ittillampningar/ittillampningar-inom-utbildning/ss-120002020/](https://www.sis.se/produkter/informationsteknik-kontorsutrustning/ittillampningar/ittillampningar-inom-utbildning/ss-120002020/)

## Kontakt
https://www.skolverket.se/kontakt

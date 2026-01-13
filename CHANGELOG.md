# Changelog

Viktiga ändringar i systemet för överföring av uppgifter till provtjänsten publiceras här.

## Provtjänst v1.3 (2025-09-22)

### Nya funktioner
* Huvudmän kan nu lämna e-postadress för teknisk kontaktperson via e-tjänsten Administration
  provtjänsten. Skolverket kommer använda informationen för att kontakta ansvariga vid tekniska frågor.

### Ändringar
* Huvudmän får återkoppling på överförda uppgifter direkt efter uppgifterna lagrats hos
  Skolverket. Själva överföring vidare till provplattformen sker i en separat process som kan ta upp till
  30 minuter. Vid hög belastning kan det ta ännu längre tid.
* Det finns nu en begränsning på datamängd vid överföring av uppgifter via provisionerings-API.
  Data som skickas ska inte överstiga 1 MB per anrop, vilket motsvarar cirka 2000 elevers uppgifter.
* Pågående prov kommer inte längre hindra överföring av uppgifter. Uppgifter relaterat till en elev i
  pågående prov lagras hos Skolverket i samband med provisionering och sedan förs över till provplattformen
  på natten varje dag oavsett elevens provstatus.
* Det returneras inte längre felmeddelande när en personuppgift med påhittat personnummer
  (personnummer som är korrekt utformat inklusive kontrollsiffra men som inte finns i folkbokföringen)
  överförs till provtjänsten. Ansvariga för skyddade personuppgifter hos huvudmannen får information
  via Administration provtjänsten om att ändra personnumret inom 10 dagar. Om personnummer inte ändras
  inom 10 dagar kommer personuppgiften tas bort från provtjänsten.
* Attributet `provisioningStatus` har tagits bort från asynkrona svaret som returneras vid överföring
  av uppgifter via provisionerings-API. Rekommendationen är att personal på skolorna loggar in i
  provplattformen och kontrollerar att de överförda uppgifterna stämmer.

## Provtjänst v1.3.1 (2026-01-19)

### Nya funktioner och ändringar
* Nya Get-endpoints som används för att hämta redan överförda uppgifter finns nu implementerade i provisionerings-API.
  Se [OpenAPI-specifikationen för provisionerings-API](provisioning-api/dnp-provisioning-api.yaml) för detaljer. 
  **Huvudmän kan testa de nya funktionerna i verifieringstestmiljö från och med den 13 januari 2026.**
* Från och med den 19 januari 2026 införs begränsningar i antal anrop per sekund till samtliga
  endpoints i provisionerings-API. Begränsningarna varierar beroende på typ av endpoint:
  * Endpoints för överföring av uppgifter och uppföljning av status har en begränsning på
    _5 anrop per sekund per klient-IP_.
  * Endpoints för hämtning av uppgifter har en begränsning på _10 anrop per sekund per klient-IP_.
* Statuskoden `429 Too Many Requests` returneras när en klient överskrider tillåtet antal anrop.
  Det rekommenderas att klienter implementerar en backoff-strategi med återförsök för att hantera
  detta svar.
* Från och med den 19 januari 2026 sänks den maximala datamängden som kan överföras per anrop via
  provisionerings-API från 1 MB till 200 KB per anrop. Det motsvarar cirka
  400 elevers uppgifter. Klienter som överför uppgifter via provisionerings-API behöver därför
  anpassa sina implementationer för att stödja den nya gränsen.

# Changelog

Viktiga ändringar i systemet för överföring av uppgifter till provtjänsten publiceras här.

## Provtjänst v1.3 (2025-09-22)

### Nya funktioner
* Huvudmän kan nu lämna e-postadress för teknisk kontaktperson via e-tjänsten Administrera
  provtjänsten. Skolverket kommer använda informationen för att kontakta ansvariga vid tekniska frågor.

### Ändringar
* Huvudmän får återkoppling på överförda uppgifter direkt efter uppgifterna lagrats hos
  Skolverket. Själva överföring vidare till provplattformen sker i en separat process som kan ta upp till
  30 minuter. Vid hög belastning kan det ta ännu längre tid.
* Det finns nu en begränsning på datamängd vid överföring av uppgifter via provisionerings-API.
  Data som skickas ska inte överstiga 1 MB per anrop, vilket motsvarar ungefär 2000 elevers uppgifter.
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


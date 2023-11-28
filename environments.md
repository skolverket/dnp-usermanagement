# Provtjänstens miljöer för överföring av uppgifter
Skolverkets provtjänst tillhandahåller följande miljöer för överföring av uppgifter.
* **_Produktionsmiljö_**: Detta är skarpt miljö där endast riktiga uppgifter ska överföras. 
* **_Verifieringstest miljö_**: Detta är en miljö där huvudmännen kan testa överföring av uppgifter
  till Skolverkets provtjänst. 

## Verifieringstest miljö

Tabellen nedan innehåller URL:er till de komponenter som kan användas för att genomföra
vertifieringstest relaterat till överföring av uppgifter till Skolverkets provtjänst.

| Komponent                         | **URL till API eller e-tjänst**                                                       | **Kommentar**                                                                                                                                                                                                    |
|-----------------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| _**Mina ombud**_                  | https://minaombud.se                                                                  | Skapa och hantera fullmakt. Fullmakterna kommer att användas för behörighetskontroll i e-tjänsten Administration provtjänsten.                                                                                   |
| _**Administration provtjänsten**_ | https://administrationprovtjansten-verifieringstest.skolverket.se                     | Överföring av uppgifter via fil och registrering av huvudmans SS12000-API URL.                                                                                                                                   |
| _**Auktoriseringsserver**_        | https://nutid-auth-test.sunet.se/transaction                                          | Autentisering och auktorisering.                                                                                                                                                                                 |
| _**Open-data API**_               | https://apigw-pre.skolverket.se/provtjanst/verifieringstest/open-data/v1              | Provtjänstens öppna-data. Se [Open-data API swagger-GUI](https://apigw-pre.skolverket.se/provtjanst/verifieringstest/open-data/v1/swagger-ui.html).                                                              |
| _**Provisioning API**_            | https://apigw-pre.skolverket.se/provtjanst/verifieringstest/provisioning/v1           | Överföring av uppgifter via Skolverkets provisionerings-API. Se [Provisioning API swagger-GUI](https://apigw-pre.skolverket.se/provtjanst/verifieringstest/provisioning/v1/swagger-ui.html).                     |
| _**SS12000 klient**_              | https://apigw-pre.skolverket.se/provtjanst/verifieringstest/ss12000/klient/v1/{urlId} | Överföring av uppgifter enligt standarden SS 12000. `urlId` är unik identifierare för huvudmans SS12000-API URL. Vid prenumeration registrerar SS12000-klienten en specifik "target-URL" som innehåller `urlId`. |

## Kontakt
https://www.skolverket.se/kontakt

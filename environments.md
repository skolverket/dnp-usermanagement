# Provtjänstens miljöer för överföring av uppgifter

Skolverkets provtjänst tillhandahåller följande miljöer för överföring av uppgifter.

* **_Produktionsmiljö_**: Detta är en skarp miljö där endast verkliga uppgifter ska överföras.
* **_Verifieringstestmiljö_**: Detta är en miljö där huvudmän kan testa överföring av uppgifter
  till Skolverkets provtjänst.

## Produktionsmiljö

Tabellen nedan innehåller URL:er till de komponenter som ska användas för att överföra verkliga
uppgifter
till Skolverkets provtjänst.

| Komponent                         | **URL till API eller e-tjänst**                                | **Kommentar**                                                                                                                                                                                                                                                                              |
|-----------------------------------|----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| _**Mina ombud**_                  | https://minaombud.se                                           | Används för att skapa och hantera fullmakt. Fullmakterna kommer att användas för behörighetskontroll i e-tjänsten Administration provtjänsten.                                                                                                                                             |
| _**Administration provtjänsten**_ | https://administrationprovtjansten.skolverket.se               | Används för att överföra uppgifter via fil, registrera huvudmans SS12000-API URL, beställa IdP och beställa stöd för e-legitimation.                                                                                                                                                       |
| _**Auktoriseringsserver**_        | https://nutid-auth.sunet.se/transaction                        | Används för att hämta JWT (access-token) efter autentisering enligt mTLS.                                                                                                                                                                                                                  |
| _**Open-data API**_               | https://api.skolverket.se/provtjanst/open-data/v1              | Provtjänstens öppna-data. Se [Open-data API swagger-GUI](https://api.skolverket.se/provtjanst/open-data/v1/swagger-ui.html).                                                                                                                                                               |
| _**Provisioning API**_            | https://api.skolverket.se/provtjanst/provisioning/v1           | Används för att överföra uppgifter genom att skicka data till Skolverkets API. Se [Provisioning API swagger-GUI](https://api.skolverket.se/provtjanst/provisioning/v1/swagger-ui.html).                                                                                                    |
| _**SS12000 klient**_              | https://api.skolverket.se/provtjanst/ss12000/klient/v1/{urlId} | Används för att överföra uppgifter enligt standarden SS 12000 där Skolverket hämtar data från huvudmannens API. Notera att `urlId` är en unik identifierare för huvudmans SS12000-API URL. Vid prenumeration registrerar SS12000-klienten en specifik "target-URL" som innehåller `urlId`. |

## Verifieringstestmiljö

Tabellen nedan innehåller URL:er till de komponenter som ska användas för att tekniskt verifiera
överföring av uppgifter till Skolverkets provtjänst. Uppgifter kan vara verkliga eller fiktiva.

**Notera att från och med den 15 mars 2024 accepteras endast produktionscertifikat i
verifieringstestmiljö.
Detta innebär att JWT (access-token) behöver hämtas från auktoriseringsservern som finns i
produktionsmiljö (https://nutid-auth.sunet.se/transaction) för att kunna överföra uppgifter till
verifieringstestmiljö.**

| Komponent                         | **URL till API eller e-tjänst**                                                       | **Kommentar**                                                                                                                                                                                                                                                                                         |
|-----------------------------------|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| _**Mina ombud**_                  | https://minaombud.se                                                                  | Används för att skapa och hantera fullmakt. Fullmakterna kommer att användas för behörighetskontroll i e-tjänsten Administration provtjänsten.                                                                                                                                                        |
| _**Administration provtjänsten**_ | https://administrationprovtjansten-verifieringstest.skolverket.se                     | Används för att testa överföring av uppgifter via fil och registrering av huvudmans SS12000-API URL.                                                                                                                                                                                                  |
| _**Auktoriseringsserver**_        | https://nutid-auth-test.sunet.se/transaction                                          | Används för att hämta JWT (access-token) efter autentisering enligt mTLS. Notera att från och med den 15 mars 2024 ska det användas auktoriseringsservern i produktionsmiljö (https://nutid-auth.sunet.se/transaction).                                                                               |
| _**Open-data API**_               | https://apigw-pre.skolverket.se/provtjanst/verifieringstest/open-data/v1              | Provtjänstens öppna-data. Se [Open-data API swagger-GUI](https://apigw-pre.skolverket.se/provtjanst/verifieringstest/open-data/v1/swagger-ui.html).                                                                                                                                                   |
| _**Provisioning API**_            | https://apigw-pre.skolverket.se/provtjanst/verifieringstest/provisioning/v1           | Används för att testa överföring av uppgifter genom att skicka data till Skolverkets API. Se [Provisioning API swagger-GUI](https://apigw-pre.skolverket.se/provtjanst/verifieringstest/provisioning/v1/swagger-ui.html).                                                                             |
| _**SS12000 klient**_              | https://apigw-pre.skolverket.se/provtjanst/verifieringstest/ss12000/klient/v1/{urlId} | Används för att testa överföring av uppgifter enligt standarden SS 12000 där Skolverket hämtar data från huvudmannens API. Notera att `urlId` är en unik identifierare för huvudmans SS12000-API URL. Vid prenumeration registrerar SS12000-klienten en specifik "target-URL" som innehåller `urlId`. |

## Kontakt

https://www.skolverket.se/kontakt

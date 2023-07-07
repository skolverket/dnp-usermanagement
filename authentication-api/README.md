# API för autentisering
API för autentisering används för att autentisera klienter inför maskinell överföring av uppgifter till
Skolverkets provtjänst. API:et kallas också för auktoriseringsserver eftersom det utfärdar auktoriseringstoken
till klienter efter autentisering.

**Notera att arbetet med digitala nationella prov är ett pågående projekt och att informationsinnehållet i
detta dokument uppdateras kontinuerligt.** Informationen är med andra ord inte officiell och fastställd
av Skolverket, utan ska betraktas som ett innehåll i ett arbetsdokument.

## Förutsättningar för autentisering och auktorisering
För att kunna överföra uppgifter till Skolverkets provtjänst behöver du som huvudman uppfylla ett av följande
huvudkrav:

1. Ansluta till en TLS-federation som är ansluten till [Fidus](https://www.skolverket.se/om-oss/var-verksamhet/skolverkets-prioriterade-omraden/digitalisering/digitala-nationella-prov/tekniska-forutsattningar-for-digitala-nationella-prov/fidus)
   samt se till att huvudmannens klientcertifikat är registrerat i ett metadataregister som TLS-federationen
   tillhandahåller.
   > [Läs mer om TLS-federation](https://www.ietf.org/archive/id/draft-halen-fed-tls-auth-00.html).
2. Skaffa ett klientcertifikat från en certifikatutfärdare som är godkänd av Fidus.

Utöver huvudkraven ovan gäller följande specifika krav:

* Klientcertifikatet används för att identifiera huvudmannen. Därför ska varje huvudman ha minst ett
  klientcertifikat. Leverantörer som hanterar överföring av uppgifter för flera huvudmän behöver tillhandahålla
  minst ett klientcertifikat per huvudman.
* Klientcertifikatet ska ha en koppling till huvudmannens organisationsnummer.
    * En huvudman som är ansluten till en TLS-federation ska registrera både klientcertifikat och
      organisationsnummer i sitt metadata.
    * En huvudman som har ett klientcertifikat från en certifikatutfärdare ska se till att sitt
      organisationsnummer finns i certifikatet.
* Följande alternativ finns för huvudmän som hanterar överföring av uppgifter med flera klienter eller flera
  SS 12000-API:er:
    * Tillhandahålla ett klientcertifikat per klient eller SS 12000-API. Notera att Skolverkets provtjänst
      behandlar klientcertifikaten som tillhör en huvudman lika, det vill säga att varje certifikatinnehavare
      har full åtkomst till huvudmannens data i provtjänsten.
    * Använda ett klientcertifikat för alla sina klienter eller SS 12000-API:er.

**I dagsläget är provtjänstens auktoriseringsserver konfigurerad med en TLS-federation. Skolverket arbetar med
att integrera andra TLS-federationer samt vissa certifikatutfärdare till auktoriseringsservern.**

## Autentisering utifrån vald metod för överföring av uppgifter
Inför maskinell överföring av uppgifter till Skolverkets provtjänst krävs det att klienter autentiserar
sig mot provtjänstens auktoriseringsserver enligt principen mTLS ("mutual Transport Layer Security") och hämtar
JWT ("JSON Web Token").

"Trust store" för provtjänstens auktoriseringsserver förbereds genom att:
1. Hämta och aggregera metadata från olika TLS-federationer som är anslutna till FIDUS.
2. Hämta och konfigurera rotcertifikaten från olika certifikatutfärdare som är godkända av FIDUS.
3. Hämta och konfigurera certifikatet från provtjänstens SS 12000-klient.

![Auktoriseringsserver trust store](authentication-api-trust-store.png)

Nedan beskrivs specifika flöden för autentisering utifrån vald metod för överföring av uppgifter.

### Autentiseringsflöde för huvudmän som överför uppgifter via Provisionerings-API - data skickas till Skolverket
![Autentiseringflöde för Provisionings-API](authentication-flow-provisioning-api.png)
1. Huvudmannens klient autentiserar sig mot provtjänstens auktoriseringsserver genom att presentera
   sitt klientcertifikat.
2. Auktoriseringsservern autentiserar klientcertifikatet mot etablerad "trust store". Huvudmannens klient
   kan också kontrollera servercertifikatet av auktoriseringsservern.
3. Auktoriseringsservern utfärdar JWT till klienten.
4. Huvudmannens klient presenterar JWT vid överföring av uppgifter till provtjänstens Provisioning-API.

### Autentiseringsflöde för huvudmän som överför uppgifter enligt standarden SS 12000 – Skolverket hämtar data
Denna metod för överföring av uppgifter består av två huvudsteg utifrån vilken part som initierar
kommunikationen. Vid förändringar av uppgifter skickar huvudmannen notifiering, som också kallas "webhook", till
Skolverkets provtjänst. Utifrån informationen som finns i notifieringen hämtar Skolverkets provtjänst förändringar
från huvudmannens SS 12000-API. Autentiseringsflöden för dessa två steg beskrivs nedan.

#### **_Autentiseringsflöde vid notifiering av förändringar_**
![Autentiseringflöde för SS 12000 "webhook" notifiering](authentication-flow-ss12000-webhook-notification.png)
1. Huvudmannens notifieringsklient autentiserar sig mot provtjänstens auktoriseringsserver genom att presentera
   sitt klientcertifikat.
2. Auktoriseringsservern autentiserar klientcertifikatet mot etablerad "trust store". Notifieringsklienten kan också
   kontrollera servercertifikatet av auktoriseringsservern.
3. Auktoriseringsservern utfärdar JWT till notifieringsklienten.
4. Notifieringsklienten presenterar JWT vid notifiering av förändringar till provtjänstens SS 12000-klient.

#### **_Autentiseringsflöde vid datainhämtning_**
![Autentiseringflöde för SS 12000 datainhämtning](authentication-flow-ss12000-data-fetching.png)
1. Provtjänstens SS 12000-klient autentiserar sig mot provtjänstens auktoriseringsserver genom att presentera
   sitt klientcertifikat.
2. Auktoriseringsservern autentiserar klientcertifikatet mot etablerad "trust store". Provtjänstens SS 12000-klient
   kan också kontrollera servercertifikatet av auktoriseringsservern.
3. Auktoriseringsservern utfärdar JWT till klienten.
4. Skolverkets SS 12000-klient presenterar JWT till huvudmannens SS 12000-API vid hämtning av förändrade uppgifter.

## Http-anrop mot auktoriseringsserver
Nedan finns ett exempelanrop för hur en klient kan hämta JWT från provtjänstens auktoriseringsserver.

````shell script
$curl --cert login-test.crt --key login-test-private.key \
     --pinnedpubkey sha256//gWH4cyRuOw9GnGoQNR5NazM3gt36IkK6fszCvqkSll0=  \
     -X POST -H "Content-Type: application/json" -d @body.json \
     https://nutid-auth-test.sunet.se/transaction
````
Beskrivning av ovanstående `Curl` kommand:

* `curl` är ett kommandoradsverktyg för dataöverföring.
* `--cert login-test.crt` pekar på var klientens publika nyckel finns i filsystemet.
* `--key login-test-private.key` pekar på var klientens privata nyckel finns i filsystemet.
* `--pinnedpubkey sha256//gWH4cyRuOw9GnGoQNR5NazM3gt36IkK6fszCvqkSll0=` används för att autentisera server certifikatet av auktoriseringsservern.
* `body.json` är JSON-data som skickas till auktoriseringsservern i anropet. Nedan finns ett exempel JSON-data.
  ````json
    {
        "access_token": [
            {
                "flags": [
                    "bearer"
                ]
            }
        ],
        "client": {
            "key": "https://login-test.skolverket.se"
        }
    }
  ````
  Notera att `client.key` är identifierare (entityId) för specifika metadata för klienten.
* `https://nutid-auth-test.sunet.se/transaction` är URL till auktoriseringsservern i testmiljö.

Efter autentisering av klienten utfärdar auktoriseringsservern ett JWT som returneras i svaret. JWT
hittas i attributet `access_token.value`.

````json
{
  "access_token": {
    "value": "eyJhbGciOiJFUzI1NiJ9.eyJhdW...3jcM3lRV73iojs-CNPg",
    "access": [],
    "expires_in": 864000,
    "flags": [
      "bearer"
    ]
  }
}
````

## Kontakt
https://www.skolverket.se/kontakt

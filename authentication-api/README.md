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

Se [JWT för åtkomst till Provisioning API](#jwt-för-åtkomst-till-provisioning-api).

### Autentiseringsflöde för huvudmän som överför uppgifter enligt standarden SS 12000 – Skolverket hämtar data
Denna metod för överföring av uppgifter består av två huvudsteg utifrån vilken part som initierar
kommunikationen. Vid förändringar av uppgifter skickar huvudmannen notifiering, som också kallas "webhook", till
Skolverkets provtjänst. Utifrån informationen som finns i notifieringen hämtar Skolverkets provtjänst förändringar
från huvudmannens SS 12000-API. Autentiseringsflöden för dessa två steg beskrivs nedan.

#### _Autentiseringsflöde vid notifiering av ändrade uppgifter_
![Autentiseringflöde för SS 12000 "webhook" notifiering](authentication-flow-ss12000-webhook-notification.png)
1. Huvudmannens notifieringsklient autentiserar sig mot provtjänstens auktoriseringsserver genom att presentera
   sitt klientcertifikat.
2. Auktoriseringsservern autentiserar klientcertifikatet mot etablerad "trust store". Notifieringsklienten kan också
   kontrollera servercertifikatet av auktoriseringsservern.
3. Auktoriseringsservern utfärdar JWT till notifieringsklienten.
4. Notifieringsklienten presenterar JWT vid notifiering av förändringar till provtjänstens SS 12000-klient.

Se [JWT för åtkomst till notifieringsändpunkt av SS12000-klienten](#jwt-för-åtkomst-till-notifierings-ändpunkten-av-provtjänstens-ss12000-klient).

#### _Autentiseringsflöde vid datainhämtning_
![Autentiseringflöde för SS 12000 datainhämtning](authentication-flow-ss12000-data-fetching.png)
1. Provtjänstens SS 12000-klient autentiserar sig mot provtjänstens auktoriseringsserver genom att presentera
   sitt klientcertifikat.
2. Auktoriseringsservern autentiserar klientcertifikatet mot etablerad "trust store". Provtjänstens SS 12000-klient
   kan också kontrollera servercertifikatet av auktoriseringsservern.
3. Auktoriseringsservern utfärdar JWT till klienten.
4. Skolverkets SS 12000-klient presenterar JWT till huvudmannens SS 12000-API vid hämtning av förändrade uppgifter.

Se [JWT för åtkomst till SS12000-API hos huvudman](#jwt-för-åtkomst-till-huvudmannens-ss12000-api).

## HTTP-anrop mot auktoriseringsserver
Nedan finns 3 exempelanrop för hur JWT kan hämtas från provtjänstens auktoriseringsserver.

### JWT för åtkomst till Provisioning API
````shell script
$curl --cert login-test.crt --key login-test-private.key \
     --pinnedpubkey sha256//A92nvLZeX4wdmZJnLvG0iddOyke/j9X0/itqoKTFKXo=  \
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
            "access": [
                {
                    "type": "provisioning-api",
                    "locations": [
                        "https://api-pre.skolverket.se/dnp/iga/provisioning/v1"
                    ]
                }
            ],
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
  Notera att `client.key` är identifierare (entityId) för specifika metadata för klienten samt att bas-URL till
  provtjänstens Provisioning-API behöver anges under `locations`.
* `https://nutid-auth-test.sunet.se/transaction` är URL till auktoriseringsservern i testmiljö.

Efter autentisering av klienten utfärdar auktoriseringsservern ett JWT som returneras i svaret. JWT
hittas i attributet `access_token.value`.

````json
{
  "access_token": {
    "value": "eyJhbGciOiJ...17A2kqYewNw",
    "access": [
      {
        "type": "provisioning-api",
        "locations": [
          "https://api-pre.skolverket.se/dnp/iga/provisioning/v1"
        ]
      }
    ],
    "expires_in": 864000,
    "flags": [
      "bearer"
    ]
  }
}
````

### JWT för åtkomst till notifierings ändpunkten av Provtjänstens SS12000-klient
Exempel anrop:
````shell script
$curl --cert login-test.crt --key login-test-private.key \
     --pinnedpubkey sha256//A92nvLZeX4wdmZJnLvG0iddOyke/j9X0/itqoKTFKXo=  \
     -X POST -H "Content-Type: application/json" -d @body.json \
     https://nutid-auth-test.sunet.se/transaction
````
`body.json` är JSON-data som skickas till auktoriseringsservern i anropet. Nedan finns ett exempel JSON-data.
````json
{
  "access_token": [
    {
      "access": [
        {
          "type": "ss12000-client",
          "locations": [
            "https://apigw-pre.skolverket.se/provtjanst/ss12000/klient/v1"
          ]
        }
      ],
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
Exempel svar:
````json
{
  "access_token": {
    "value": "eyJhbGciOiJFU...H_ElVcCY1VCZw",
    "access": [
      {
        "type": "ss12000-client",
        "locations": [
          "https://apigw-pre.skolverket.se/provtjanst/ss12000/klient/v1"
        ]
      }
    ],
    "expires_in": 864000,
    "flags": [
      "bearer"
    ]
  }
}
````

### JWT för åtkomst till huvudmannens SS12000-API
Exempel anrop som skickas från provtjänstens SS12000-klient till auktoriseringsserver för åtkomst till en huvudmans
SS12000-API:
````shell script
$curl --cert login-test.crt --key login-test-private.key \
     --pinnedpubkey sha256//A92nvLZeX4wdmZJnLvG0iddOyke/j9X0/itqoKTFKXo=  \
     -X POST -H "Content-Type: application/json" -d @body.json \
     https://nutid-auth-test.sunet.se/transaction
````
`body.json` är JSON-data som skickas till auktoriseringsservern i anropet. Nedan finns ett exempel JSON-data.
````json
{
  "access_token": [
    {
      "access": [
        {
          "type": "ss12000-api",
          "locations": [
            "https://testhuvudman.se/ss12000-api/v2.0"
          ]
        }
      ],
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
Notera att huvudmannens SS12000-API url anges under `locations`.

Exempel svar:
````json
{
  "access_token": {
    "value": "eyJhbGciOi...EQoc11YIoTA",
    "access": [
      {
        "type": "ss12000-api",
        "locations": [
          "https://testhuvudman.se/ss12000-api/v2.0"
        ]
      }
    ],
    "expires_in": 864000,
    "flags": [
      "bearer"
    ]
  }
}
````

### Verifiering av JWT som skickas från Provtjänstens SS12000-Klient

JWT som skickas från provtjänstens SS12000-klient till huvudmannens SS12000-API ser ut som följande. 
````json
{
  "aud": "nutid test",
  "entity_id": "https://login-test.skolverket.se",
  "exp": 1696545515,
  "iat": 1695681515,
  "iss": "https://nutid-auth-test.sunet.se",
  "nbf": 1695681515,
  "organization_id": "SE2021004185",
  "requested_access": [
    {
      "locations": [
        "https://testhuvudman.se/ss12000-api/v2.0"
      ],
      "type": "ss12000-api"
    }
  ],
  "source": "https://xxxxx.se",
  "version": 1
}
````
Huvudmän ska göra följande kontroll på en JWT som skickas från SS12000-klienten. 

1. Verifiera att JWT har utfärdats av rätt auktoriseringsservern med hjälp av nyckeln ("JSON Web Key Set - JWKS") som
   finns i denna URL: https://nutid-auth-test.sunet.se/.well-known/jwks.json. Nedan finns ett exempel på nyckel.
    ````json
    {
      "keys": [
        {
          "kty": "EC",
          "kid": "default",
          "crv": "P-256",
          "x": "xyuSyzrasTzUeKcU3GlzoZ8-uWOJtMY-R8vANsBn20c",
          "y": "xVCe-G7WFkZexRgk5JjUNAUXXnZPXk2zhDEhg3Nopnc"
        }
      ]
    }
    ````
2. Kontrollera att `organization_id` i JWT är Skolverkets organisationsnummer (SE2021004185).
3. Kontrollera att JWT är utfärdad för att endast komma åt huvudmannens SS12000-API specificerad under `requested_access`.
    `````json
    {
      "locations": [
        "https://testhuvudman.se/ss12000-api/v2.0"
      ],
      "type": "ss12000-api"
    }
    `````
4. Det är också viktigt att kontrollera JWT:s giltighet med hjälp av informationen under `nbf` och `exp`.

## Kontakt
https://www.skolverket.se/kontakt

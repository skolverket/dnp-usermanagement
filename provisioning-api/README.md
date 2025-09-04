# API för användarprovisionering (provisioning-api)
API för användarprovisionering används för att skapa, uppdatera och ta bort elever och personal i Skolverkets provtjänst för digitala nationella prov. 

API:et bygger på en delmängd ur datamodellen i standarden SS12000:2020.

API för användarprovisionering (provisioning-api) kräver att klienten är autentiserad.

## Autentiseringsflöde
Följande sekvensdiagram visar hur en klient hos huvudman autentiserar sig mot provtjänstens
auktoriseringsserver och därefter överför uppgifter med att skicka data via Provisioning-API.

`````mermaid
sequenceDiagram
    title   Autentiseringsflöde
    autonumber
    participant Auktoriseringsserver
    participant Huvudmannens-Klient
    participant Provisioning-API
    Huvudmannens-Klient->>Auktoriseringsserver: Hämta JWT med klientcertifikat
    Auktoriseringsserver-->>Huvudmannens-Klient: JWT
    Huvudmannens-Klient->>Provisioning-API: Överföra uppgifter med JWT
`````

## Ordningsföljd för överföring av uppgifter via Provisioning API
Följande sekvensdiagram visar i vilken ordningsföljd som uppgifter ska överföras via Provisioning API.

````mermaid
sequenceDiagram
    title   Ordningsföljd för överföring av uppgifter via Provisioning API
    autonumber
    participant Huvudmannens-Klient
    participant Provisioning-API
    Huvudmannens-Klient->>Provisioning-API: Skicka flera 'Person' objekt
    Provisioning-API-->>Huvudmannens-Klient: 'jobId' returneras
    Huvudmannens-Klient->>Provisioning-API: Kontrollera status med 'jobId'
    Provisioning-API-->>Huvudmannens-Klient: Statusar för varje 'Person' objekt
    Huvudmannens-Klient->>Huvudmannens-Klient: Spara statusar för varje 'Person' objekt
    
    Note over Huvudmannens-Klient: Kontrollera att 'Person' objekt<br>är inskickad och fått lyckad status<br>innan 'Duty' skickas
    Huvudmannens-Klient->>Provisioning-API: Skicka flera 'Duty' objekt
    Provisioning-API-->>Huvudmannens-Klient: 'jobId' returneras
    Huvudmannens-Klient->>Provisioning-API: Kontrollera status med 'jobId'
    Provisioning-API-->>Huvudmannens-Klient: Statusar för varje 'Duty' objekt
    Huvudmannens-Klient->>Huvudmannens-Klient: Spara statusar för varje 'Duty' objekt
    
    Note over Huvudmannens-Klient: Kontrollera att gruppmedlemmars 'Person' objekt<br>är inskickad och fått lyckad status<br>innan 'Group' skickas
    Huvudmannens-Klient->>Provisioning-API: Skicka flera 'Group' objekt
    Provisioning-API-->>Huvudmannens-Klient: 'jobId' returneras
    Huvudmannens-Klient->>Provisioning-API: Kontrollera status med 'jobId'
    Provisioning-API-->>Huvudmannens-Klient: Statusar för varje 'Group' objekt
    Huvudmannens-Klient->>Huvudmannens-Klient: Spara statusar för varje 'Duty' objekt

    Note over Huvudmannens-Klient: Kontrollera att relevanta 'Group' och 'Duty' objekt<br>är inskickad och fått lyckad status<br>innan 'Activity' skickas
    Huvudmannens-Klient->>Provisioning-API: Skicka flera 'Activity' objekt
    Provisioning-API-->>Huvudmannens-Klient: 'jobId' returneras
    Huvudmannens-Klient->>Provisioning-API: Kontrollera status med 'jobId'
    Provisioning-API-->>Huvudmannens-Klient: Statusar för varje 'Activity' objekt
    Huvudmannens-Klient->>Huvudmannens-Klient: Spara statusar för varje 'Activity' objekt
````


## Referenser
Information om digitalisering av de nationella proven finns på
[Skolverkets webbplats](https://www.skolverket.se/prov-och-bedomning/provplattformen---tekniska-forberedelser/om-digitala-nationella-prov)

SS 12000 specifikation finns på
[https://www.sis.se/produkter/informationsteknik-kontorsutrustning/ittillampningar/ittillampningar-inom-utbildning/ss-120002020/](https://www.sis.se/produkter/informationsteknik-kontorsutrustning/ittillampningar/ittillampningar-inom-utbildning/ss-120002020/)

## Kontakt
https://www.skolverket.se/kontakt

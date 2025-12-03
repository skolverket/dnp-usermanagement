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
    box Vetenskapsrådet
    participant AS as Auktoriseringsserver
    end
    box Huvudman      
    participant Klient as Provisionerings-Klient
    end
    box Skolverkets<br>provisioneringstjänst
    participant API as Provisioning-API
    end
    Klient->>AS: Hämta JWT med klientcertifikat
    AS-->>Klient: JWT (JSON Web Token)
    Klient->>API: Överföra uppgifter med JWT
`````

## Ordningsföljd för överföring av uppgifter via Provisioning API
Följande sekvensdiagram visar i vilken ordningsföljd som uppgifter ska överföras via Provisioning API.

````mermaid
sequenceDiagram
    title   Överföring av uppgifter via Provisioning API
    autonumber
    box Huvudman
    participant Klient as Provisionerings-klient
    end
    box Skolverkets<br>provisioneringstjänst
    participant API as Provisioning API
    end
    box Provplattformen
    participant PP_API as Provplattformens-API
    end
    Klient->>API: Skicka flera 'Person' objekt
    API-->>Klient: 'jobId' returneras
    Klient->>API: Kontrollera status med 'jobId'
    API-->>Klient: Statusar för varje 'Person' objekt
    Klient->>Klient: Spara statusar för varje 'Person' objekt<br>inklusive eventuella felmeddelanden
    opt Har 'Person' objekt inskrivning?
    API->>PP_API: Skapa/uppdatera elevkonto
    end
    
    Note over Klient: Kontrollera att 'Person' objekt<br>är inskickad och fått lyckad status<br>innan 'Duty' skickas
    Klient->>API: Skicka flera 'Duty' objekt
    API-->>Klient: 'jobId' returneras
    Klient->>API: Kontrollera status med 'jobId'
    API-->>Klient: Statusar för varje 'Duty' objekt
    Klient->>Klient: Spara statusar för varje 'Duty' objekt<br>inklusive eventuella felmeddelanden
    API->>PP_API: Skapa/uppdatera personalskonto
    
    Note over Klient: Kontrollera att gruppmedlemmars 'Person' objekt<br>är inskickad och fått lyckad status<br>innan 'Group' skickas
    Klient->>API: Skicka flera 'Group' objekt
    API-->>Klient: 'jobId' returneras
    Klient->>API: Kontrollera status med 'jobId'
    API-->>Klient: Statusar för varje 'Group' objekt
    Klient->>Klient: Spara statusar för varje 'Group' objekt<br>inklusive eventuella felmeddelanden
    opt Är 'Group' av typ 'Klass'?
    API->>PP_API: Uppdatera elevkonton med klassinformation
    end

    Note over Klient: Kontrollera att relevanta 'Group' och 'Duty' objekt<br>är inskickad och fått lyckad status<br>innan 'Activity' skickas
    Klient->>API: Skicka flera 'Activity' objekt
    API-->>Klient: 'jobId' returneras
    Klient->>API: Kontrollera status med 'jobId'
    API-->>Klient: Statusar för varje 'Activity' objekt
    Klient->>Klient: Spara statusar för varje 'Activity' objekt<br>inklusive eventuella felmeddelanden
    API->>PP_API: Skapa/uppdatera undervisningsgrupper och koppla elever och personal
````


## Referenser
Information om digitalisering av de nationella proven finns på
[Skolverkets webbplats](https://www.skolverket.se/prov-och-bedomning/provplattformen---tekniska-forberedelser/om-digitala-nationella-prov)

SS 12000 specifikation finns på
[https://www.sis.se/produkter/informationsteknik-kontorsutrustning/ittillampningar/ittillampningar-inom-utbildning/ss-120002020/](https://www.sis.se/produkter/informationsteknik-kontorsutrustning/ittillampningar/ittillampningar-inom-utbildning/ss-120002020/)

## Kontakt
https://www.skolverket.se/kontakt

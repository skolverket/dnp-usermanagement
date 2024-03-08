## Checklista for en huvudman som överför uppgifter enligt standarden SS 12000

1. Samla in och förbereda uppgifter som behöver överföras till Skolverkets provtjänst.
   >[Se attributlista för provisionering](https://www.skolverket.se/skolutveckling/digitala-nationella-prov/overforing-av-uppgifter-till-skolverket#h-UppgiftersombehoveroverforastillSkolverket).
2. Hämta provtjänstens öppna data från _**open-data API**_.<br />
   Provaktiviteter som motsvarar ämnen eller kurser där det genomförs nationella prov och bedömningsstöd ska
   hämtas från open-data API och konfigureras i huvudmannens system. Notera att undervisningsgrupper som genomför
   ett nationellt prov eller bedömningsstöd behöver kopplas till en provaktivitet.<br /><br />
   Det är också rekommenderat att hämta studievägs- och skolenhetsinformation från open-data API för att sedan
   kontrollera uppgifter innan överföring till Skolverkets provtjänst.<br /><br />
   Notera att det är Skolverkets identifierare (UUID) för provaktiviteter som ska användas vid
   överföring av uppgifter.
   >[Läs mer om open-data API](../open-data-api/README.md)
   
3. Tillgängliggöra uppgifter via SS12000-API.<br />
   Skolverket har tagit fram en referensimplementation för standarden
   SS12000. Referens API:et finns tillgängligt som öppen källkod på Skolverket Github.
   >[Läs mer om provtjänstens SS12000 reference API](https://github.com/skolverket/dnp-ss12000-reference-api)

   >**Notera att det är endast de uppgifter som huvudmannen tillgängliggör via API som Skolverket hämtar. Därför
   är det rekommenderat att etablera en profil för Skolverkets SS12000-klient med en definition av vilka uppgifter
   som kan hämtas av klienten samt implementera teknik för att säkerställa att endast definierade uppgifter kan
   hämtas av klienten**
4. Skaffa klientcertifikat och etablera integration mot provtjänstens auktoriseringsserver.
   >[Läs mer om provtjänstens auktoriseringsserver](../authentication-api/README.md)
   
5. Skaffa fullmakt i _**Mina ombud**_ för de personer som administrerar provisionering för huvudmannens räkning i
   Skolverkets e-tjänst _**Administration provtjänsten**_. 
   >[URL till Mina ombud](https://minaombud.se/)

   >[Läs mer om Administration provtjänsten](https://www.skolverket.se/skolverkets-e-tjanst-administration-provtjansten)

6. Registrera URL till huvudmannens SS12000-API via Skolverkets e-tjänst Administration provtjänsten. Notera att en
   huvudman kan registrera flera SS12000-API URL:er. Registrering av SS12000-API URL ska ske enligt nedanstående flöde:
   1. Huvudmannens ombud loggar in i Administration provtjänsten och anger SS12000-API URL.
   2. Administration provtjänsten tar emot och skickar URL:en vidare till provtjänstens _**SS12000-klient**_.
      >[Läs mer om SS12000-klienten](../ss12000-client/README.md)
   3. SS12000-klienten hämtar JWT från provtjänstens auktoriseringsserver.
   4. SS12000-klienten skickar anrop med JWT för att prenumerera på ändringar av uppgifter hos huvudmannens
      SS12000-API. Vid prenumeration registrerar SS12000-klienten en specifik "target-URL" för varje SS12000-API URL
      som kommer att användas för att ta emot notifieringar.
   5. SS12000-klienten skickar flera anrop till huvudmannens SS12000-API för att hämta alla uppgifter som behövs i
      provtjänsten.<br /><br />
   6. SS12000-klienten skickar statistik via ändpunkten "/statistics" för uppgifter som togs emot och lagrats i provtjänsten.
   7. När fel upptäckts i en uppgift skickar SS12000-klienten felmeddelande via ändpunkten "/log". 
7. När uppgifter ändras ska huvudmannens SS12000-API skicka notifieringar till provtjänstens SS12000-klient.
   Notifiering om ändringar ska ske enligt nedanstående flöde:
   1. Huvudmannens notifieringsklient hämtar JWT från provtjänstens auktoriseringsserver.
   2. Huvudmannens notifieringsklient skickar notis med JWT till SS12000-klienten.
   3. SS12000-klienten verifierar huvudmannens JWT och registrerar tidpunkten för när notisen togs emot och de
      objekt som notisen handlar om.
   4. SS12000-klienten hämtar JWT från provtjänstens auktoriseringsserver.
   5. SS12000-klienten skickar anrop med JWT för att hämta objekt som har ändrats 5 minuter bakåt från den
      registrerade tidpunkten för notifieringen.
   6. Huvudmannens SS12000-API ska verifiera JWT som kommer från SS12000-klienten.
      >[Läs mer om hur JWT verifieras](../authentication-api/README.md#verifiering-av-jwt-som-skickas-från-provtjänstens-ss12000-klient)
   7. SS12000-klienten uppdaterar de uppgifter som har ändrats.
   8. SS12000-klienten skickar statistik via ändpunkten "/statistics" för uppgifter som togs emot och lagrats i provtjänsten.
   9. När fel upptäckts i en uppgift skickar SS12000-klienten felmeddelande via ändpunkten "/log".
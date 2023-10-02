## Checklista för en huvudman som överför uppgifter genom att skicka data till provtjänstens Provisioning API

1. Samla in och förbereda uppgifter som ska överföras till Skolverkets provtjänst.
   >[Se attributlista för provisionering](https://www.skolverket.se/download/18.3c0656d5188bc287d51332f/1688496183174/Uppgifter%20vid%20maskin-till-maskin%20provisionering%20v0.3_23-07-03.pdf).
2. Hämta provtjänstens öppna data från _**open-data API**_.<br />
   Provaktiviteter som motsvarar ämnen eller kurser där det genomförs nationella prov och bedömningsstöd ska
   hämtas från open-data API och konfigureras i huvudmannens system. Notera att undervisningsgrupper som genomför
   ett nationellt prov eller bedömningsstöd behöver kopplas till en provaktivitet.<br /><br />
   Det är också rekommenderat att hämta studievägs- och skolenhetsinformation från open-data-api för att sedan
   kontrollera uppgifter innan överföring till Skolverkets provtjänst.<br /><br />
   Notera att det är Skolverkets identifierare (UUID) för skolenheter och provaktiviteter som ska användas vid
   överföring av uppgifter.
   >[Läs mer om open-data API](../open-data-api/README.md)

   >[URL till Open-data API](https://api-pre.skolverket.se/dnp/iga/open-data/swagger-ui.html)
3. Skaffa klientcertifikat och etablera integration mot provtjänstens auktoriseringsserver.
   >[Läs mer om provtjänstens auktoriseringsserver](../authentication-api/README.md)

   >[URL till provtjänstens auktoriseringsserver](https://nutid-auth-test.sunet.se/transaction)
4. Huvudmannens klient skickar uppgifter till provtjänstens Provisioning API.
   >[URL till provtjänstens Provisioning API](https://api-pre.skolverket.se/dnp/iga/provisioning/v1/swagger-ui.html)
   
   Överföring av uppgifter ska ske enligt nedanstående flöde:
   1. Huvudmannens klient hämtar JWT från provtjänstens auktoriseringsserver.
   2. Klienten ska verifiera JWT och skicka JWT i varje anrop som görs mot provtjänstens Provisioning API.
   3. Huvudmannens klient ska skicka alla uppgifter som behövs i provtjänsten. Uppgifterna ska skickas i följande ordning:
      ````
      Person -> Duty -> Group -> Activity
      ````
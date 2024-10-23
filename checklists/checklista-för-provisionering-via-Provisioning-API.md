## Checklista för en huvudman som överför uppgifter genom att skicka data till provtjänstens Provisioning API

1. Samla in och förbereda uppgifter som ska överföras till Skolverkets provtjänst.
   >[Se attributlista för provisionering](https://www.skolverket.se/skolutveckling/digitala-nationella-prov/overforing-av-uppgifter-till-skolverket#h-UppgiftersombehoveroverforastillSkolverket)

2. Hämta provtjänstens öppna data från _**open-data API**_.<br />
   Provaktiviteter som motsvarar ämnen eller kurser där det genomförs nationella prov och bedömningsstöd ska
   hämtas från open-data API och konfigureras i huvudmannens system. Notera att undervisningsgrupper som genomför
   ett nationellt prov eller bedömningsstöd behöver kopplas till en provaktivitet.<br /><br />
   Det är också rekommenderat att hämta studievägs- och skolenhetsinformation från open-data-api för att sedan
   kontrollera uppgifter innan överföring till Skolverkets provtjänst.<br /><br />
   Notera att det är Skolverkets identifierare (UUID) för skolenheter och provaktiviteter som ska användas vid
   överföring av uppgifter.
   >[Läs mer om open-data API](../open-data-api/README.md)
   
3. Skaffa klientcertifikat och etablera integration mot provtjänstens auktoriseringsserver.
   >[Läs mer om provtjänstens auktoriseringsserver](../authentication-api/README.md)

4. Skaffa fullmakt i _**Mina ombud**_ för de personer som hanterar överföring av uppgifter, inloggning och
   skyddade personuppgifter för huvudmannens räkning i Skolverkets e-tjänst _**Administration provtjänsten**_.
   >[URL till Mina ombud](https://minaombud.se/)

   >[Läs mer om Administration provtjänsten](https://www.skolverket.se/skolverkets-e-tjanst-administration-provtjansten)

5. Registrera e-postadress i _**Administration provtjänsten**_ för mottagande av meddelanden om viktig information gällande skyddade
   personuppgifter.<br /><br />

6. Överföra uppgifter till Skolverkets provtjänst med att skicka data till Provisioning API. Överföring av uppgifter ska ske enligt nedanstående flöde:
   1. Huvudmannens klient hämtar JWT från provtjänstens auktoriseringsserver.
   2. Klienten ska verifiera JWT och skicka JWT i varje anrop som görs mot provtjänstens Provisioning API.
      >[Denna sekvensdiagram visar autentiseringsflöden](../provisioning-api/README.md#autentiseringsflöde)
   3. Huvudmannens klient ska skicka alla uppgifter som behövs i provtjänsten i rätt ordningsföljd.
      >[Denna sekvensdiagram visar i vilken ordning som uppgifterna ska skickas](../provisioning-api/README.md#ordningsföljd-för-överföring-av-uppgifter-via-provisioning-api)
      
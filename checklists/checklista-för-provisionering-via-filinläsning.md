## Checklista för en huvudman som överför uppgifter via filinläsning

1. Samla in uppgifter som ska överföras till Skolverkets provtjänst. Uppgifter som behövs i Skolverkets 
   provtjänst finns i följande Excelmall. 
   >[Se Excelmall för överföring av uppgifter via filinläsning](https://www.skolverket.se/download/18.a1a676d18aaddf64d713e4/1695665450567/Excelmall%20f%C3%B6r%20filinl%C3%A4sning%20v0.8_2023-09-25.xlsx).
2. Det är inte obligatoriskt men är rekommenderats av Skolverket att hämta studievägs- och skolenhetsinformation
   från _**open-data API**_ för att sedan kontrollera uppgifter innan överföring till Skolverkets provtjänst.
   
   >[Läs mer om open-data API](../open-data-api/README.md)

   >[URL till Open-data API](https://api-pre.skolverket.se/dnp/iga/open-data/swagger-ui.html)
3. Förbereda uppgifter enligt Skolverkets mall för filinläsning.
   1. Ta bort exempeldata i Excelmallen
   2. Fyll i följande fyra flikar i Excelmallen:
      1. _**Klasser**_: Excelblad som ska innehålla information om klasser i specifika skolenheter hos huvudmannen.
      2. _**Ämnesgrupp & provaktiviteter**_: Excelblad som ska innehålla information om ämnesgrupper
         (undervisningsgrupper) och vilket nationellt prov eller bedömningsstöd som varje grupp genomför.
      3. _**Elevinskrivningar**_: Varje rad i detta Excelblad ska innehålla information om en elevs:
         * personuppgifter
         * tillhörighet till skolenhet som bedrivs av huvudmannen
         * klasstillhörighet
         * tillhörigheter till alla ämnesgrupper (undervisningsgrupper).
         
         `Notera att elever som hör skolformen VUXGY (Kommunal vuxenutbildning på gymnasial nivå) inte kan överföras
         till provtjänsten för närvarande.`
      4. _**Personalstjänstgöringar**_: Excelblad som ska innehålla information om personal och deras tjänstgöringar
         hos skolenheter som bedrivs av huvudmannen. Notera att varje tjänstgöring som en personal har i skolenheter
         som bedrivs av huvudmannen ska fyllas i på egen rad i Excelbladet.    
   3. Det är inte obligatoriskt men Skolverket rekommenderar att förbereda alla uppgifter i en fil vid första
      inläsning.<br /><br />
4. Skaffa fullmakt i _**Mina ombud**_ för de personer som administrerar provisionering för huvudmannens räkning i
   Skolverkets e-tjänst _**Administration provtjänsten**_.
   >[URL till Mina ombud](https://minaombud.se/)
   
5. Läsa in fil via Skolverkets e-tjänst Administration provtjänsten.
   >[Läs mer om Administration provtjänsten](https://www.skolverket.se/skolverkets-e-tjanst-administration-provtjansten)

   >[URL till Administration provtjänstens testmiljö](https://administrationprovtjansten-pre.skolverket.se)

   Filinläsning ska ske enligt nedanstående flöde:
   1. Huvudmannens ombud loggar in i Administration provtjänsten. 
   2. Om en person är ombud för flera huvudmän väljer ombudet den huvudman som hen kommer att överföra
      uppgifter för.
   3. Huvudmannens ombud laddar upp fil och väntar på överföringsstatus. Notera att det kan ta alltifrån några sekunder
      till flera minuter att ladda upp fil.
   4. Administration provtjänsten presenterar överföringsstatus av alla uppgifter i en tabell. Tabellen innehåller
      både lyckade och misslyckade uppgifter. Misslyckade uppgifter behöver rättas och läsas in igen.
6. Uppgifter som har ändrats från tidigare inläsning bör förberedas i en ny fil och läsas in via
   Administration provtjänsten, exempelvis:
   * Personuppgifter för elever eller personal har ändrats.
   * Elev byter skola inom samma huvudman.
   * Elev byter till en skola som bedrivs av annan huvudman. Avslut av elevinskrivning ska rapporteras till
     Skolverket med att sätta "Ja" i "Ta bort" kolumnen för den specifika elevinskrivning.
   * Personals tjänstgöring i en skolenhet har avslutat. Detta ska också rapporteras till Skolverket
     med att sätta "Ja" i "Ta bort" kolumnen för den specifika tjänstgöring.
   * Ny elev har registrerats.
   * Ny personal har registrerats eller befintlig personal har fått ny tjänstgöring.
   * Ny klass eller ämnesgrupp har skapats.
   * Elevs klass- eller ämnesgrupp tillhörighet har ändrats.
   * Personals ämnesgrupp tillhörighet har ändrats.
   * Namn på en klass eller ämnesgrupp har ändrats.
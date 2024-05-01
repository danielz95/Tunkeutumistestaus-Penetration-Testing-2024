# x) Lue/Katso ja tiivistä

## OWASP Top 10 2021: Broken Access Control

### Mikä Broken Access Control?

Tämä Web-pääsynhallintahaavoittuvuus mahdollistaa tietohin pääsyä, muokkaamista tai tuhoamista luvattomilta käyttäjiltä. 

Tyypillisimmät haavoittuvuudet:
-  Least privlidge laiminlyönti
-  URL parametrien muokkailu, joka ohittaa pääsynhallintamekanismit
-  Toisten tilille pääsy UID:n viittauksien avulla (insecure direct object references)
-  API:lle pääsyt puuttuvilla POST,PUT,DELETE toiminnoilla
-  Käyttöoikeuksien nostattamista admin-oikeuksilla normaalikäyttäjänä
-  Metadatan muokkaamista JWT:lla, AC tokeneilla tai kekseillä
-  CORS määritysvirheet, mahdollistavat API pääsyä epäluotettavilta lähteiltä
-  'Force Browsing' mahdollistaa pääsyn rajoitettuihin sivustoihin luvattomalta käyttäjältä

### Miten estetään?

Käytetään palvelinpuolen koodia tai serverless API:ta. Estetään hyökkääjän pääsy muokkaamasta AC tarkistuksia tai metadataa.

-  Kielletään pääsy yksityisiin resursseihin oletuksena ja implementoidaan AC mekanismit kerran, ja uudelleenkäytetään ne läpi sovellusta.
-  CORS käytön minimointi myös auttaa vähentämään haavoittuvuusriskiä
-  AC Mallit määrittävät ketkä omistajat voivat muokata mitäkin tietoja ja Domain Mallit määrittävät miten liiketoiminnan applikaatioiden vaatimukset ja rajoitukset määritetään.
-  Piilotetaan Webbipalvelinten hakemistot ja metadatat, sekä root hakemistot.
-  AC virhetilanteiden loggaus ja hälytykset admineille.
-  API ja controllerien rate limitit automaattisten hyökkäystyökalujen minimointiin.
-  Aikakatkaistuja JWT tokeneita ja Stateful sessioiden käyttöä.
-  

# x) Lue/Katso ja tiivistä

## OWASP 

### A10:2021 - Broken Access Control

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

**Broken Access Control - Miten estetään?**

Käytetään palvelinpuolen koodia tai serverless API:ta. Estetään hyökkääjän pääsy muokkaamasta AC tarkistuksia tai metadataa.

-  Kielletään pääsy yksityisiin resursseihin oletuksena ja implementoidaan AC mekanismit kerran, ja uudelleenkäytetään ne läpi sovellusta.
-  CORS käytön minimointi myös auttaa vähentämään haavoittuvuusriskiä
-  AC Mallit määrittävät ketkä omistajat voivat muokata mitäkin tietoja ja Domain Mallit määrittävät miten liiketoiminnan applikaatioiden vaatimukset ja rajoitukset määritetään.
-  Piilotetaan Webbipalvelinten hakemistot ja metadatat, sekä root hakemistot.
-  AC virhetilanteiden loggaus ja hälytykset admineille.
-  API ja controllerien rate limitit automaattisten hyökkäystyökalujen minimointiin.
-  Aikakatkaistuja JWT tokeneita ja Stateful sessioiden käyttöä.

**Broken Access Control - Esimerkkihyökkäykset:**

Näitä voivat olla muodossa SQL-injektiot tai piilotettujen webbipolkujen kokeilemista (Force browsing)

### A01:2021 - Server-Side Request Forgery (SSRF) 

SSRF haavoittuvuudet tapahtuvat kun webappit hakevat käyttäjäpyynnön perusteella resursseja ilman, että ne tarkistavat URL:ien luvallisuutta. Näitä pyyntöjä saattavat mennä myös palomuurien, VPN:ien tai ACL:ien läpi. Näiden hyökkäyksien yleisyys ja vakavuusasteet lisääntyvät pilvipalveluiden ja eri arkkitehtuurien monimutkaisuuksien myötä.

**SSRF - Miten estetään?**

-  Resurssien verkkosegmentoinnilla minimoidaan SSRF haavoittudet
-  Tulimuurien ja NAC sääntöjen ylläpito
-  Sovelluskohtaisten tulimuurisääntöjen elinkaaren toteutus ja kaiken liikenteen loggaus.
-  Clienttien datasyöttöjen validointi
-  URL kaavan, portin ja vastaanottajan valvonta
-  Ei raakavastausia clienteille ja ei HTTP uudelleenohjauksia.
-  URL poikkeavuuksien havainnointi välttääkseen DNS rebind ja TOCTOU hyökkäyksiä.
-  Vältä perinteistä deny-listan käyttöä. Hyökkääjillä olemassa työkalut, jolla listat
  kierretään.
- Lisäksi tietoturvaan liittyvät palvelut ei pidetä frontendissa, ja frontendissa kannattaa     käyttää VPN:ää verkkoliikenteen salaamiseksi.

**SSRF - Esimerkkihyökkäykset**

SSRF hyökkäyksiä voidaan toteuttaa esim. Porttiskannaamalla sisäverkkoja, varastamalla suojaamattomia sisäverkon datavarastojen tietoja tai pilvipalveluiden metadatavarastoja. Haavoittuneella sisäverkolla voidaan toteuttaa myös muita hyökkäyksiä kuten RCE ja DoS hyökkäykset.

## PortSwigger

### Insecure direct object references (IDOR)

Tällä hyökkääjän syöttämällä URL:llä päästään käsiksi viittaamalla suoraan database objekteihin, staattisiin tiedostoihin tai käyttäjäID:siin, jolloin käyttäjä voi päästä suoraan näihin tietoihin käsiksi.

### Path Traversal

Tällä haavoittuvuudella hyökkääjä voi URL:ia muokkaamalla päästä /var/www/ palvelinhakemistosta muihin palvelimen kansioihin esim parametri `loadImage?filename=img1.png` sijaitsee var/www/images kansiossa. Hyökkääjä voi muokata tätä muotoon `loadImage?filename=../../../etc/passwd` ja päästä katsomaan 'passwd' tiedostoa Linux palvelimella.






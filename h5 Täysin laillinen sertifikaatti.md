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

Tällä hyökkääjän syöttämällä URL:llä päästään käsiksi viittaamalla suoraan database objekteihin, staattisiin tiedostoihin tai käyttäjäID:siin, jolloin käyttäjä voi päästä suoraan näihin tietoihin käsiksi. Esim.

`https://.../static/12144.txt` saadaan palvelimen puolelta .txt tiedostoa luettua.

`https://.../customer_account?customer_number=132355` saadaan avattua backend tietokannasta asiakkaan ID:n 132355 henk.koht. sivua. 

### Path Traversal

Tällä haavoittuvuudella hyökkääjä voi URL:ia muokkaamalla päästä /var/www/ palvelinhakemistosta muihin palvelimen kansioihin esim parametri `loadImage?filename=img1.png` sijaitsee var/www/images kansiossa. Hyökkääjä voi muokata tätä muotoon `loadImage?filename=../../../etc/passwd` ja päästä katsomaan 'passwd' tiedostoa Linux palvelimella.

### Server-side template Injection

Tällä haavoittuvuudella hyökkääjä voi käyttää natiiveja template syntakseja, jotka suoritetaan pavelimen puolella. Templatella syötöt suoritetaan komentoina palvelimen puolella sen sijaan, että ne luetaan datana. Tämä yleensä vakavampi kuin client-side template injectionit, sillä templatet tarkistetaan ja suoritetaan palvelimen puolella.

Hyökkäys tapahtuu vaiheissa, jossa ensin tunnistetaan esim. fuzzaamalla templateissa yleisimpiä erikoismerkkejä, joka auttaa tunnistamaan käytettävää template engineä ja mahdollisen haavoittuvuuden olemassaoloa. 

Template engineitä ovat mm.
-  Smarty
-  Mako
-  Jinja2
-  Twig

Nämä käyttävät erilaisia template syntakseja esim.

Syntaksi `{{7*'7'}}` palauttaa Twigissä `49` ja Jinja2:ssa `7777777`

### Server-side request forgery (SSRF)

SSRF mahdollistaa hyökkääjän tekevän palvelinpuolen appipyyntöjä toiseen kohteeseen, johtuen web-turvamekanismin haavoittuvuudesta. Hyökkääjä pääsee tällä esim. pakottamaan palvelimen avaamaan hänelle yhteyden sisäverkkoon ja yhdistämään sen ulkoiseen verkkoon.

Tämä voidaan tehdä muuttamalla POST pyynnön tiedot esim.verkkokauppan varastosaldon takrikstuksen pyynnössä muutetaan varastosaldotarkistuksen sisäverkon URL sijaintia esim. admin sijaintiin:

````
POST /.../...
Content type : ...
Content-Length : ...

stockApi=http://.../product/stock/check%3FproductId%3D6%storeId%3D1

````

Vaihdetaan stockApi osio:

`stockApi=http://localhost/admin`

### Cross-site scripting (XSS)

Tämä haavoittuvuus mahdollistaa hyökkääjän ijektoimaan JavaScript koodia uhrikäyttäjän selaimeen ja tätä kautta varastamaan käyttäjän ID/password ja muita tietoja, sekä tekemään samoja toimintoja mihin käyttäjäkin pystyy. 

XSS hyökkäyksien tyyppejä ovaat:
-  Reflected XSS: Hyökkääjä kirjoittaa scriptin suoraan URL:iin. Skripti käynnistyy, kun vieraillaan URL:ia.
  
-  Stored XSS / Persistent XSS / Second-order XSS: Hyökkääjä tallettaa skriptin johonkin postaukseen esim. chattifoorumilla, viestinä, sähköpostina yms. Uhri klikkaa viestin ja skripti käynnistyy.
  
-  DOM-based (Document Object Model) XSS: Hyödynnetään JavaScriptiä, joka käytetään HTML sivulla suorittamaan HTML toimintoja. Jos hyökkääjä pääsee käsiksi näihin toimintoihin, skriptiä voi ajaa osana jotakin ns. sivun normaalia toimintoa.

# a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Laita ZAP sieppaamaan myös kuvat, niitä tarvitaan tämän kerran kotitehtävissä. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään. (Ei toimi localhost:lla ilman Foxyproxya)

### ZAP asennus

Aloitin asentamalla Kali virtuaalikoneeseeni ZapProxy komennolla:

`sudo apt-get -y install zaproxy`

Asennuksen jälkeen käynnistin:

`zaproxy`

ZAP:in käynnistyessä valitsin 'Persist ZAP Session' vaihtoehtoon NO:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/3c26f37d-4697-4120-aa3b-125b60759b49)

ZAP:in versio:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/1b94d132-8edb-4b65-9388-3837e40a0123)

Kävin kytkemässä selaimen HUD näkymän pois päältä valitsemalla ZAP:in yläpalkista **Tools -> Options... -> HUD -> Enable when using ZAP Desktop** ja poistamalla ruksin sen kohdasta.

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/ebb6dbb3-e3e6-4227-b297-d6ee54c56dac)

Proxyn saa päälle Firefoxiin avaamalla sen ZAP yläpalkin Firefox ikonista:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/f5b3cda5-c4ce-4bc2-9da1-97b46c8e0336)

Tämä avaa Firefoxin automaattisesti käyttäen localhost:8080 proxya:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/5774ca8a-97a5-4bee-b5a2-b43387cf80f7)

Laitoin seuraavaksi Apache2 web-palvelimen päälle Kalilla:

`sudo systemctl start apache2`

Testasin proxy selaimella mennä siihen:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/8ba4a1fe-2cda-448d-a91d-133a45af2c16)

Hakupyyntö ilmestyy myös ZAP:iin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/deb1220c-9239-4d81-9423-f3fa6deb99eb)

### FoxyProxyn, sekä CA-sertifikaatin generointi ja asennus Firefoxiin

ZAP:in selain ei kuitenkaan tallenna CA sertifikaatteja, eikä lisäosia, joten asennetaan Kalin Firefoxiin kyseiset lisäkonfiguraatiot.

Avasin Firefoxin ja asensin Foxyproxyn: https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/b41ed532-ec2f-4cc5-8232-42ebe7f735ca)

**Remove** napin tilalla on **Add to Firefox**. Minulla sattui olemaan se valmiiksi asennettuna.

Seuraavaksi määritetään FoxyProxyn ottamaan ZAP proxyn tiedot:

Valitaan tästä **Preferences**

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/5a1e4210-a01e-4ef1-962d-c1b9d0693b5a)

Määritetään proxyn tiedot ja valitaan **Save**:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/59f862d9-eb0f-41b6-a3de-759128b64378)

Klikataan **ZAP Proxy** päälle:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/3a7beaf7-986d-4a9d-90eb-886162662742)

Nyt kun FoxyProxy on päällä, yritetään avata jotakin sivua:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/3ab4f654-7298-43c0-a4a6-7c329a0add89)

www.google.com ilmoittaa, ettei CA ole luotettu. Korjataan tämä seuraavaksi.

Avataan **ZAP -> Tools -> Options... -> Network -> Server Certificates**

Klikataan **Generate** ja sen jälkeen **Save**. Tallennetaan .cer tiedosto vaikka root kansioon.

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/0206e88b-0ae1-4f83-ab2b-834bb49000ce)

Mennään takaisin Firefoxiin ja navigoidaan **Settings -> Privacy & Security -> Certificates -> View Certificates... -> Import...**

Valitaan meidän generoimaa **zap_root_ca.cer** tiedostoa ja klikataan päälle molemmat checkboxit ja OK

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/a8da7fa5-2dcb-4936-83be-425181452ab4)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/fb9f1254-f2e7-40c3-9de3-36738ec1542d)

Nyt CA:n pitäisi olla luotettu ja kokeillaan www.google.com uudestaan:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/d819d210-5083-434b-86c1-58fb9a04bf46)

Nyt sivu toimii ja CA on verifioitu. Hakupyynnöt näkyvät myös ZAP:issa:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/c7da7680-4d2c-4442-90b8-a82721f0e512)

localhost hakupyynnöt näkyy myös ZAP:issa:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/dda75a6d-43db-4dd2-9288-c3524c8e2fe0)

# b) Kettumaista. Asenna FoxyProxy Standard Firefox Addon, ja lisää ZAP proxyksi siihen. Käytä FoxyProxyn "Patterns" -toimintoa, niin että vain valitsemasi weppisivut ohjataan Proxyyn. (Läksyssä ohjataan varmaankin PortSwigger Labs ja localhost.)

Edellisessä vaiheessa asensin ja konffasin FoxyProxyn. Nyt kytketään "Patterns" toiminto Foxyproxysta sieppaamaan vain PortSwigger ja localhost liikenteen.

FoxyProxysta avasin **Proxies** välilehden ja lisäsin ZAP Proxy:n alle Pattersit localhostille ja portswiggerille:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/cb97f599-1b31-4e0c-8aad-375eb871d647)


Nyt kun Patternit tallentaa, kytketään päälle **Proxy by Patterns** vaihtoehtoa:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/2328b600-1709-4d6d-818a-870ce4a59071)

Nyt ZAP sieppaa ainoastaan portswigger ja localhost liikenteen:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/8de3340e-9cea-4660-981d-10575871b753)

Nyt ollaan valmiita aloittamaan PortSwigger Labratehtävät.

# PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu. Kannattaa käyttää ZAPia, vaikka malliratkaisut käyttävät harjoitusten tekijän maksullista ohjelmaa. Malliratkaisun kopioiminen ZAP:n tai selaimeen ei ole vastaus tehtävään, vaan ratkaisu ja haavoittuvuuden etsiminen on selitettävä ja perusteltava.

## c) Insecure Direct Object Reference (IDOR)

Tein ensin uuden Patternin PortSwigger Labran harjoitussivuille, jotta ZAP sieppaa sen liikenteen:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/1bf77f90-3e47-4290-aa2e-6d666f47e525)

Aloitin menemällä login sivulle ja Postasin Username: carlos ja salasana: 123

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/704694f9-b174-4bc0-af2d-b998d2efdd1b)

Salasana oli väärä, joten tarkistelin Zap:ista POST tiedot:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/4e367809-db65-42d1-9211-d1fe5f8856fe)

Hetken mietittyä kokeilin mennä **Live Chat** osioon, joka myös sieppaa keskustelulogit ZAP:iin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/2f1c9979-c75e-451b-bdf1-a6202b43431d)

Huomasin, että transcriptit alkaa tekstitiedostolla 2.txt ja mietin puuttuvaa 1.txt alkavaa tiedostoa.

Avasin keskustelun contentit ZAP:in Requester ikkunaan:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/f1bcc602-935a-41e0-bcd9-5124f6141a24)

Vaihdoin headerista **3.txt** osion lataamaan **1.txt** tilalle ja painoin **Send**

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/0ac1210d-129e-4f76-b437-a2fa146eab51)

Vastaus palautti logitiedosto **1.txt**, jossa näkyy käyttäjän carlos:in salasana:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/890e42ca-fc4a-4544-9fbb-491cd6dd3b83)


Syötin **Login** ikkunaan carlosin kirjautumistiedot ja sain Labran ratkottua!


![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/ae3e7e9b-ed0e-4f8e-9cd5-e8f0179c096f)

## 















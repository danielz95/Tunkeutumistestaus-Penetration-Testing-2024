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

## d) File path traversal, simple case

Tässä oli tarkoitus saada `etc/passwd` hakemiston tietoja.

Katsoin ZAP request headereista kuvatiedostot ja ne oli URL muodossa:

**/image?filename=xx.jpg**

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/0479f3a6-c0e1-4dae-96bb-b036b29369bc)

Lähdetään tässä muuttamaan .jpg tiedostojen tilalle tiedostopolkua etc/passwd hakemistoon.

Seurasin tämän artikkelin avulla path traversal peiaatteen ja lähdin toteuttamaan: https://portswigger.net/web-security/file-path-traversal

Hain jonkun kuvatiedoston tiedon ZAP:ista:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/77066528-0efa-42f4-b118-9c622c4b3ad2)

Laitoin tämän Requesteriin ja muokkasin syntaksin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/be5553c2-97cd-4aea-ab2d-0e7bf766b85f)

Tässä muutetaan tiedostopolkua, jossa kuvat ovat oletettavasti **var/www/images** hakemistopolulla palvelimen puolella. Käytännössä uudella muokatulla pyynnöllä siirrytään hakemistossa 3 kertaa taaksepäin root kansioon ja sen jälkeen eteenpäin **/etc/passwd** kansioon. 

Lähetin pyynnön ja sain labran ratkottua!

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/9d40d8c1-71d5-4c5a-94ca-97963f0dedd4)

# e) File path traversal, traversal sequences blocked with absolute path bypass

Tässä on sama tavoite kuin edellisessä "Path traversal" tehtävässä, mutta polkujen vaihtosyntaksit ovat blokattu. 

Hain kuvatiedostoa sisältävän headerin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/d8dc7fff-dd7b-4308-9c0a-518ba6fadb86)

Yritin kiertää polun vaihtosyntaksin laittamalla tilalle `....//`

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/7d119843-675c-4759-b86f-352d296190bc)

Tämä ei tuottanut tulosta, joten kokeilin kirjoittaa absoluuttisen polun Requestiin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/080ebfd1-218c-4e3b-99e0-636abac322d9)

Näin se lähti toimimaan hakemalla tiedoston absoluuttista polkua!

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/70a5cdd2-ceb0-4b81-aeb4-bfa47a1b7320)

# f) File path traversal, traversal sequences stripped non-recursively

Tässä sama periaate kuin edellisissä path traversal tehtävissä, mutta tällä kertaa käytetään 'non-recursive' polkujen vaihtoa. Eli palvelin blokkaa normaalia `../` polunvaihtosyntaksia, joten käytetään `....//` syntaksia kiertämään tätä blokkia.

Etsin jälleen Image headerin requesteista:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/8943427b-c56f-4e99-8362-83fad7cb6241)

Tämän jälkeen laitoin sen Requesteriin ja vaihdoin syntaksin, sekä lähetin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/4854351d-3892-4533-8b89-15cfe7480f70)

Palvelin vastaa statuksella **200 OK** eli pyyntö meni läpi ja sain tehtävän tehtyä!

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/fb288f5b-ad78-456e-a1d5-9384b2a879a5)

# g) Server-side template injection with information disclosure via user-supplied objects

Tässä suoritetaan SSTI injektio, jossa hyödynnetään palvelinpuolen template enginen haavoittuvuutta. Tarkoitus on löytää frameworkin salainen avain.

Tehtävänannossa annettiin kirjautumistunnus ja salasana **ID: content-manager / PW: C0nt3ntM4n4g3r**

Aloitin kirjautumalla **content-manager** käyttäjällä sisään:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/b15941b6-f8d9-498c-a58f-75e89074df88)

Seuraavaksi valitsin jonkun tuotteen kotisivulta ja valitsin tuotteen sivulta **Edit template** nappia:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/c81efe96-5e0c-4dae-93b6-c1873f8b732c)

Tästä pääsee muokkaamaan tuotteen **Description osiota**. Huomasin, että viimeisellä rivillä sivu hakee tuotteen nimen, hinnan ja saldoarvot tietyllä templatella käyttäen **{{teksti}}** muotoa:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/8722619e-0b45-4afd-94dd-7b9bf39647ce)

Pitkän ihmettelyn ja kokeilun jälkeen, päätin katsoa ratkaisuvideon tästä, missä kerrottiin eri vaiheet:
https://www.youtube.com/watch?v=8o5QPU-BvFQ&t=243s

Tässä on siis kyseessä Django frameworkin, josta voidaan selvittää syöttämällä templateen `{% debug %}` toiminnon:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/a859c601-aa35-4e8a-81e3-92db1a1f67cf)

Se näyttää meille debuggerin kautta Pyhton kirjaston kontentin, jonka kautta lähdetään selvittämään salaisen avaimen.

Löysin HackTricks:in sivulta tietoa, miten salaisen avaimen saa näkyviin syöttämällä templateen **{{settings.SECRET_KEY}}**:

https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/2b2fd530-6cc8-4736-b53b-e199a41489a5)

Syötin tämän labran Template editoriin ja tällä sain salaisen avaimen:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/caaa0a61-ac60-46f8-87cc-20c3d4919bbb)

Syötin avaimen vastauskenttään ja näin ratkaisin tehtävän:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/02b41378-74f7-44b8-b3e5-dd680c86a998)


# h) Basic SSRF against the local server

Tässä tehtävänä on päästä käsiksi **http://localhost/admin** tilille ja poistaa käyttäjää **carlos**

Tässä tehtävänannossa mainittiin, että stock check toiminnossa on haavoittuvuus, jossa pystytään sitä kautta saamaan admin oikeudet käyttöön.

Aloitin avaamalla labrasivusta jonkun tuotteen ja tarkistamalla sen varastosaldoa:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/2ebf1efe-71ff-4d7d-b4c7-1d69f59edff1)

ZAP:ista ilmestyy POST headeri, jonka syötin requesteriin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/9c83ca9e-c623-4a16-b9e7-cbf8bd3bf953)

Tässä kohdassa **StockApi** arvo muutetaan ja pyyntö lähetetään:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/3ea1bb36-0d30-4032-af20-13e1dd7c9cf5)

Response osiossa näkyy Admin Panel:in HTML sisältö, jossa löytyy toiminto poistaa käyttäjiä.

Lähetin uuden StockApi pyynnön uudella parametrillä poistaakseen käyttäjää **carlos**

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/ec52e23a-f23a-4652-a327-58b1de09b557)

Vastaus ilmoittaa, että **Admin interface only available if logged in as an administrator, or if requested from loopback**. Oletan, että kun pyyntö tehtiin localhost:in kautta niin se meni läpi. Sain tämän tehtävän suoritettua:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/8bf6b4b5-35af-4bcb-be80-2c0a03b3f492)

# i) Reflected XSS into HTML context with nothing encoded

Tässä tehtävässä on tarkoitus tehdä XSS hyökkäys kutsumalla **alert** funktiota.

Aloitin perehtymällä aiheeseen tarkemmin: https://portswigger.net/web-security/cross-site-scripting/reflected

XSS artikkelia luettuani, lähestyin tehtävään yritten etsiä 'search' parametriä HTTP paketista ja syöttämällä hakutermin tilalle skriptiä.

Aloitin tekemällä esimerkkihakua hakusanalla 'gift':

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/cf85c081-c209-4cce-9f82-4cdde77add0d)

Sen jälkeen etsin requesteistä headerin, jossa hakusanaparametri esiintyy:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/f8d0eb64-8f3e-4346-9f24-67ad4d5919b8)

Muokkasin hakuparametriä niin, että se ajaa scriptin **alert("You have been swigged!)**:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/e1895637-7c0f-41d4-9ebf-f2d64c8edc42)

Pyyntö oli ajanut skriptin hakuparametriin ja GET request oli muuntanut skriptin syntaksia erikoismerkkien **< ja >** kohdille. 

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/b15d8f26-b13e-4c4f-a5dd-4c57326dd73b)

Tehtävä meni läpi:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/dc889812-5f03-44f8-883e-aa3142d13b82)

# j) Stored XSS into HTML context with nothing encoded

Tässä tehdään Stored XSS hyökkäys, jossa postataan blogisivun kommenttikenttään skripti, joka suoriutuu kun blogisivua avataan.

Aloitin avaamalla jonkun blogijulkaisun ja kirjoittamalla kommenttiin scriptin ja täyttämällä loput tiedot tarvittaviin kenttiin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/db256b01-3147-4334-a184-1736e52c609c)

Painoin **Submit** ja tehtävä ratkesi!

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/a39b01f6-52df-45da-87bf-b2e97c1fa5b8)

Nyt kun yritin päivittää sivua, niin alert ilmestyy:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/57bfab1e-8380-4ee3-b540-8cea8785f7e5)

# k) Asenna Webgoat 2023.4. (Uusi versio, jossa on eri tehtäviä kuin vanhemmissa. Jos olet jo asentanut sen, ei tarvitse raportoida asennusta uudelleen; raportoi silloin vain pelkkä testi, jolla toteat sen toimivaksi.)

En ehtinyt enempää harjoituksia tehdä kuin PortSwigger harjoitukset. Näiden tekeminen menee vasta deadlinen jälkeen. 

# Lähteet:

Karvinen T. Eettinen hakkerointi 2024, https://terokarvinen.com/2024/eettinen-hakkerointi-2024/

OWASP, A01:2021 – Broken Access Control, https://owasp.org/Top10/A01_2021-Broken_Access_Control/

OWASP, A10:2021 – Server-Side Request Forgery (SSRF) ,https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/

PortSwigger, Insecure direct object references (IDOR), https://portswigger.net/web-security/access-control/idor

PortSwigger, Path traversal, https://portswigger.net/web-security/file-path-traversal

PortSwigger, Server-side template injection, https://portswigger.net/web-security/server-side-template-injection

PortSwigger, Server-side request forgery (SSRF), https://portswigger.net/web-security/ssrf

PortSwigger, Cross-site scripting, https://portswigger.net/web-security/cross-site-scripting

SalesForce, Setting up ZAP for browser, https://security.my.salesforce-sites.com/security/tools/webapp/zapbrowsersetup

FoxyProxy, URL Patterns, https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/

Seven Seas Security, SSTI Complete Lab Breakdown: SSTI w/ info disclosure via user-supplied objects, https://www.youtube.com/watch?v=8o5QPU-BvFQ

HackTricks, SSTI (Server Side Template Injection), https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection












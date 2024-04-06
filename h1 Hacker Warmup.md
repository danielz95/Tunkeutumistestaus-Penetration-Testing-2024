# h1 Hacker Warmup
Tässä tehtävässä lämmitellään Tero Karvisen pitämää tunkeutumistestauskurssia varten tekemällä tunkeutumiseen liittyvät harjoitukset.

Kurssin tiedot löytyvät täältä: https://terokarvinen.com/2024/eettinen-hakkerointi-2024/

## Lue/katso ja tiivistä
Tässä katsoin läpi videosarjaa 'The Art of Hacking (Video Collection):Lesson 4'

https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/

- Aktiivisessa tiedustelussa käytetään erinäisiä työkaluja ja menetelmiä etsiäkseen haavoittuvuuksia hyökättävän kohteen suojauksessa, kuten porttiskannaukset ja haavoittuvuus-skannaukset
- Metodologiassa edetään järjestyksessä Port scanning, Web Service Review ja lopuksi vasta Vulnerability scanning.
- Yleisin virhe on hypätä suoraan Vuln. scanningiin ja ohittaa edelliset vaiheet.
- Porttiskannaustyökalut:
     * Nmap, suosituin ja monipuolisin porttiskanneri
     * Masscan, nopein porttiskanneri
     * Udpprotoscanner, nopea UDP porttiskaneri
- Webbisivu skannaustyökalut:
     * EyeWitness, taltioi ja ottaa kuvankaappauksia Web-sivuista, portit 80 ja 443
- Verkkohaavoittuvuusskannerit:
     * OpenVAS, ilmainen
     * Nessus, maksaa
     * Nexpose, maksaa
     * Qualys, maksaa
     * Nmap, rajoitettavuudet tähän tarkoitukseen. Voidaan kuitenkin ajaa lukuisia scriptejä jotka paljastavat erilaisia haavoittuvuuksia.
          /nsedoc/ Nmap dokumentaatiosta löytyy kaikki scriptit ja kuvaukset niihin.
- Web-haavoittuvuusskannerit
     * Nikto, helppokäyttöinen
     * WPScan, WordPress sivujen skannaukset
     * SQLmap, SQL injektioihin ja tietokantojen PenTestauksiin.
     * Burp Suite, suosituin web vuln. skanneri
     * Zed Attack Proxy (ZAP), täysin ilmainen


## Kali Linux asennus
Aloitin asentamalla Kali Linux:in, joka tulen käyttämään tämän kurssin harjoituksien tehtävien tekemiseen.

1. Latasin VirtualBox VM:lle käytettävän tiedoston osoitteesta: https://www.kali.org/get-kali/#kali-virtual-machines

   Käytän omalla koneellani VirtualBox versiota 7.0.6

   **![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/86e81c3b-0ccd-4426-8c7b-a43857b32819)**

2. Asensin sen VirtualBox:ille ja kirjauduin oletustunnuksilla:
   - Username: kali
   - Password: kali
 
  ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/ad159056-7d5f-48b7-a3ec-61c2023f6ebc)

   

## Over the Wire: Bandit - tehtävät
Aloitetaan ratkaisemalla Over the Wire - pelin 3 ensimmäistä tasoa (0-2)
Pelit tiedot saa osoitteesta: https://overthewire.org/wargames/bandit/
### Level 0 : Kirjaudu sisään peliin
 Kirjaudun Kali:lla SSH:n kautta peliin:

   - Username: **bandit0**
   - Password: **bandit0**
   
   ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/92f97182-6322-4cc6-9410-c226be8d65a3)
   
   **!!Pelin sääntöjä kunnioittaen, en julkaise tasojen ratkaisuja, vaan pelkät lopputulokset!!:**

   ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/1aaa58c3-3b9c-45c6-aefb-8b9060c9a4de)

### Level 0 -> Level 1: Etsi salasana readme-tiedostosta päästääkseen Level 1:seen

Tehtävänä etsii kotihakemistosta readme-tiedosto, josta löytyy salasana päästääkseen Level 1 - tasolle

Löydettyäni salasanan, kirjauduin taas peliin SSH:lla **bandit1** Username:lla ja uudella salasanalla.

### Level 1: Etsi salasana '-' nimisestä tiedostosta päästääkseen Level 2:seen.

Tässä tehtävässä käytännössä sama periaate kuin edellisessä, mutta tiedoston avaaminen/lukeminen vaatii hieman erilaista komentoa ;).


## WebGoatin asennus, sekä "HTTP Basics" ja "Developer Tools" tehtävät

### Asennetaan Kali:lle WebGoat 8:

1. Käytin T.Karvisen tekemää ohjeistusta tähän : https://terokarvinen.com/2020/install-webgoat-web-pentest-practice-target/
2. Ensin asennetaan ufw (Jätin Javan, wget ja bash-completion asennukset pois, koska ne oli jo esiasennettuna Kaliin.)
   Näitä tarkistin komennolla **apt list --installed | grep 'paketin nimi'**

   ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/8bd7f2cd-7aba-4ea5-b44b-05ba27aab861)

4. Kytketään ufw päälle:

   ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/94975dd4-80d7-4a43-8bd3-54eb98d4f3de)

5.	Ladataan ja asennetaan WebGoat 8:

   ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/cbf77b3d-dca4-40ec-81c9-c1cc7f404333)

 ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/25322dbc-29fc-4be7-aec3-630d47d15ed0)

Kun konsoliin tulee alla oleva ilmoitus, jätetään konsoli auki taustalle:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/9716de6d-66e3-4fe5-9cc7-894dd7a8d9d3)

Avataan selain (Firefox) ja mennään osoitteeseen **http://localhost:8080/WebGoat**

Rekisteröidään uusi käyttäjä:
- usr: hacker1
- pw : hacker1

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/90ac4153-e1c2-4793-8c0e-7238b23aad94)

### HTTP Basics

Kun WebGoat rekisteröinti on tehty, siirrtyään General -> HTTP Basics välilehteen

Tässä osiossa palvelin ottaa vastaan syötetyn tekstin (post) ja kääntää sen takaperin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/3e98cfb1-f800-4ff6-a9c7-ce533816e355)

Avaamalla selaimen 'Network' osion (F12 -> Network) nähdään pyynnön sisältö:

'attack1' sisälsi POST tiedot kirjoittaessani nimeni kenttään.

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/600a5dd6-c595-4634-9487-6e6b61107067)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/92d9cbc4-d3c2-49c3-897f-2a4ba47ac277)

Seuraavassa osiossa kysytään aikaisemman HTTP:n tietotyyppiä ja 'Magic Number' tietoa:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/f61ffba3-dcd8-4f78-9bc9-b62c1e64707e)

Tässä jouduin avuksi etsimään tietoa netistä, koska en löytänyt 'attack1' osiosta maagista numeroa.

Löysin JD Wilson:in pitämästä WordPress blogista vastauksen: https://thejdwilson.wordpress.com/2020/11/24/webgoat-http-basics/

Maaginen numero oli ilmeisesti 'attack2' POST tiedossa:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/fc877e57-f349-4512-a8ed-203f72e8a345)

'Ylläolevasta kuvasta näkyy, että Magic Number oli 23, jonka syötin kenttään ja sain oikean vastauksen.

### Developer Tools

Siiryn seuraavaksi Developer Tools osioon.

Osiossa käydään läpi selainten (Chrome, Firefox, Edge yms.) kehitystyökalut ja mitä niillä voi tehdä, mitkä ovat itselleni jo jonkin verran tuttuja, joten siirryn suoraan harjoitusosioihin:

Siirryin alla olevaan osioon:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/b19fa0d0-1f5c-4b5e-a32d-982c78856657)

Avasin konsolin ja kutsuin ylläolevaa js funktiota ja sain vastauksen numerosarjan **-413522746**:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/641dcacf-2ce8-4c6b-adec-be4cc9453c44)

Seuraavassa harjoituksessa etsin HTTP pyynnöstä numerosarjaa:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/8e23dc4a-36dc-48da-834f-2956949fa64f)

Painoin 'Go!' painiketta, jolloin palvelin lähetti pyynnön hakemaan 'networkNum' numerosarjaa, joka löytyi devtools:in 'Network' osiosta 'Request' välilehdeltä:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/d3a1166b-840d-40a2-93b5-7aae7e063b8d)

Syötin saamani numerosarjan 75.64880431849772 ja tarkistuksessa meni läpi!

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/c4f4a707-1c64-4bde-9ccd-a48a5ddbc295)

## PortSwigger Labs: SQL injections
Aloitin tämän harjoituksen menemällä harjoitussivuun: https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/279f358f-7a39-4982-b051-7c7e03b4bbaf)


Tässä harjoituksessa olisi tarkoitus tehdä SQL-injektio, joka paljastaa piilotettuja tuotteita, jotka eivät ole vielä julkaistu.

Tein ilmaisen käyttäjän ja pääsin käsiksi harjoituksen labrasivuun.

https://0a87006e03bb2d8e80574981004700fb.web-security-academy.net/filter?category=%27%20or%201=1%20--

Lähdin avaamaan tehtävää tutkimalla kehitystyökalun ikkunan 'Network' ja 'Element' osioita, ja yritin sieltä etsiä SQL lauseita, jotka pystyisi mahdollisesti muokkaamaan. Hetken etsinnän jälkeen pystyin kuitenkin päättelemään, että kategoriat suodatetaan harjoituksen URL osoitteessa esim 'Pets' kohdalla:

- /filter?category='Pets'

Tätä piti siis muuttaa sellaiseen muotoon, jotta se näyttäisi kaikkia tuotteita, sekä sellaisia tuotteita, jotka eivät ole julkisesti saatavilla.
Hetken pohdinnan jälkeen, etsin ratkaisua PortSwigger harjoituksen sivun 'Solutions' osiolta:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/ded5694c-d012-444f-978f-2a89474c6383)

Tämä edellytti jonkun erillisen ohjelman latausta, joka kaappaa network paketteja muokattaviksi ja edelleenlähetettäviksi palvelimeen. Tässä tapauksessa 'Burp Suite', joka voidaan ilmeisesti tulkita Man-in-the-Middle (MitM) hyökkäyksen muotona. Olen käynyt aikaisemmin 'Tietoturvan Perusteet'-kurssilla MitM hyökkäyksiä käyttäen esimerkiksi ZAP Zed Attack Proxy ohjelmaa, joka ajaa samaa asiaa kuin 'Burp Suite'.

Löysin kuitenkin opetusvideon ja ratkaisun tehtävään, joka ei edellyttänyt pakettien sieppausta : https://www.youtube.com/watch?v=X1X1UdaC_90

Tässä muokattiin URL osoitetta:

- /filter?category=' OR 1=1 --

Tämä muokkaus lähettää siis kyselyn, että kategoria on joko tyhjä TAI että se näyttää kaikkia tuotteita (1=1). Viimeinen -- muokkaa loppukyselyn kommentiksi, joten se jättää huomiotta mahdollista 'Released = 1' ehtoa lauseen jatkeessa.

Lopputuloksena saatiin kaikki tuotteet näkyviin ja tehtävän suoritettua.

- ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/564e4e4c-1a05-4dbd-acc5-731b050c15fb)

## Porttiskannaukset

### 1000 tcp-portin skannaus omalta koneelta (localhost)

Aloitin selvittämällä Kali VM:n ip konfiguraatiot:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/2ea7024b-7eec-46ef-8307-91dc552f88fd)

Tämän jälkeen käytin nmap:ia skannaakseen localhostin 127.0.0.1 1000 yleisintä TCP porttia:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/7f59e30f-8fcb-4827-9cef-2488449a3d0d)

Tulokset paljastavat, että 2 porttia 998:sta olivat auki, nämä portit olivat 8080/tcp (http-proxy) ja 9001/tcp (tor-orport)

### Kaikkien porttien skannaus

Seuraavaksi skannasin localhostilta kaikki portit (1-65535)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/be153284-e327-4687-b582-b9b86a526966)

Avointen porttien tulokset olivat samat kuin edellisessä skannauksessa.

### Laaja porttiskannaus kaikille porteille

Seuraavaksi suoritin 'nmap -A' skannausken kaikille porteille:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/521db4c6-ce87-40a8-ae90-e4c189602942)

Tulokset:
   -   Portti 8080 ei anna hirveästi muuta tietoa, kun sen GetRequest vastauksena tuli 404:Not Found. Date osion alla oli ilmeisesti scriptit, mitä nmap yritti ajaa kyseiselle portille ja vastauksena oli tullut 400:Bad Request.
   -   Portti 9001 näyttää 'jdbc', eli Java Database Connectivity palvelua, joka on ilmeisesti sen takia, että minulla on WebGoat päällä taustalla, jonka tietokanta on yhdistetty porttiin 9001.

### Apache2 aloitus ja uusi laaja porttiskannaus

Aloitin Apache2 daemonin ja tarkistin että se pyörii:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/230011cc-d0e0-4bb8-be76-2ac892e46afa)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/bca20ef0-46c1-4f66-88fa-9bef3e7ff516)

Ajoin laajan porttiskannauksen uudestaan:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/1938b34a-0ded-4d3c-9a76-49192e239680)

Nyt tuloksiin ilmestyi portti 80/tcp (http), joka on Apache:n palvelu ja näyttää Apache2:n oletussivua.

# Lähteet

Karvinen T., Tunkeutumistestaus 2024, https://terokarvinen.com/2024/eettinen-hakkerointi-2024/

Santos et al 2018: The Art of Hacking (Video Collection):Lesson 4 (Video series), https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/

Download Kali Linux, https://www.kali.org/get-kali/#kali-virtual-machines

Karvinen T., Install WebGoat, https://terokarvinen.com/2020/install-webgoat-web-pentest-practice-target/

Khalil R., SQL Injection Lab #1 Web Security Academy (Video): https://www.youtube.com/watch?v=X1X1UdaC_90
























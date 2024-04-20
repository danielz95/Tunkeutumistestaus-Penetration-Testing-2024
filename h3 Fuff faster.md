# x) Lue/katso ja tiivistä.

### Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf

  #### Mikä Ffuf?

-  'joohoi':n luoma Web fuzzeri.
-  Auttaa mm. löytämään piilotettuja verkkopolkuja, samankaltainen kun gobuster tai dirtbuster.
-  Verkkopolkujen lisäksi voidaan etsiä headerit, http-metodit (GET, POST) yms.

  #### Miten asennetaan?

-  wget github ffuf repo linuxilla.
-  Ladataan sanakirjasto yleisimmistä verkkopoluista, esim. Daniel Miesselerin luoma Seclists.

  #### Miten käytetään?

-  Yleisin syntaksi polkujen etsimiseen:
    `./ffuf -w common.txt -u http://kohdeURL/FUZZ`
   
  -w lippu käyttää Seclistin 'common.txt' sanalistaa käydääkseen kaikki yleisimmät polut läpi
  -u lippuun syötetään fuzzattavan kohde URL ja /FUZZ osio kokeilee 'common.txt' sanalistan syötteitä yksi kerrallaan, eli generoi tuhansia pyyntöjä.

  #### Tuloksien suodatus

-  **-fc** : Suodattaa HTTP status koodit vastauksesta (200 OK, 404 NotFound yms.)
-  **-fl** : Suodattaa rivimäärän (Lines) vastauksesta
-  **-fmode** : Asettaa suodatusparametreja (and, or)
-  **-fr** : Regular expression (regexp) suodatus
-  **-fs** : Suodattaa HTTP response tavukokoa
-  **-ft** : Suodattaa vastauksien vasteajan mukaan millisekuntteina (esim >100 tai <10)
-  **-fw** : Suodattaa vastausksen sanamäärän mukaan

-  Samankaltaiset tulokset halutaan suodattaa pois, jotta löydetään poikkeavat tulokset.

    `./ffuf -w common.txt -u http://kohdeurl/FUZZ -fs 132`
   
   Tässä suodatetaan tulokset vastauksen koon mukaan (size: 132), jotta löydetään poikkeavat erikokoiset vastaukset, jotka voivat paljastaa piilotettuja verkkopolkuja.

### Karvinen 2022: Cracking Passwords with Hashcat

  #### Mikä Hashcat?

-  Brute-force tyyppinen työkalu salattujen (Hashed) salasanojen murtamiseen.
-  Kokeilee yleisimmät sanat sanakirjasta salasanojen arvaamiseksi esim. vuodatettujen salasanojen tai vastaavista kirjastoista. 

  #### Miten asennetaan?

-  `sudo apt-get hashid hashcat`

    hashid tunnistaa, mitä salausmenetelmää salasanan salaamiseen käytetään ja hashcat käytetään niiden murtamiseen.

- asennetaan sanakirjasto esim. Rockyou, joka sisältää 14 miljoonaa eri sanaa.

 #### Miten käytetään?

-  Tunnistetaan salaustyyppi hashid:lla:

    `hashid -m 99083j54fhjg94584cc45`

    -m lippu kertoo Hashcat moodinumeron mikä käytetään murtamisessa.

-  Salaukset murtaminen:

    `hashcat -m 0 '99083j54fhjg94584cc45' rockyou.txt -o solved`

    '-m 0' lippu asettaa murtamiseen käytetty moodi, minkä saatiin hashid:lta
   
    '-o solved' lippu tallentaa ratkaisut erilliselle tiedostolle nimeltään 'solved' nykyiselle polulle.

    Jos ratkaisua jätetään tallentamatta, voidaan käyttää '--show' parametriä:

    `hashcat -m 0 '99083j54fhjg94584cc45' rockyou.txt --show`

### Karvinen 2023: Crack File Password With John

  #### Mikä John?

-  John the Ripper, Brute-force työkalu, joka voidaan käyttää murtamaan salasanasuojattuja tiedostoja.
-  Tässä opetetaan käyttämään 'Jumbo' versio ja käyttämään sen murtamaan .zip tiedoston salasanaa.

  #### Miten asennetaan?

-  Tarvittavat lisätyökalut ja kirjastot:
    -  git, build-essential, libssl-dev, zlib1g, zlib1g-dev, zlib1g-gst, libbz2-1.0, libbz2-dev, atool zip

- John the Ripper, Jumbo version lataaminen:

    `git clone --depth=1 https://github.com/openwall/john.git`

-  Tiedostojen konfigurointi:

    `cd john/src/`
   
    `./configure`

    'configure' tunnistaa ympäristön ja luo seuraavan vaiheen 'make' komennolle Makefile tiedoston.
  
    Konfigurointivaiheessa saatta tulla virheitä, jotka viittaavat puuttuviin riippuvuuksiin (dependencies). Error-viestien     perusteella voidaan etsiä puuttuvat paketit 'apt-get' ja 'apt-cache search' komennoilla ja asentaa ne. Tämän jälkeen        ajetaan './configure' uudestaan.

-  Tiedostojen varsinainen koonti (compiling):

    `make -s clean && make -sj4`

   Ylläoleva 'make' komento saadaan aikaisemman onnistuneen konfigurointivaiheen outputista, joka ajetaan sen jälkeen.

-  John:in käynnistys

    Uudet ajettavat tiedostot ja scriptit pitäisi nyt löytyä '/john/run' hakemistosta

    `~/john/run/john`

   Ylläoleva komento pitäisi ajaa John ja näyttää sen versionumeroa yms.

  #### Miten käytetään?

- Ladataan testi .zip tiedosto, jonka salasanaa yritetään murtaa:

    `wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip`

Tämän avaaminen 'unzip tero.zip' komennolla ei onnistu, sillä meillä ei ole oikeaa salasanaa sen purkamiseen.

-  Puretaan ensin hash-tiedosto, joka John pystyy alkaa murtamaan:

    `~/john/run/zip2john tero.zip > tero.zip.hash`

- Ajetaan john:ia murtamaan .hash tiedostossa olevaa salattua avainta

    `~/john/run/john tero.zip.hash`

  Mitä pidempi/vaikeampi salaus, sen kauemmin sen murtamiseen menee. Kun john suorittaa murtauksen loppuun, saadaan       
  ulostuloon salattu avain luettavassa tekstimuodossa ja pystytään tätä käyttämään 'tero.zip' tiedoston purkamiseen.

# a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.

## Luodaan esimerkkitiedosto

  Aloitin luomalla tiedoston 'salainen.txt' ja salaamalla sen MD5 menetelmällä käyttämällä 'md5sum':

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/53fa6dc1-d718-43e6-939e-0077d8a949c6)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/aca54769-c3e3-4819-b933-df120ee4f85a)

  Salattu teksti on nyt 'salattu.txt' tiedostossa 

## Hashcat ja kirjastojen asennus

  Seuraavaksi asensin 'hashid' ja 'hashcat', sekä 'Rockyou' kirjaston

  ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/c9c0f1fc-f447-4410-97fc-cf68605a8a3a)

  Tässä tapauksessa Kali koneella on jo valmiiksi asennettu molemmat paketit.

  Ladataan rockyou kirjasto ja puretaan:

  ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/ae680229-2797-4f0c-a4d5-7b957628bd00)

  ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/20639d9d-4571-4673-b206-490819e852cc)
  
## Salasanan murtaminen

  Nyt kun on tarvittavat paketit ja tiedostot, aloitetaan tunnistamalla hash menetelmä 'hashid':lla meidän 'salattu.txt' tiedostossa olevaa hash:ia.

  ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/59110513-527c-4dcc-b27a-f758610b1eb7)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/4d17fd70-6da7-4f7a-a5a5-4578b1d38306)
  
hashid palautti aika monta salausmenetelmää, mitkä ovat mahdollisesti käytetty tämän salasanan salaamiseen. Tiedetään kuitenkin, että tässä on käytetty MD5 salausmenetelmää, joten käytetään se.

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/f77a9b04-a9db-4dc3-befd-ba9704537338)

Ajoin komennon joka aloittaa murtamisen ja tallentaa tuloksen 'ratkaistu.txt' tiedostoon, mutta hetken päästä antoi errorin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/29f57dab-bdec-48ea-a7ee-d990e931f663)

Hetken googlettelun jälkeen, tämä johtuu tod.näk. liian vähäisestä RAM:ista, minkä olen antanut VBox Kalilleni. Vaihdoin sen RAM allokaation 2GB -> 4GB ja se korjasi ongelman!

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/7a94c476-03fb-4e8d-bc4f-8ed7af2320aa)

Ajoin komennon uudestaan, mutta se ei onnistunut murtamaan salasanaa:

ls![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/6ef4be77-ed07-4fa0-a7e3-a28a8d37f082)

Ilmeisesti luomani salasana 'horseshoe123' ei ollut rockyou sanalistan joukossa.

Yritetään uudestaan luomalla "helpompi" salasana ja ajamalla hashcat uudestaan:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/e0d7d273-183d-428a-9362-cae591365429)

Tulokset näyttävät silti, että salasanaa ei löytynyt:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/1264b8f4-759b-4a67-af66-b4fba4a7c2e0)

Tarkistin, että 'iloveyou' löytyy kuitenkin 'rockyou.txt' sanalistasta. En tiedä johtuuko se väärän moodin käytöstä vai onko md5sum menetelmän käyttö liian vahva hashcatille...

# b) Salainen, mutta ei multa. Ratkaise dirfuzt-1 artikkelista Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf

## Esimerkkitiedoston lataus

Aloitin lataamalla 'dirfuzt-1' esimerkkitiedoston:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/703c4f53-e659-44a1-98f4-019c081870db)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/ad0881ca-dfd2-4dbd-a781-06e170c1630d)

Jätin terminaalin pyörimään taustalle ja avasin selaimeen:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/d4b79a3d-5aa8-4b54-94c8-ae57712c856a)

## Dictionary 

Seuraavassa vaiheessa hyppään suoraan sanakirjan lataukseeb yleisimmistä verkkopoluista, sillä minulla on jo ffuf esiasennettuna Kaliin.

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/4bc38561-584a-4eee-a333-73df1007b9b5)

Nyt on sanakirja ladattu:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/0a3eb46a-14c8-4a9f-9014-f093a2761d3d)

## Ffufin käyttö

Vaihdoin ensin Kalilta 'Host-only' verkkoon, ettei paketit vuoda nettiin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/a7d835e5-c980-4b57-b295-3a5e42530c1b)

Testasin myös, ettei Kalilla ole yhteyttä nettiin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/5cb27499-da8a-40d0-b84a-764b44966929)

Nyt voidaan aloittaa turvallisesti ffuf:in käyttöä:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/4c047cba-8d75-4b9f-9a56-c5cbf165fbe0)

Satunnainen kuva prosessilistasta:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/419549f4-e379-40c8-9dbc-9e7e5f51ba39)

Vaikuttaa siltä, että yleisimmät vastaukset ovat kooltaan 154 tavua, 9 sanaa pitkiä ja 10 riviä pitkiä.

Suodatetaan tuloksen koon mukaan:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/cf1eac9f-90e7-4a36-ae42-4ee19d760445)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/e935567e-254a-4023-8077-cd1b7deb77be)

Nyt näyttää lupaavalta, suodatettiin 154 tavuiset vastaukset pois ja löysimme 7 polkua, josta yksi on **wp-admin**

Tarkistetaan se 'curl' toiminnolla:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/260465f0-af86-47bb-95f0-539ab766b106)

Löysimme salaisen sivun!

# c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana.

## Ladataan riippuvuudet ja John Jumbo version, sekä koostetaan tiedostot

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/88bd007e-01dc-44b7-ac15-3e0f3f761f45)

apt-get ei ilmeisesti löytänyt 'zlib-get' pakettia, toivotaan että toimii ilman ja palataan tähän, mikäli ei toimi.

Kloonataan John repo:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/1aea0ff8-d16b-4a52-bfab-331431074a40)

Konfigurointi:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/17ba5d60-4ddd-48ed-9a3d-d3be5ef0df1c)

konfigurointi palautti virheen, ettei openSSL headereita löytynyt:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/1ade761f-8783-4e51-bc40-fa22aecbc117)

Ajoin sen uudestaan '--without-openssl' vaihtoehdolla ja se meni läpi:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/e59cecb9-0f50-476a-b51c-4584ac2112af)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/d80c0292-a434-4675-9302-641206cfd7d2)

Compile:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/96e3891a-261c-43db-bb97-8cc3984ef9aa)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/1e1399a1-bab3-44dc-9307-fb7b7a0fb0d7)


Run hakemisto: 

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/a952dc49-2037-4cb0-b436-7ca3009e3894)

Run john:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/92035ecb-c8e7-49db-8f42-e62c9d4d46b6)

## Ladataan testitiedosto tero.zip

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/361a8228-8aa8-4d7a-8e89-52223e406691)

Yritin purkaa tiedoston, mutta on salasanasuojattu:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/24303497-bac3-434c-9467-4d4c2e08de20)

## Cracking ZIP password

Puretaan ensin salaus erilliselle .hash tiedostolle:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/dc92e720-f2aa-49cf-a58f-0fe8ba3a8897)

Ajetaan john .hash tiedostolle:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/56f8d0f8-83d4-4221-9c1d-b2bba3b041e8)

Löydettiin salasana 'butterfly', käytetään tätä .zip tiedoston purkamisessa:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/6c869bdc-5dd1-41bc-bc3b-65b463aa95f9)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/df16e017-82cf-464d-9751-872264a8bd0d)

Saatiin SECRET.md tiedoston sisältö auki!

# d) Fuffme. Asenna Ffufme harjoitusmaali paikallisesti omalle koneellesi. Ratkaise tehtävät (kaikki paitsi ei "Content Discovery - Pipes")

## Testiympäristön konffaus:

Asennetaan Kali:lle docker.io:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/db0b20c9-8dac-46b5-b27d-f785198b6ed8)

Kloonataan ffufme-maalikoneen repo:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/c95d1a5a-965e-4533-ad25-32120eb7720d)

Rakennetaan ffufme kone dockeriin:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/3d185d5d-6080-457a-8680-c308053ca031)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/d54e586e-57e4-43e0-90bb-4ef3084b5d72)

Käynnistetään ffufme maalikone:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/19a44f6a-8789-4039-8b27-1ee3ced93899)

Tarkistin, että ffuf.me toimii localhost portilla 80:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/e224007c-6fd1-4289-bc0a-e666b76dd203)

Ladataan wordlistit:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/d0ee358d-5f64-40aa-be8c-72751985bbbb)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/946d2324-188f-4b55-ab9b-0de4640729a9)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/31a4ed1b-38a5-4b82-bf62-3299e2299704)

Nyt on kaikki tarvittavat asennettu ja ladattuna. Kytkin vielä 'Host-only' verkkomoodin Kalille ennen harjoituksien aloittamista

## Harjoitukset

### Content Discovery

**Basic:** 


![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/b83af460-ae97-4fdc-a315-be9651a30305)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/1defdb13-5a46-4693-a486-03e9add92b3b)

Löydettiin 'class' ja 'development.log' tiedostot

**Recursion:** 

-recursion lippu käskee fuzzerin tekemään uuden haun löytämäänsä hakemiston sisälle niin kauan, kun se pääsee hakemiston loppuun. Tässä tapauksessa /admin -> /admin/users/ -> /admin/users/96

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/7d126042-9c56-4717-9fe2-89d0e4dea82b)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/9ca5fce4-4157-4611-944c-8fd5aa30bf7a)

**File extensions:**

-e lipulla voidaan yksilöidä tiedostotyyppiä, tässä tapauksessa etsittiin .log tiedostoja hakemistosta /users/users.log

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/5fe4b600-1e04-4ab8-8b98-d9546d180347)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/e451666b-6232-4ab7-a243-461831c2b113)

**No 404 status:**

Tässä etsittiin "Page can not be found" sivua, mutta tuloksia tuli aika paljon.  

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/2ee938f4-7ff2-44f0-99a9-b1fe1945d93a)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/27d61203-270c-4ee2-98be-62d7cbcbe061)

Yleisin koko sivuille on 669 tavua, joten suodatetaan -fs 669 lipulla sen kokoiset sivut pois:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/90109ec0-0628-40fd-b48e-156f176c0065)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/59a78314-2685-4682-ab7e-bd731773fd4c)

Nyt saatiin yksilöity joukosta 'secret' niminen sivu.

**Param Mining:**

Vieraillaan sivulla /cd/param/data, joka palauttaa oheisen virheen:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/2b4a5a76-4712-4a24-8710-5bd3e8188603)

Tämä palauttaa HTTP 400 koodin 'Bad Request'. ffuf:lla yritetään etsiä oikea parametri:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/fc23f19e-586f-46d6-8bf2-551ab43b053b)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/cff03d45-6293-4f3b-a1ec-25d37efeb88d)

Selvisi, että 'debug' oli oikea parameti, jolloin URL:iin syötettynä sivu toimii:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/cff5159d-eacd-4caa-ab10-9b21462c55b0)

**Rate Limited:**

Testataan ffuf:ia oletusnopeudella etsimään sivuja -mc 200,429 lipulla, joka palauttaa vain HTTP status koodit 200 ja 429 sivuja:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/39aaa133-6f6e-4cd8-83b3-498d10ac541a)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/4bac69eb-0e4f-4ac4-aee2-b0e3535ea1d0)


Vastaukset palauttivat pelkästään Status: 429 sivuja, joka tarkoittaa että pyyntöjä ovat tilapäisesti estetty niiden suuren määrän vuoksi. 

Seuraavaksi rajoitetaan pyynnöt käyttämällä `-p 0.1` lippua, joka pysähtyy pyyntöjen välissä 0.1 sekunttia, sekä `-t 5`
lippua, joka luo 5 eri ffuf prosessia, jolloin pyyntöjä tehdään max 50 per sekuntti, jolloin palvelin ei ylikuormitu.

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/3d6f6a42-0bc7-427c-bfd4-6bb86a476ad8)


![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/4f3ea3af-41b6-4dd2-93f3-1415ddcb29cd)

Nyt löydettiin HTTP status 200 vastaus, joka on 'oracle'.

### Subdomains

**Virtual Host Enumeration**

ffuf voidaan käyttää myös löytämään aliverkkoja

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/b9586e85-8928-43d7-9eaa-d4061a56dcee)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/63bb29bb-72c3-420b-8193-493647560610)

Yhtenäistä näissä on koko: 1495, sanat: 230, rivit: 40. Suodatetaan ne koon mukaan:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/aadd3e51-a67e-4f4e-ad86-9ec90bb40169)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/89ee76be-3f80-4c93-8d70-fa4d2ed968ce)

Löydettiin aliverkko nimeltään 'redhat', joka poikkesi muista aliverkoista.
  

# e) Tee msfvenom-työkalulla haittaohjelma, joka soittaa kotiin (reverse shell). Ota yhteys vastaan metasploitin multi/handler -työkalulla.

# f) Asenna Windows virtuaalikoneeseen. Voi olla esimerkiksi Metasploitable 3 tai Microsoftin sivuilta saatava ilmainen kokeiluversio.

# g) Ota Windowsiin graafinen etähallintayhteys Linuxista. Käytä RDP:tä eli Remote Desktop Protocol.
















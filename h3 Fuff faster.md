# Lue ja tiivistä

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

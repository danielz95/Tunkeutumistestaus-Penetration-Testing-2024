# h2 Scanning Cyberspace

Tässä tehtävässä perehdytään tarkemmin porttiskannaustekniikkoihin ja niiden testaamista hyödyntäen Metasploitable 2-testiympäristöä.

## Lue/katso ja tiivistä

### Port Scanning basics

https://nmap.org/book/man-port-scanning-basics.html

Nmap tunnistaa 6 erilaista porttitilaa:

- **Open** : Jonkun applikaation käytössä oleva portti joka vastaanottaa TCP, UDP, SCTP paketteja. Yleensä porttiskannauksien päätavoite löytää näitä.

- **Closed** : Saavutettavissa, eli host IP ylhäällä, muttei portti ole minkään applikaation käytössä.

- **Filtered** : Ei voida todentaa, onko portti auki vai kiinni, koska sen skannausyritykset suodatetaan tulimuurin johdosta. Hidastaa huomattavasti skannausyrityksiä.

- **Unfiltered** : Portti on saavutettavissa, muttei tiedetä onko se auki vai kiinni. ACK skannauksen lisäksi Window, SYN tai FIN skannaukset saattavat selvittää sen tilaa tarkemmin.

- **Open|filtered** : Ei varmuutta, onko portti 'open' vai 'filtered' tilassa. Yleensä tämä tilaluokitus tulee kun avoin portti ei anna vastausta. UDP, IP, FIN, NULL, Xmas skannaukset luokittelevat portit näin.

- **Closed|filtered** : Ei varmuutta, onko portti 'closed' vai 'filtered' tilassa. IP ID idle skannauksen luokitus.

### Port Scanning Techniques

https://nmap.org/book/man-port-scanning-techniques.html

- **-sS (TCP SYN scan)** : Oletus ja suosituin skannaus. Nopea, eikä tulimuurit hidasteena. Huomaamattomampi, sillä ei suorita TCP yhteyksiä loppuun. Palauttaa 'open', 'closed' tai 'filtered' tuloksia.

- **-sT (TCP connect scan)** : Oletus, jos SYN skannaus ei ole vaihtoehtona. Raakapakettien lähetys/vastaanoton sijaan, Nmap pyytää yhteyttä kohdeporttiin Berkley Sockets API:n kautta. SYN skannaus parempi vaihtoehto, sillä TCP yhteys on hitaampi ja helposti havaittavissa (IDS, logit).

- **-sU (UDP scans)** : UDP porttiskannaus. Esim. DNS, SNMP, DHCP käyttää UDP:ta. Toimii lähettämällä kohdeportteihin UDP paketteja. 'open|filtered' tulokset saattaa selvitä Version detection (-sV) skannauksella. UDP skannauksen suurin haaste on sen hitaus, pakettirajoitukset voivat olla 1 per sekunti, eli 65536 porttia kestää 18 tuntia. Suositellaan ensin nopea skannaus kaikista porteista ja yksilöimään suositut portit UDP skannaukselle.

- **-sY (SCTP INIT scan)** : Uudempi protokolla joka yhdistää TCP ja UDP ominaisuudet. SCTP INIT skannaus toimii samalla periaatteella kun TCP SYN skannaus.

- **-sN, -sF, -sX (TCP NULL, FIN, Xmas scans)** : Pyrkii hyödyntämään porsaanreiän erottaakseen 'open' ja 'closed' portteja. Voi ohittaa non-stateful tulimuureja ja pakettisuodatusta. Saattaa olla huomaamattomampi kuin SYN skannaus. Haittoina IDS:n havainnointi, kaikki portit näyttää 'closed' tilaa, eikä pysty tiettyjä portteja erottamaan open|filtered tilasta.

- **-sA (TCP ACK scan)** : Käyetään kartoittamaan tulimuurien tilaa, sääntöjä ja mitkä portit ovat suodatettuja. Ei tarkista avoimia portteja.

- **-sW (TCP Window scan)** : Sama kuin ACK scan, mutta hyödyntää systeemin iplementointiin liittyvää haavoittuvuutta (TCP Window value) arvioimaan porttien tiloja. Ei voida pitää aina luotettavana, koska riippuvainen internetissä julkaistuista systeemien implementaatiotiedoista.

- **-sM (Maimon scan)** : Sama kuin NULL, FIN, Xmas skannaukset. Erona FIN/ACK kyselyn käyttö, joka droppaa pakettia kun portti on auki tilassa.

- **--scanflags (Custom TCP Scan)** : Räätälöity TCP skannaus. esim. URG,ACK,PSH,RST,SYN,FIN voidaan käyttää kaikki lisäyksellä --scanflags URGACKPSHRSTSYNFIN.

- **-sZ (SCTP COOKIE ECHO scan)** : Edistyksellisempi SCTP skannaus. Huomaamattomampi kuin INIT skannaus piilottamalla COOKIE ECHO sisältäviä paketteja. Hyvä IDS voi silti havaita, eikä skannaus pysty erottamaan 'open' ja 'filtered' porttien eroa. Palauttaa aina 'open|filtered'

- **-sI <'zombie host'>[:<'probeport'>] (idle scan)**: Porttiskannaus zombie koneelta AKA Proxy skannaus. Luotettujen zombien kautta tehty skannaus voi paljastaa paljonkin.

- **-sO (IP Protocol scan)** : Arvioi mitä IP protokollia kohdekoneet tukevat.

- **-b <'FTP relay host'> (FTP bounce scan)** : Voidaan hyödyntää FTP-palvelinta porttiskannaamaan kohdekoneet meidän puolesta. Hyvä tulimuurien ohitustekniikka. Laajasti korjattu haavoittuvuus, mutta voi kokeilla jos mikään muu ei toimi.

  
## Asenna Metasploitable 2 virtuaalikoneeseen

Aloitin menemällä : https://sourceforge.net/projects/metasploitable/files/Metasploitable2/

Hyödynsin asennusohjeita VirtualBoxille täältä: https://www.geeksforgeeks.org/how-to-install-metasploitable-2-in-virtualbox/

Latasin Metasploitable2 .zip tiedoston ja purin tiedoston. Tämän jälkeen avasin VirtualBox:in (Versio 7.0.6) Windows 11- Host koneellani. Loin uuden virtuaalikoneen alla olevilla tiedoilla:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/cb0e3f36-4509-4389-9dce-0e51461a2bb1)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/673b59f2-ee42-45c8-acd0-c3748b680b46)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/0b691165-8376-46a5-b56d-a35c7043115d)

Kun sain konffaukset valmiiksi, käynnistin Metasploitable 2 ja kirjauduin oletustunnuksilla 
- User: msfadmin
- PW : msfadmin

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/6c7de0d8-1f06-4975-b120-20a4ef93f7ee)

#### Ohitin Kalin asennusvaihetta, sillä minulla oli jo Kali valmiiksi asennettuna virtuaalikoneeseen edellisestä tehtävää tehdessäni

### Verkkoasetuksien määrittely Kali ja Metasploitable 2:lle

Kirjautumisen jälkeen muutin verkkoasetukset molemmille VM:lle seuraavasti:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/6f1d0c86-00cd-48ea-9365-60cb2fa08fe6)

Testasin, ettei VM koneet ovat eristetty internetistä:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/c7923b4a-f3fc-47c5-a0bf-b4b5908a81e6)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/e663c20a-52a9-4cca-90ab-78892e1d1fee)

Tarkistin Metasploitable:n ip-konfiguraatiot:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/d04faa9d-7159-4a26-b654-2e31f630852a)


## Testiyhteys ja porttiskannaukset


Testasin ottaa yhteyttä Kali -> Metasploitable 2:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/0ddabaa3-34b5-4584-a5b3-1ac8fca61842)

Tästä löysin hyödylliset ohjeet metasploitin käytöstä: https://learning.oreilly.com/library/view/mastering-metasploit/9781838980078/B15076_01_Final_ASB_ePub.xhtml#_idParaDest-29

Seuraavaksi käynnistin Metasploitin Kalilla:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/96a14a44-acaf-4fda-9111-2b4819e05b3f)


Tässä käynnistettiin Metasploit Frameworkin Postgre database, sekä Metasploit konsolin, josta voidaan tehdä **db_nmap** skannauksia.


Avasin Kali:lla Wiresharkin ja asetin **eth0** kaapaamaan liikennettä: 

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/98e5953f-9623-4166-98f5-92bf22e0af32)

Suoritin db_nmap skannauksen ja kaappasin pakettiliikenteen:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/63f58e27-411b-4603-89e3-c82363548f7c)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/88d4fe88-2cea-4aa7-8bc9-f0f78b622d3d)

Tuloksena oli TCP ja ARP pakettien vaihtoa koneiden välillä.

### Huolellinen skannaus

Seuraavaksi tehdään huolellinen skannaus Metasploitablen koneesta:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/c0ca240b-daf7-45f5-8c65-0b8f0fc8aa0e)

Skannauksen tulokset paljastivat hyvin monta avointa porttia, mikä vaikuttaa hyvin huloestuttavalta. Palvelut kuten telnet, exec, login, shell voivat mielestäni toimia backdoor tunkeutumisen menetelminä.

Wireshark tulokset:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/b81ec99e-e5e9-4bf7-8b4d-def44cafe683)

Wiresharkin mukaan skannausn tuotti yhteensä 131 146 TCP pakettia. Paketeista huomasin, että avoimet portit jäivät ACK tilaan ja suljetut portit menivät RST,ACK tilaan.

### Skannauksen tuloksen tallennus

Tässä tallennetaan skannauksen tulos -oA optioilla, joka tallentaa sitä .gnmap, .nmap ja .xml muodoissa.

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/93c9792a-c268-40c5-802e-3b576d82f5eb)
Tässä kuvassa on esitetty .nmap formaatin logi, mikä oli selkein formaatti luettavuudeltaan konsolissa.

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/a30d4b64-c017-4a41-aeba-ded4bb3b3544)

### Lokin luominen script työkalulla

Tässä loin 'log001.txt' tiedoston:

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/626a022e-a21b-4dd5-9469-0474cd23b023)

### Grep:in käyttö

Tässä etsin kaikista mahdollisista paikoista (logit,tiedostot yms.), missä mainitaan '192.168.56.102' eli Metasploitable koneen IP osoitetta:

    grep -ir 192.168.56.102

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/5e295d8a-ed71-48d3-b269-04ca82c8f0da)


Nämä tallentuvat siis log001.txt tiedostoon.

### Nmap runtime interactions

Ajankohtaiset toiminnot selitetään hyvin tässä: https://nmap.org/book/man-runtime-interaction.html

nmap skannauksen aikana voidaan kytkeä tietyillä näppäimistön syötteellä tiettyjä toimintoja esim. pakettienjäljitys päälle painamalla 'p' näppäintä ja pois päältä 'P' (Shift + p) näppäinyhdistelmällä. Periaatteena on, että pieni kirjain lisää ja iso kirjain vähentää tiettyä toimintoa.

## Lähteet

Karvinen T., h2 Scanning Cyberspace, https://terokarvinen.com/2024/eettinen-hakkerointi-2024/

nmap.org, port scanning basics, https://nmap.org/book/man-port-scanning-basics.html

nmap.org, port scanning techniques, https://nmap.org/book/man-port-scanning-techniques.html

sourceforge/rapid7user, metasploitable 2 download, https://sourceforge.net/projects/metasploitable/files/Metasploitable2/

lucifer2411, Metasploitable VirtualBox installation guide, https://www.geeksforgeeks.org/how-to-install-metasploitable-2-in-virtualbox/

Jaswal N., Mastering metasploit, https://learning.oreilly.com/library/view/mastering-metasploit/9781838980078/B15076_01_Final_ASB_ePub.xhtml#_idParaDest-29

nmap.org, nmap runtime interactions,  https://nmap.org/book/man-runtime-interaction.html




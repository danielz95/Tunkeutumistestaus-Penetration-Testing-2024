# h2 Scanning Cyberspace

Tässä tehtävässä 

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

- 

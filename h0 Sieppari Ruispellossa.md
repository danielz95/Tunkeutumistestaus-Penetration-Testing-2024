# Tässä tehtävässä siepataan ja analysoidaan verkkoliikennettä

Olen käyttänyt Firefox-selaimen Network Monitor/verkkomonitorointityökalua (Ctrl+Shift+E)

![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/7359ed6e-b0a0-465d-bc5c-d60fd1780914)

Tässä kaappauksessa olen ladannut verkkosivun www.youtube.com. Verkkoliikenteen osiot selitettynä:

### Status
Tämä kertoo liikenteen tilaa kerrottuna numerokoodilla.
### Method
Tässä kerrotaan verkkoliikenteen HTTP metodia (GET/POST). GET metodi lähettää pyynnön palvelimelta hakeakseen dataa tietyltä resurssilta. 
POST vuorostaan lähettää dataa tiettyyn resurssiin päivittääkseen sitä.
### Domain
Domain kertoo, mistä resurssi on peräisin.
### File
File kertoo tiedoston nimeä, mitä on haettu.
### Initiator
Initiator on tiedosto tai resurssi, joka aloittaa verkkotapahtuman.
### Type
Tämä tarkoittaa tiedoston tyyppiä, kuten html/js tiedostot ja png kuvatiedostot
### Transferred
Tämä kertoo tiedoston lähetysmäärän kompressoituna
### Size
Tämä kertoo tiedoston todellisen koon
### Latency/ms
Tämä kertoo, kuinka kauan tiedoston lähetyksessä kesti millisekuntteina

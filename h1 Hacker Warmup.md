# h1 Hacker Warmup
Tässä tehtävässä lämmitellään Tero Karvisen pitämää tunkeutumistestauskurssia varten tekemällä tunkeutumiseen liittyvät harjoitukset.

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
   
   **Pelin sääntöjä kunnioittaen, en julkaise tasojen ratkaisuja, vaan pelkät lopputulokset:**

   ![image](https://github.com/danielz95/Tunkeutumistestaus-Penetration-Testing-2024/assets/128583292/1aaa58c3-3b9c-45c6-aefb-8b9060c9a4de)

### Level 0 -> Level 1: Etsi salasana readme-tiedostosta päästääkseen Level 1:seen

Tehtävänä etsii kotihakemistosta readme-tiedosto, josta löytyy salasana päästääkseen Level 1 - tasolle

Löydettyäni salasanan, kirjauduin taas peliin SSH:lla **bandit1** Username:lla ja uudella salasanalla.

### Level 1: Etsi salasana '-' nimisestä tiedostosta päästääkseen Level 2:seen.

Tässä tehtävässä käytännössä sama periaate kuin edellisessä, mutta tiedoston avaaminen/lukeminen vaatii hieman erilaista komentoa ;).


## WebGoatin asennus, sekä "HTTP Basics" ja "Developer Tools" tehtävät

## PortSwigger Labs: SQL injections

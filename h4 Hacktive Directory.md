# Lue/Katso ja tiivistä

Valitsin tähän tutkittavaksi Nikita Ponomarevin esityksen materiaaleista PowerView työkalu, joka on osana PowerSploitMafian ylläpitämää PowerSploit työkalukokoelmaa.

Linkki sivulle:
-  PowerSploit: https://github.com/PowerShellMafia/PowerSploit
-  PowerView: https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon

### Mikä PowerView

-  On PowerShell työkalu, jolla autetaan käyttäjää hahmoittamaan verkkoympäristöä paremmin Windows domaineilla.
-  Korvaa Windowsin 'net *' komentoja, joka käyttää PowerShell AD koukkuja ja sen pohjalla olevia Win32 API toimintoja.
-  Suorittaa Windows Domain funktioita.
-  Pystyy mm.
      -  Tunnistamaan mihin verkkoihin mitkäkin käyttäjät ovat kirjautuneena
      -  Tarkistamaan millä domainin koneella nykyisellä käyttäjällä on Admin-oikeudet
      -  Keräämään (ennumerate) doimainiin liittyvää dataa ja hyväksikäyttämään sen luottamukselliset funktiot

### Funktiot

  **Domain/LDAP Functions**

Näillä komennoilla pystytään keräämään tietoja Windows AD:n DNS, Controllerit, Käyttäjät, Objektit, OU:t, ryhmät.

  Esimerkkejä:

-  `Get-Domain` : Palauttaa nykyisen tai erikseen määritellyn domainin objekteja
-  `Set-DomainObject` : Muokkaa Domain objektiin liittyviä ominaisuuksia
-  `Find-InterestingDomainAcl` : Etsii objekti ACL:iä joilla on muokkausoikeuksia muulla kuin built-in objekteilla

  **GPO Functions**

Group Policy Objecteihin liittyviä hakufunktioita

  Esimerkkejä:

-  `Get-DomainGPO`: Palauttaa kaikki GPO:t tai valittuja GPO:ita AD:ssa.
-  `Get-DomainGPOUserLocalGroupMapping`: Kerää tietoa tetyistä domain user/groupeista jotka ovat tiettyjä local groupin jäseniä käyttäen GPO korrelaatiota.

**Computer Ennumeration Functions**

Näillä pystytään keräämään tietoja tietokoneesta ja sen yhteyksistä

Esimerkkejä:

-  `Get-NetLocalGroup`: Kerää tietoja localgroupista paikallisista tai etäkoneista.
-  `Test-AdminAccess`: Testaa, onko nykyisellä käyttäjällä Admin oikeudet paikalliseen tai etäkoneeseen.
-  `Find-InterestingFile`: Etsii tiedostoja jotka sopivat asetettuihin hakukriteereihin. Hyödynnettiin tätä seuraavan tehtävän tekemisessä ;)

**Threaded 'Meta'-Functions**

Enimmäkseen Domain kohtaisia hakufunktioita

Esimerkkejä:

- `Find-DomainUserLocation`: Etsii domain koneita, johon tietyt käyttäjät ovat kirjautuneet
- `Find-InterestingDomainShareFile`: Etsii tiedostoja jotka sopivat asetettuihin hakukriteereihin luettavista jakokohteista domainilla.
- `Find-LocalAdminAccess`: Etsii koneita domainilla, josta löytyy kirjautuneita käyttäjiä, jolla on local Admin oikeuksia.

**Domain Trust Functions**

Domain Trust / Luottamussuhteisiin liittyvät funktiot

Esimerkkejä:

-  `Get-DomainTrust`: Palauttaa kaikki domain luottamussuhteet nykyiselle tai valitulle domainille
-  `Get-DomainTrustMapping`: Kerää kaikki luottamussuhdetiedot nykyisestä domainista ja myös jokaiselle domaineille joka löydetään.


# a) Ratkaise Nikitan / WithSecuren Windows-haaste. Älä julkaise läpikävelyohjetta, myöskään itse kirjoittamaasi. (Update: Tämä on nyt siis kerrankin sellainen tehtävä, johon voi vastata vain "tehty").

## Tilanne 24.4.2024 Klo 22:54

Sain tehtyä tähän menneessä 4/6 WithSecure Playground labran tehtävistä.

Jatkan tekemistä ennen kuin labra sulkeutuu Sunnuntaina 28.4.2024

## Tilanne 26.4.2024 Klo 15:45

Sain tehtyä 5/6 tehtävistä.

### klo 17:03
En saanut tehtyä viimeistä tehtävää, sillä minulla oli ongelmia "Rubeus.exe":n suorittamisessa ja ticketer.py:n suorittamisessa. 

#Lähteet

PowerView, https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon

Ponomarev N., Attacking windows and active directory, Luento 22.4.2024



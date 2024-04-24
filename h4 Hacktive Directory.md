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


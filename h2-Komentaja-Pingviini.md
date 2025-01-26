# h2 - Komentaja Pingviini

Käyttöympäristö avattu edellisen kotitehtävän [raportissa](https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md). Debian-virtuaalikoneen asennus on sama kuin siinä.

## Artikkelin tiivistelmä

- Artikkeli käsittelee Linuxin komentokehotteen peruskäyttöä, `pwd`, `ls` ja `cd` -komennot ensiksi.
- Artikkeli esittelee tiedostojen muokkauksen ja kansiorakenteen hallinnan perusteet, sekä SSH-etäyhteyden luomisen perusteet, oletettavasti tulevaa palvelimen hallintaa varten.
- Lisäksi artikkeli esittelee `sudo`-komennon korotettuihin käyttöoikeuksiin ja sen käyttöä paketinhallinnan peruskomentojen kanssa, demonstroiden Nethack-pelin asennuksen ja poiston Linux-koneelta.
- Ihan kaikkia tunnilla käytettyjä komentoja (kuten grep) ei ole listattu, mikä olisi auttanut muistamaan niitä paremmin.

## a. Micro asennus

Ensimmäinen yritys `sudo apt-get install Micro` tuottaa virheilmoituksen `E: Unable to locate package Micro`. Kirjaisinkoko on siis merkitsevä, asennus ei toimi isolla alkukirjaimella.

Toinen yritys `sudo apt-get install micro` toimii ja sovellus asentuu virtuaalikoneelle.

## b. CLI-sovellusten asennus

Olisi tosi kiva jos tähän olisi joku lista hyödyllisistä sovelluksista joita apt-get -repositoriosta löytyy, itselle tämä on vielä ihan musta aukko. Julkisesta internetistä etsimällä tulee kaikenlaista kivaa mitä apt-get -komennolla ei saa asennettua ja `apt-get cache search` -komennolla hukkuu runsaudentulvaan, eikä ihan randomeita haluaisi asentaa.

Komento: `sudo apt-get install sl ncdu ddgr`

![Asennus onnistui](https://github.com/user-attachments/assets/b181e1d2-646d-4b9f-bed6-e73e10928a6a)

Steam locomotive, eli sl, on sovellus joka opettaa kirjoittamaan `ls`-komentoja oikein ja samalla parantaa oikean ja vasemman käden välistä koordinaatiota. 

![sl toiminnassa](https://github.com/user-attachments/assets/b1d33a42-c9c9-4a13-9206-cb67f8470ce4)

NCDU on työkalu kovalevyn asennusten hallintaan. Päänäkymässä se näyttää mitkä kansiot käyttävät eniten tilaa tietokoneen kovalevyllä.

![ncdu-kuva](https://github.com/user-attachments/assets/b7155f10-2ac5-44f1-b9c9-d213dee12add)

DDGR mahdollistaa Duckduckgo-selaimen käytön Linuxin terminaalissa.

![Turvallisia hakuja](https://github.com/user-attachments/assets/0c22337c-8cf1-4e00-b835-9a7384d66a8b)


## c. File Hierarchy System

**/** on Linuxin juurikansio, jonka alla kaikki tietokoneelle asennetut kansiot ja tiedostot sijaitsevat. Siihen pääsee `cd /`-komennolla. Alla kuvakaappaus juurikansion näkymästä `ls`-komennolla.

![juurikansio](https://github.com/user-attachments/assets/6e8ace24-e77f-4d8a-aec1-41bbe2f993b7)

**/home/** sisältää kaikki tietokoneen käyttäjätilit, joiden alle kaikki käyttäjien omat tiedostot tallentuvat. Omalla koneellani on vain yksi käyttäjä. Komennot: `cd home` + `ls`.

**/home/penskuj/** sisältää käyttäjän penskuj tiedostot. Kuvaava esimerkki sisällöstä on Documents-kansio, joka voisi sisältää erinäisiä käyttäjän luomia tiedostoja, mutta tässä tapauksessa viime tunnilla tehdyt harjoituskansiot ja -tiedostot. Nämä kaikki sivu-kerrallaan rullaavassa näkymässä saa komennoilla: `cd /home/penskuj/Documents` + `find|less`.

**/etc/** sisältää kaikki järjestelmänlaajuiset asetukset ihmisen luettavissa tekstidokumenteissa. `cd /etc` vie kansioon ja siellä näitä tiedostoja voi avata luettavaksi esim. `cat`-funktiolla tai eri sovelluksilla. Alla on näkymä Linux-version näyttävästä tiedostosta `micro debian_version`komennon jälkeen.

![debian versio](https://github.com/user-attachments/assets/0b8ff4c7-2710-4577-a2cf-e0752f2b4ae3)

**/media/** on kansio, jonne kaikki liitetyt ulkoiset mediat lisätään. Tänne pääsee `cd /media`-komennolla, mutta `ls`-komennon jälkeen huomaan kansion olevan tyhjä.

**/var/log/** sisältää järjestelmän tekemiä logitiedostoja. `cat README`-komennolla saa lisätietoja mm. siitä, että perinteinen tapahtumalogi ei tule tänne, vaan löytyy `journalctl`-komennolla.

## d. & e. Grep & Pipe

Grep on tehokas suodatustyökalu komentokehotteessa, ja pipe on eri komentoja yhdistävä työkalu. Nämä toimivat usein hyvin yhdessä, mutta kumpikaan ei tarvitse toista, tosin grepin käyttö itsekseen on aika epäkäytännöllistä.

Esimerkiksi jos haluan löytää kaikki kissa-sanan sisältävät tiedostot ja hakemistot ollessani /home/penskuj/Documents-kansiossa, voin antaa `find|grep kissa|less` komennon, ja se listaa viikonpäiväkansioissa sijaitsevat kissa.txt-dokumentit, sivu kerrallaan jos niitä olisi enemmän.

Grep toimii myös tiedostojen sisältöihin. Ohessa kuvakaappaus, jossa putkitetulla cat & grep-komennoilla haetaan tietyn sanan sisältävä tekstirivi/kappale useamman kappaleen mittaisesta Lorem Ipsum -markdown-tekstitiedostosta.
![grep tekstitiedostosta](https://github.com/user-attachments/assets/4392108d-b203-46bf-988a-cb50628b5406)

## f. Rauta

![Rauta kuvassa](https://github.com/user-attachments/assets/740a549c-e08e-4487-b79c-523a0da3097c)

Virtuaalikoneen järjestelmä tunnistaa olevansa VirtualBox-alustalla, sekä sille varatut muistin ja kovalevyn määrät. Virtuaalikone kykenee kuitenkin myös tunnistamaan fyysisen tietokoneen prosessorimallin. Samat asiat voi löytää edellisen tehtävän Debian-asennuksen raportista (paitsi VRAMia olen lisännyt jälkeenpäin).

Virtualisoidessa on näköjään huomioitava melko paljon tietokoneen komponentteja, mutta onneksi nämä vielä mahtuivat komentokehotteen yhdelle sivulle.

## Lähteet:

Karvinen, T. 2024. Linux Palvelimet 2025 alkukevät. Luettavissa: https://terokarvinen.com/linux-palvelimet/. Luettu 26.1.2025.

Karvinen, T. 2020. Command Line Basics Revisited. Luettavissa: https://terokarvinen.com/2020/command-line-basics-revisited/. Luettu 26.1.2025.

Pennanen, J. 2025. h1 Oma Linux. Luettavissa: https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md. Luettu 26.1.2025.

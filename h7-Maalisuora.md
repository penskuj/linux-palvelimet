# H7 Maalisuora

Linux-ympäristö on dokumentoitu ensimmäisen harjoitustehtävän [raportissa](https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md) (Pennanen, 2025).

Tämän raportti on tehty 10.3.2025 klo 15-17 välillä. Tehtävät tehty opettajan kotisivun tehtävänantoa seuraten (Karvinen, 2024).

## a) Hei Maailma

Tarvittavien koodien lähteenä käytetty opettajan sivujen Hello World -ohjeistusta (Karvinen,2018).

Asensin javan JRE-osion Borisov (2023) antaman ohjeistuksen mukaan komennolla `sudo apt-get install openjdk-17-jre`.

![image](https://github.com/user-attachments/assets/ad5a1dd3-375e-46c6-b7f4-5e2a9c64f942)


### Python

Tein `Hei_py-tiedoston, jonka sisään koodirivin print("Hei Maailma"). Se lähti toimimaan pythonilla varsin suoraviivaisesti:

![image-2](https://github.com/user-attachments/assets/e0acce65-9bf5-469b-80a6-186aee89648d)

### Java

Javaa varten tein uuden tiedoston Hei_java ja lisäsin sinne sopivan koodin. Tämän kanssa sain hetken miettiä, koska se ei lähtenyt toimimaan. Koska olin tehnyt ohjeen mukaan ja nimennyt java-luokan tiedostonimen mukaan, esimerkistä eri tavalla oli se, että tiedoston päätteestä puuttui .java, joten lisäsin sen, minkä jälkeen Hei Maailma -tulostus onnistui: 

![image-1](https://github.com/user-attachments/assets/1b3d2f0c-ec51-41bf-b8d8-e26502b0c2c6)

### Ruby

Tein lisäksi ruby-kielen mukaisen tiedoston Hei_ruby.rb, ja sinne koodirivin `print("Hei Maailma\n")`. Ensimmäisellä yrittämällä huomasin että Ruby ei ollut asennettuna. Asensin rubyn komennolla `sudo apt-get install ruby`. Arvasin ihan itse että tämä voisi toimia, ja se toimi:

![image-3](https://github.com/user-attachments/assets/376e0ac8-4f23-4572-8833-667217f2d2b7)

## b) Lähdeviitteet

Mielestäni olen laittanut käyttämäni lähteet paikalleen alusta alkaen. Jos ehdin tarkastaa vanhat raportit ja korjaan jotain viittauksia, pistän niihin aikaleiman.

## c) Uusi komento

Lähteenä omat muistiinpanot viime luennolta. Tein uutta omaa komentoa varten uuden tekstitiedoston `micro kello.sh` -komennolla. Päätin antaa sinne aikatulosteen ja uptime-komennon.

![image-4](https://github.com/user-attachments/assets/33baa6c7-e032-4c42-887f-f6f85fe5d4e8)

Seuraavaksi annoin kaikille käyttäjille execute-oikeudet tiedostoon ja poistin -sh päätteen, jotta se toimii nätimmällä kirjoitusasulla. Testasin että bash-komento toimii. Siirsin tiedoston `/usr/local/bin`-kansioon ja menin pois esimerkinomaiseen `/etc`-kansioon, jossa kokeilin että komento `kello` toimii myös sieltä käsin. En nyt alkanut kokeilemaan muilla käyttäjillä koska virtuaalikoneellani on vain yksi käyttäjä luotuna, koska näin jo aiemmin että execute-oikeus oli annettu other-ryhmälle.

![image-5](https://github.com/user-attachments/assets/f3a9ded8-ac99-48cc-944b-8ffb92e8b792)

## d) Laboratiorioharjoitus

Hakutoiminnolla ensimmäisenä tuli vuoden 2019 alkukevään laboratorioharjoitus, joka lisätty lähteisiin (Karvinen, 2019). Ensimmäistä komentoa yrittäessä, en saa paketinhallintaa löytämään Salt Minionia. 

![image-6](https://github.com/user-attachments/assets/c5c2b828-210d-450f-b5b8-38d04be09c61)

VMWare (2022-2025) kertoo Salt Minionin asennuksen alustukseen tarvittavat tiedot. 

![image-7](https://github.com/user-attachments/assets/41048bee-50c0-4428-853d-02b63b2963c8)

Ohjeistetut komennot antamalla saa Salt Minionin asennuksen onnistumaan. Tämän jälkeen ajan toivotut muut 3 komentoa:

![image-8](https://github.com/user-attachments/assets/ec9d627b-f0f1-434d-8e9a-81d3ab685fb6)

Tämän jälkeen ei kuitenkaan löydy toivottua tero.txt tiedostoa `/tmp/`-hakemistosta (tai toisestakaan mainitusta paiaksta):

![image-9](https://github.com/user-attachments/assets/eed81fd3-dcd3-4c5c-8e82-84a8b3b81163)

Palautuksen deadline tulee tässä kohtaa vastaan ja palautan harjoituksen tässä kohtaa.

## e) Uusi virtuaalikone

Deadline lähestyy niin palautan harjoituksen ja asentelen itselleni uuden virtuaali-linuxin myöhemmin illan aikana.

## Lähteet

Borisov, B. 2023. How to Install Java on Debian 12 (Bookworm). Luettavissa: https://linuxiac.com/how-to-install-java-on-debian-12-bookworm/. Luettu 10.3.2025.

Karvinen, T. 2018. Hello World Python3, Bash, C, C++, Go, Lua, Ruby, Java – Programming Languages on Ubuntu 18.04. Luettavissa: https://terokarvinen.com/2018/hello-python3-bash-c-c-go-lua-ruby-java-programming-languages-on-ubuntu-18-04/. Luettu 10.3.2025.

Karvinen, T. 2019. Arvioitava laboratorioharjoitus – Linux palvelimet ict4tn021-3004 ti – alkukevät 2019 – 5 op. Luettavissa: https://terokarvinen.com/2019/arvioitava-laboratorioharjoitus-linux-palvelimet-ict4tn021-3004-ti-alkukevat-2019-5-op/. Luettu 10.3.2025.

Karvinen, T. 2024. Linux Palvelimet 2025 alkukevät. Luettavissa: https://terokarvinen.com/linux-palvelimet/. Luettu 10.3.2025.

Pennanen, J. 2025. h1 Oma Linux. Luettavissa: https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md. Luettu 10.3.2025.

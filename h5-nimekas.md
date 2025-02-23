# h5 Nimekäs

Linux-ympäristö on dokumentoitu ensimmäisen harjoitustehtävän [raportissa](https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md) (Pennanen, 2025).

Tämän raportin verkkosivujen tekninen toteutus on tehty 17.2.2025 klo 18-21 välillä, raportin viimeistely ja DNS-tehtävä e) 23.2. 22-24 välillä. Tehtävät tehty opettajan kotisivun tehtävänantoa seuraten (Karvinen, 2024).

## a) Nimi

![image](https://github.com/user-attachments/assets/1c28eeac-3144-4267-9659-d2ec526a93aa)

Linkkaamalla Namecheapin ilmaispaketin GitHub Education -pakettiin joutuu antamaan aika käsittämättömät käyttöoikeudet Namecheapille. Tässä kohtaa hyväksyn nämä koska koulutehtävieni kaupallinen arvo on melko pyöreä nolla, mutta varmaan saan sulkea ja polttaa maan tasalle tämän GitHub-tilini kunhan valmistun. Kaiken lisäksi oikeuksien antamisen jälkeen selvisi, ettei Namecheap anna ilmaisia .me -domaineja kuin valittujen koulujen opiskelijoille, joten en päässyt etenemään. GitHubin options-valikon Applications-kohdasta pystyi poistamaan Namecheapin pyytämät oikeudet tiliini.

Seuraavksi yritin Name.com -sivuston kautta, joka on toinen GitHub Education -paketin kanssa yhteistyötä tekevä domain-palveluntarjoaja. 

![image-1](https://github.com/user-attachments/assets/6b788c6b-c877-490f-a767-104bcca57085)

Rekisteröitymisen jälkeen sain luotua ilmaisen penskuj.live -nettisivun Name.com kautta. 

![image-2](https://github.com/user-attachments/assets/f6fb6636-570c-4be2-b1e6-dc75a7318a61)

Seuraavaksi linkkasin DNS-asetuksiin oman hostatun verkkosivuni IP-osoitteen.

![image-3](https://github.com/user-attachments/assets/d1616a14-da47-4682-8ddc-42d1d91d58da)

Ei mennyt juuri hetkeäkään että aiemmin luomalleni testietusivulleni pääsi selaimen kautta, kunhan muisti vaihtaa protokollan https:stä http:ksi.

## b) Based

Name-based virtual hosting oli edellisen kotitehtävän lisäharjoituksena. Sitä kautta tämä oli jo kutakuinkin tehtynä. Olin kuitenkin käyttänyt `.com`-loppuista nimeä hakemistopuussa, joten kävin muuttamassa nimeämisen hankkimani `.live`-doimainin mukaiseksi:
- Hakemistopuun nimen sai muutettua `mv`-komennolla.
- `/etc/apache2/sites-available` hakemiston conf-tiedostoon tein alla olevan kuvan mukaiset muokkaukset ja muutin tiedoston nimeksi `penskuj.live.conf`.
- Lopuksi muutin `sites-enabled`-kansion linkin tämän uuden `penskuj.live.conf` mukaiseksi ja käynnistin apache2-palvelimen uudelleen.

![image-4](https://github.com/user-attachments/assets/4806ef50-0742-414b-82c5-6f01769f0ef5)

Näiden muutosten myötä sivu näkyi toivotun mukaisesti, kun edellisessä vaiheessa se tuli näkyviin oltuaan ainoa apache-palvelimen tuntema kotisivu.

## c) Kotisivu

Tein opettajan ehdotuksen mukaiset `blog-html` ja `projects.html` -tiedostot, sekä päivitin `index.html`-tiedoston sisällön alla olevan mukaiseksi. Kaikkien sivujen html-koodi on nyt hyvin samankaltainen minimalistinen; ehkä saan näitä kehitettyä myöhemmin pidemmälle.

![image-5](https://github.com/user-attachments/assets/ae64d124-785a-4f0c-8132-711856f7d0e2)

## d) Alidomain

Cloudfare (2025) kertoo mitä DNS-tietueiden A- ja CNAME-tyypit ovat. Tein `test` ja `alt` -alidomainit Name.comin hallintasivulla. Jälkimmäisen tein CNAME-tietueena, joka siis osoittaa penskuj.com domain-nimeen.

![image-6](https://github.com/user-attachments/assets/7948138e-1c80-45a6-8536-e3f1819e40e0)

## e) DNS-tiedot

![image-7](https://github.com/user-attachments/assets/0b07d551-5918-4413-809f-7eb6d012faec)

Ensimmäinen haaste: Dig-komentoa ei löydy. Arunral (2019), selventää että komento ei välttämättä ole esiasennettuna ja opastaa sen asentamiseen.

Asennan DNS-utils -työkalut `sudo apt-get install dnsutils`-komennolla. Tämän jälkeen pystyin käyttämään host ja dig -komentoja, joita kokeilin ensimmäisenä omaan uuteen kotisivuuni:

![image-8](https://github.com/user-attachments/assets/a5221019-538e-48ee-ae99-34a39f1ca78e)

![image-9](https://github.com/user-attachments/assets/d59753b9-f845-4326-ac6b-1b9da3b49111)

Rehellisesti sanoen, dig-komennon tuotos näyttää tässä vaiheessa aika paljolta tekstiltä joka kertoo hyvin vähän muuta kuin että yhteys on saatu. Seuraavaksi kokeilen näitä komentoja Atkins ry:n verkkosivuille atkins.fi:

![image-10](https://github.com/user-attachments/assets/8627d7a8-5a9d-4411-b8dc-10c3f70e5332)

Host-komennon tuloksissa yllättää se, että ainejärjestöllämme on kuitenkin kaksi erillistä IPv4-osoitetta saman domain-nimen alla. Kokeilen komentoja myös suuren yrityksen, Microsoftin kotisivuihin microsoft.com

![image-11](https://github.com/user-attachments/assets/a64ff70f-ef28-4d25-b147-96ee38408d67)

![image-12](https://github.com/user-attachments/assets/3339897c-6922-44e2-9443-8d7e3865356c)

Microsoftilta löytyi host-komennolla vielä muutama IP-osoite enemmän. Dig-komennolla eri IPv4-osoitteista tuli aika samannäköisiä lopputulemia. HowToGeek-sivuston McKay (2024) kertoo dig-komennon tulosteen tulkinnasta melko hyvin, ehkä eniten sen että komentoa voi käyttää myös domain-nimellä, jolloin tulosteena saa kaikki sen käyttämät IPv4 osoitteet hyvin host-komennon tapaisesti.

## Lähteet

Arunral, A. 2019. How to install dig, nslookup, host commands in Linux. Luettavissa: https://www.crybit.com/install-dig-nslookup-host-commands/. Luettu 23.2.2025

Cloudflare. 2025. What is a DNS record? Luettavissa: https://www.cloudflare.com/learning/dns/dns-records/. Luettu 23.2.2025.

Karvinen, T. 2024. Linux Palvelimet 2025 alkukevät. Luettavissa: https://terokarvinen.com/linux-palvelimet/. Luettu 17.2.2025.

McKay, D. 2024. How to Use the dig Command on Linux. Luettavissa: https://www.howtogeek.com/663056/how-to-use-the-dig-command-on-linux/. Luettu 23.2.2025.

Pennanen, J. 2025. h1 Oma Linux. Luettavissa: https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md. Luettu 17.2.2025.

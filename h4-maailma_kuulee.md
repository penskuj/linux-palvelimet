# h4 Maailma Kuulee

Linux-ympäristö on dokumentoitu ensimmäisen harjoitustehtävän [raportissa](https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md) (Pennanen, 2025).

Tämän raportin palvelimen asennus on tehty 9.2.2025 klo 17-20 välillä. Tehtävät tehty opettajan tehtävänannon mukaisesti (Karvinen, 2024).

## x) Artikkelitiivistelmät

**Teoriasta käytäntöön pilvipalvelimen avulla (h4)** (Lehto, 2022)
- Melko vastaava Linux-palvelimet -kurssin kotitehtävä aiemmalta vuodelta. Hieman laajempana sisältää DNS-nimipalvelun asetuksen ja tunkeutumistietojen lokien tarkastelua
- Käyttää DigitalOcean ja Namecheap -palveluita, koska nämä saa toimimaan GitHub Educationin krediiteillä.
- Tärppinä löytyy myös Billing Alerts -toiminnon asettaminen DigitalOceanilla rahankäytön hallinnan varmistamiseksi

**First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS** (Karvinen, 2017)
- Opettajan kotisivujen ohjeistus oman palvelimen luomiseen.
- Korostaa vahvojen salasanojen tärkeyttä.
- Palvelimen alustamisen ensimmäiset toimenpiteet:
    - Palomuurin asennus ja käynnistys
    - Uuden sudo-käyttäjän luonti ja root-käyttäjän sulkeminen
    - Ohjelmistopäivitykset
    - Käytön aloittaminen


## a) Vuokraa oma virtuaalipalvelin

Avasin GitHub Education -verkkosivun ja klikkasin DigitalOceanin linkkiä. Sign-up -linkin kautta pääsin luomaan käyttäjätilin GitHub-tiliin liitettynä maksutietojen ja GihHub-autentikaation jälkeen. Näiden jälkeen pääsin DigitalOceanin etusivulle, jossa Spin Up a Droplet -painikkeesta pääsee luomaan omaa virtuaalipalvelinta.

![image](https://github.com/user-attachments/assets/50b0b15d-a554-49cb-80c3-cdbea64c8f6a)

Seuraavassa valikossa asetin palvelimen sijainniksi Frankfurtin ja käyttöjärjestelmäksi Debian versio 12 (x64). Tietokoneen komponenteiksi valitsin edullisimman virtuaalikoneversion, jossa on 1GB keskusmuistia kuvan mukaisesti:

![image-1](https://github.com/user-attachments/assets/b0301476-ddbf-45e9-ba19-e4705884c3ea)

Seuraavassa vaiheessa on tarve valita SSH ja salasana-autentikaation väliltä. Tahdoin mieluummin SSH:n joten avasin oman virtuaali-linuxini saadakseni oman SSH avaimeni luotua. `sudo apt-get install openssh-client` -komennolla sain palautteen, että SSH-ohjelma on jo asennettuna koneellani. `ssh-keygen` -komento tämän jälkeen loi uuden ssh avaimen. Menin `/home/penskuj/.ssh/`-kansioon ja avasin `id_rsa.pub` -tiedoston, ja kopioin tämän julkisen SSH-avaimen DigitalOceanin SSH-kenttään.

Annoin virtuaalipalvelimelle nimeksi debian01-LiPa, ja hyväksyin asennuksen. Näin minulla on palvelin, jolla on julkinen IP-osoite.

![image-2](https://github.com/user-attachments/assets/f2a9d851-e015-484d-97c0-a8c3a4d08b22)


## b) Tee alkutoimet omalla virtuaalipalvelimellasi

kirjauduin debian01-LiPa palvelimelleni root tunnuksilla `ssh root@`komennolla palvelimen IP osoitteella lisättynä. Loin palvelimelle uuden sudo-oikeudellisen käyttäjän penskuj `sudo adduser penskuj`ja `sudo adduser penskuj sudo` -komennoilla. Annoin käyttäjälle uniikin vahvan salasanan. Tämän jälkeen lisäsin penskuj-käyttäjän ssh yhteydellä tavoitettavaksi kopioimalla julkisen ssh-avaimen tiedot root kansiosta käyttäjän kansioon `cp -n -r /root/.ssh /home/penskuj/` -komennolla. Käyttäjän kansiossa `chown penskuj:penskuj .ssh -R` -komento vaihtoi käyttäjän tämän ssh-tiedoston ja sen hakemiston omistajaksi. Lopuksi suljin virtuaalipalvelimen `exit`-komennolla.

![image-3](https://github.com/user-attachments/assets/dfb840fc-2e50-4e9c-be85-42bed5292bb6)

Uuden käyttäjän asetettuani kirjauduin tällä käyttäjällä takaisin palvelimelle `ssh penskuj@`, jossa lukitsin root-käyttäjän `sudo usermod --lock root` ja `sudo rm /root/.ssh -r` -komennoilla.

Palvelimen alustukseksi ja palomuurin asentamiseksi ajoin seuraavat komennot:
- `sudo apt-get update` (Päivitysten lukeminen)
- `sudo apt-get dist-upgrade`(Päivitysten asennus)
- `sudo apt-get install ufw`(Palomuurin asennus)
- `sudo ufw allow 22/tcp` (SSH-portin salliminen palomuurissa)
- `sudo ufw enable` (Palomuurin päällekytkentä)

Lopuksi `sudo ufw status verbose` näytti, että palomuuri on toiminnassa halutulla tavalla:

![image-4](https://github.com/user-attachments/assets/d90c7318-eda2-4d22-a9b6-bc906b99650b)


## c) Asenna weppipalvelin

Ajoin edelleen SSH-yhteydellä palvelimelle kirjautuneena komennot:
- `sudo apt-get install apache2`
- `echo "Default Test"|sudo tee /var/www/html/index.html`
- `sudo systemctl restart apache2`

Yritin mennä palvelimen Internet-sivulle, mutta ei toiminut. Muistin, että palomuurille on tehtävä muokkaus http-liikenteen sallimiseksi `sudo ufw allow 80/tcp`-komennolla. Tämän jälkeen IP-osoitteella 46.101.100.185 päsee palvelimen Internet-sivulle.

![image-5](https://github.com/user-attachments/assets/34913642-da72-4096-b701-a9f943c59879)


## d) Uusi Name Based Virtual Host

Tein palvelimen käyttäjän alle hakemiston `../public_sites/penskuj.com`, johon loin pienen html-tiedoston. Tein uuden sivuston config-tiedoston `/etc/apache2/sites-available`-hakemistossa.

![image-6](https://github.com/user-attachments/assets/14645623-bcee-462f-815c-d39e1385be81)

Tämän jälkeen kävin `sites-enabled`-hakemistossa vaihtamassa sallitun sivuston tähän. `sudo a2ensite penskuj.com`-komennolla. Samoin disabloin vanhan default-sivun ja käynnistin apachen palvelimen uudelleen. Internet-yhteys ei kuitenkaan lähtenyt toimimaan ja sain `/var/log/apache2/error.log` -tiedostoon mielenkiintoisen virheilmoituksen, että käyttäjällä ei ole riittäviä oikeuksia hakemistopuussa. 

![image-7](https://github.com/user-attachments/assets/8d683dc7-6ec6-4887-bbaa-f5fd9c655ffb)

Pienen selvittelyn jälkeen ymmärsin, että other-käyttäjäryhmä tarvitsee execute oikeudet koko hakemistopolkuun ja tämä puuttui `/home/penskuj/`-hakemistosta. Execute-oikeudet sai lisättyä kaikille ryhmille komennolla `chmod a+x .`. Tämän jälkeen verkkosivu latautui toivotusti käyttäjän kansiosta.

![image-8](https://github.com/user-attachments/assets/50063d4c-ba9f-4d74-bdf5-c5e1df33b1a9)


## Lähteet:

Karvinen, T. 2017. First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS. Luettavissa: https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/. Luettu 9.2.2025.

Karvinen, T. 2024. Linux Palvelimet 2025 alkukevät. Luettavissa: https://terokarvinen.com/linux-palvelimet/. Luettu 9.2.2025.

Lehto, S. 2022. Teoriasta käytäntöön pilvipalvelimen avulla (h4). Luettavissa: https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/. Luettu 9.2.2025. 

Pennanen, J. 2025. h1 Oma Linux. Luettavissa: https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md. Luettu 9.2.2025.

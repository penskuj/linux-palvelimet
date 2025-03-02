# h6 Salataampa

Linux-ympäristö on dokumentoitu ensimmäisen harjoitustehtävän [raportissa](https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md) (Pennanen, 2025).

Tämän raportti on tehty 2.3.2025 klo 22-24 ja seuraavan päivän 00-02 välillä. Tehtävät tehty opettajan kotisivun tehtävänantoa seuraten (Karvinen, 2024).

Sivustoni on hostattu DigitalOcean-palvelussa ja nimi on vuokrattu Name.com -sivulta.

## x) Lyhenteet

**https://letsencrypt.org/how-it-works/**
- Selittää verkkosertifikaattien teknologian toimintaperiaatteen ja Certificate Authorityjen (CA) roolin.
- Ensin sertifikointia pyytävän tahon on näytettävä hallitsevansa sivustoa, jolle hakee sertifikointia. Tämä onnistuu osoittamalla sivuston autentikaation yksityisavaimen käytöllä ja suoritetulla haasteella, esimerkiksi:
    - Luomalla CA:n tahdosta DNS-tietueen verkkosivustolle, tai
    - Luomalla sivustoon URI:in CA:n tahdosta resurssi
- Sivuston hallinnan todistamisen jälkeen CA myöntää hakijan palvelimelle sertifikaatteja enkryptoinnin yksityisavaimen. Tämän jälkeen sertifikaatit toimivat CAn antaessa julkisen avaimen sivuston vierailijoille ja sertifikaatin avainparin käydessä yhteen.
- Sivuston hallitsija voi myös pyytää sertifioinnin lakkauttamista.

**https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server**
- Opastaa sertifikaattien hallinta- ja hakuprosessin Lego-softalla käyttäjän roolissa.
- Mahdollisia käyttötapauksia useita. Tehtävän teossa keskitytään viimeiseen.
    - Legon sisäänrakennetun web-palvelimen kautta
    - DNS-palveluntarjoajan kautta
    - Räätälöidyn sertifikaattipyynnön kautta
    - Toiminnassa jo olevan verkkopalvelimen kautta
- Tässä viimeisessä käyttötapauksessa keskitytään annettavaan CLI-komentoon, jolla CA voi toteuttaa "haasteen" jonka perusteella se varmistaa sivuston internet-palvelimen olevan sertifikaatin hakijan hallinnassa.

**https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample**
- Apachen dokumentaatio vahvan SSL-salauksen luonnista verkkopalvelimelle.
- Määrittää minimitason SSL-suojauksen alustamisella, jota käytetään myös tässä raportissa
- Muu osa artikkelista puhuu vahvemmasta salauksesta ja eri salaustasojen varioinnista sivuston sisällä.

## a) Let's Encrypt

Kirjauduin omalle virtuaalipalvelimelleni `ssh penskuj@46.101.100.185`

Asensin legon komennolla `sudo apt-get install lego`.
Olin kätevästi valmiiksi penskuj-käyttäjän kotihakemistossa, joten loin uuden hakemiston legon hallitsemille sertifikaateille: `mkdir lego`.
Kokeilin sertifikaattien luontia Let's Encryptin testiympäristössä  `
lego --server=https://acme-staging-v02.api.letsencrypt.org/directory --accept-tos --email=[tässä oli oma email] --domains=penskuj.live --domains=www.penskuj.live --http --http.webroot="/home/penskuj/public_sites/penskuj.live" --path="/home/penskuj/lego" --pem run`. Pari kertaa piti yrittää että sai typoja korjattua, kunnes toimi:

![image-2](https://github.com/user-attachments/assets/c692d9cc-35a2-4a4e-a26a-ef167b87324e)

Sitten poistin `--server` -alkuisen osan komennosta, jolla loin todellisen Let's Encryptin sertifikaatin palvelimelleni.

Seuraavaksi korjailin name-based host-tiedostoani `/etc/apache2/sites-available`-kansiossa oheisen mukaiseksi:

![image](https://github.com/user-attachments/assets/7f3749ee-e2c8-4280-b050-6b7d64a8d96b)

Tämän jälkeen laitoin ssl-salauksen päälle apache-palvelimelle `sudo a2enmod ssl`-komennolla, ja restarttasin palvelimen. `sudo apache2ctl configtest` -komento tuotti Syntax OK -lopputuloksen.

Lopuksi lisäsin https-protokollan portille reiän palomuuriin. Ensimmäisessä yrityksessä sain virheen, koska kirjoitin "TCP" isolla.

![image-3](https://github.com/user-attachments/assets/fbd12f6f-bd06-4c38-9814-a91a26515346)

Otin Windows-koneeni Chrome-selaimelta yhteyden omalle nettisivulleni. Ensin se meni selaimen vanhasta muistista http-protokollalla, mutta URL-alun vaihtaminen https:ksi näytti että yhteys on nyt salattu:

![image-1](https://github.com/user-attachments/assets/a2a2cb15-0a1b-4992-854f-1b3fe39d7388)

## b) A-rating

Menin linkillä sivustolle ssllabs.com ja syötin sivustoni nimen penskuj.live. Sivusto testaili saittia hetken aikaa ja antoi lopputuloksena yllättävän vihreän raportin, josta alla muutamalla screenshotilla kriittisimmät osat. Yhteenvetona sanoisin, että luennoilla annetut palvelimen luonnin ja salauksen luomisen menetelmät johtavat tietoturvan kannalta hyvään lopputulokseen ottaen huomioon käytettyjen resurssien edullisuudenkin.

![image-4](https://github.com/user-attachments/assets/a11d4485-725e-4831-b653-6f7fb86979a1)

![image-5](https://github.com/user-attachments/assets/030d7876-214c-48eb-8ed4-3188da6d33aa)

![image-6](https://github.com/user-attachments/assets/78eea33e-1fe6-452f-adf0-83bfe5014a3f)

![image-7](https://github.com/user-attachments/assets/f4636b9e-e41b-4cb8-9e3e-9598c9fd3fb6)

## Lähteet: 

Apache Software Foundation. 2025. SSL/TLS Strong Encryption: How-To. Luettavissa: https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample. Luettu 2.3.2025.

Internet Security Research Group. 2025. How It Works. Luettavissa: https://letsencrypt.org/how-it-works/. Luettu 2.3.2025.

Karvinen, T. 2024. Linux Palvelimet 2025 alkukevät. Luettavissa: https://terokarvinen.com/linux-palvelimet/. Luettu 2.3.2025.

Lange, N. 2024. Obtain a Certificate. Luettavissa: https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server. Luettu 2.3.2025.

Pennanen, J. 2025. h1 Oma Linux. Luettavissa: https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md. Luettu 2.3.2025.

# h3 Hello Web Server

Linux-ympäristö on dokumentoitu ensimmäisen harjoitustehtävän [raportissa](https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md) (Pennanen, 2025).

Harjoitustyön osat a)-c) on tehty 29.1.2025 21:00-23:00 välillä. Muut osat 2.2.2025 18:00-20:30 välillä.

## x) Artikkelien tiivistelmät

**Name-based Virtual Host Support (Apache Software Foundation, 2025):**
- Apachen dokumentaation käsittelee name-based virtual hosting -menetelmää, jossa useita verkkosivustoja (eri domain-nimillä) isännöidään samasta IP-osoitteesta. 
- Tämä menetelmä säästää rajattua IPv4-osoiteavaruutta
- Käytännössä valittuun DNS-palveluun asetetaan sama IP osoite kullekin eri verkkosivustolle, kun taas oma Apacen HTTP-palvelin asetetaan ohjaamaan sisääntuleva liikenne halutulle sivustolle.
- Jos IP-osoitteeseen saapuva liikenne ei sisällä Apache-palvelimen listaamia sivustoja, liikenne ohjataan oletusarvoisesti konfigurointitiedostossa ensimmäiseksi listatulle sivustolle.

**Name Based Virtual Hosts on Apache (Karvinen, 2018):**
- Käytännön ohjeet ja linux-komentorivin komentoja, jotka auttavat name based virtual hosting -menetelmän käyttöönottoon.
- Apache-palvelimen asennus, uusien domain-nimien lisääminen ja näiden verkkosivujen luominen.


## a) Palvelin localhostissa

Menin internet-selaimeen ja kirjoitin osoitekenttään `localhost` ja tämä avasi aiemmin tunnilla luomani `jaakko.example.com` indeksisivun. Apache palvelin oli siis toiminnassa.

## b) Lokit

menin `var/log`-kansioon, ja avasin apache2:n `access.log`-tiedoston. Tiedoston listaamat aikalokitiedot olivat kuitenkin luennon ajalta. Viimeisimmät ovat siis eri tiedostossa.

![image](https://github.com/user-attachments/assets/8d4f96fb-c17c-4090-a396-4352cc9f3938)

Seuraavaksi avasin apache2:n `other_vhosts_access.log`-tiedoston, jonka alta viimeiset lokitiedot löytyivät. Selitteet pohjautuvat Apachen dokumentaatioon (Apache Software Foundation, 2025, "Log files"):

![image-1](https://github.com/user-attachments/assets/6ce33f6f-e8bc-4545-ae0a-8d282622fa7f)

- `jaakko.example.com:80` - Lokitiedon verkko-osoite ja portti, joihin on otettu yhteyttä.
- `127.0.0.1` - Asiakaskäyttäjän IP-osoite. Tässä tapauksessa oman sisäverkon oletusosoite.
- `- -` - Viiva ja asiakaskäyttäjälle assosioitu nimi. Tässä tapauksessa sekin on viiva, eli tyhjä.
- `[29/jan/2025:21:12:59 +0200]` - Aikaleima sekä ero UTC-aikavyöhykkeeseen.
- `"GET / HTTP/1.1"` - Asiakassovelluksen lähettämä komento.
- `304 247` - Palvelimen palauttama status-koodi (304 = Not modified), sekä palvelimen lähettämä koodipaketin koko (247 tavua)
- `"-"` - sivu, jolla olevasta linkistä saapuva liikenne kertoo tulevansa. Tässä tapauksessa tyhjä.
- `"Mozilla/5.0 (X11; Linux x86_64; rv:128:0) Gecko/20100101 Firefox/128.0"` - Asiakas-verkkoselaimen tunnistetietoja.


## c) Etusivu uusiksi

- Siirryin config-tiedostot sisältävään kansioon: `cd /etc/apache2/sites-available`
- Otin kopion olemassaolevasta conf-tiedostosta: `sudo cp jaakko.example.com.conf hattu.example.com.conf`. Tämän jälkeen muokkasin palvelimen tiedot ohjeen mukaan:

![image-4](https://github.com/user-attachments/assets/86b670d4-ab41-4ec8-9e83-375098c15c40)

- Vaihdoin kansiota, enabloin uuden sivuston ja disabloin edellisen: 
    - `cd /etc/apache2/sites-enabled`
    - `sudo a2ensite hattu.example.com`
    - `sudo a2dissite jaakko.example.com`
- Seuraavaksi käynnistin Apache-palvelimen uudellen `sudo systemctl restart apache2`

- Näiden komentojen jälkeen latasin uudelleen localhostin selaimessa, ja sain virheilmoituksen koska hattu.example.com -osoitteelle ei oltu vielä luotu html-tiedostoa.

![image-2](https://github.com/user-attachments/assets/bd60db7f-0c40-4c19-b3e9-0a94ab66cd87)

- Kävin luomassa uuden kansion hattu.example.com -sivustolle:
    - `cd /home/penskuj/Public_sites`
    - `mkdir hattu.example.com`
- Menin sisään uuteen kansioon ja tein palvelimen etusivun sisältävän tiedoston komennolla `echo "<h1>Hattu example</h1>" > index.html`. Tämän jälkeen sivun pystyi avaamaan selaimessa.

![image-3](https://github.com/user-attachments/assets/9793b802-881b-4a05-961a-6ad9ce1af5b5)


## e) HTML5 sivu

Tein karvalakin html-koodin `index.html`-tiedostoon:

![image-6](https://github.com/user-attachments/assets/bde4cb0f-32a3-4391-a263-334abb7f1146)

Sivu toimii uudelleen ladattaessa:

![image-5](https://github.com/user-attachments/assets/c9679b48-16d5-42bf-ba1f-f0bbc6300027)

## f) curl -I ja curl -komennot

Ensin `curl -I localhost` -komento:

![image-7](https://github.com/user-attachments/assets/30e50a08-7f9e-4345-a25b-8a69c580e0d2)

Selityksen Wikipediasta tulkittuna (Wikimedia Foundation, 2025):
- Ensin ilman otsikkoa: on komento ja status-viesti (200 = OK)
- `Date:` päivämäärä
- `Server:` Palvelimen nimi/versiotiedot
- `Last-Modified:` Milloin sivuston tietoja on viimeksi muokattu
- `ETag:` Tunniste resurssin versiolle.
- `Accept-Ranges:` "What partial content range types this server supports via byte serving" - Tämä on sen verran tekninen, etten ymmärrä.
- `Content-Length:` Palvelimen palauttaman vastauksen pituus tavuina.
- `Vary:` Kertoo alavirran sovelluksille miten palvelimen palauttama vastaus soveltuu näytettäväksi pyynnön otsikoiden perusteella, tai täytyykö niiden tehdä uusi pyyntö palvelimelle.
- `Content-Type:` Sivun sisällön internet-mediatyyppi.

Pelkkä `curl localhost`-komento puolestaan palauttaa HTML-lähdekoodin (tämä sama kuin edellisen e)-tehtävän kuvassa).

## m) Github education

Hakemus laitettu.

## o) Kaksi erinimistä verkkosivua

Lisäsin verkkosivustojen osoitteet hosts-tiedostoon, johon pääsee esim. `micro /etc/hosts` komennolla. Lisäsin sinne sivustojen osoitteet localhost-palvelimen IP-osoitteen jatkoksi:

![image-9](https://github.com/user-attachments/assets/7bead816-a64c-4fa6-a82f-6a8ae05d13fd)

Sain mielenkiintoisen virheen menemällä hattu.example.com -sivulle. Tämä lataa vanhan tekstitiedoston, jonka päälle olen jo sittemmin kirjoittanut HTML5-verkkosivun. Eli selaimen lataamaa sivua ei ollut enää olemassakaan.

![image-8](https://github.com/user-attachments/assets/ceb812d3-7304-43d9-8ca0-67d59508e052)

Tämä johtui sivun tallentumisesta selaimen välimuistiin. Päivitetty sivu aukesi painamalla shift samaan aikaan reload-painikkeen kanssa.

## Lähteet:

Apache Software Foundation. 2025. Name-based Virtual Host Support. Luettavissa: https://httpd.apache.org/docs/2.4/vhosts/name-based.html. Luettu 2.2.2025.

Apache Software Foundation. 2025. Log Files. Luettavissa: https://httpd.apache.org/docs/2.4/logs.html.Luettu 2.2.2025.

Karvinen, T. 2018. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. Luettavissa: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/. Luettu 2.2.2025.

Karvinen, T. 2024. Linux Palvelimet 2025 alkukevät. Luettavissa: https://terokarvinen.com/linux-palvelimet/. Luettu 2.2.2025.

Pennanen, J. 2025. h1 Oma Linux. Luettavissa: https://github.com/penskuj/linux-palvelimet/blob/main/h1-Oma-Linux.md. Luettu 2.2.2025.

Wikimedia Foundation. 2025. List_of HTTP header fields. https://en.wikipedia.org/wiki/List_of_HTTP_header_fields. Luettu 2.2.2025.

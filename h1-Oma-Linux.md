# Oma Linux -kotitehtävä

## Tiedot ympäristöstä
- Vanhahko Acer-kannettava tietokone Windows 10 Home Edition -käyttöjärjestelmällä (10.0.19045 Build 19045)
- BIOS versio: Insyde Corp. V1.19, 13/07/2018
- Prosessori: Intel(R) Core(TM) i5-8300H CPU @ 2.30GHz, 2304 Mhz, 4 Cores, 8 Logical Processors
- RAM: 8.00 GB
- SSD: 256GB, josta no 80GB vapaana ennen tehtävien aloittamista

## Raportti: Virtuaalisen Linux Debian -tietokoneen asennus
Käytin VirtualBox-virtualisointialustaa ja Linuxin Debian 12.9.0 -käyttöjärjestelmäversiota virtuaalikoneen luomisessa. Seurasin asennuksen ohessa opettajan luomaa Linux Debian -virtuaalikoneen asennusohjetta (Karvinen, 2024). Olen liittänyt raporttiini vaiheittain tulleista valikoista tärkeimpiä itse ottamiani kuvakaappauksia.

Latasin ja asensin Oracle VirtualBox-7.1.4-165100-Win -sovelluksen 14.1.2025 klo 19 (osoitteesta https://www.virtualbox.org/wiki/Downloads). Käytin asennuksessa asennustyökalun ehdottamia oletusasennuksia. Sovelluksen asennus onnistui.

Latasin debian-live-12.9.0-amd64-xfce.iso -tiedoston 19.1.2025 klo 17 (osoitteesta https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/).

Avasin VirtualBox-sovelluksen. Ensin yritin aloittaa Debian-pohjaisen virtuaalikoneen asennuksen Basic-tilassa, mutta tässä tilassa ei ollut mahdollista valita 64-bittisen Debian version asennusta. Palasin sovelluksen alkuvalikkoon, ja vaihdoin tilan Expert-tilaan. Expert-tlassa asennuksen tekeminen edistyi. Valitsin "Skip Unattended Installation" -vaihtoehdon päästäkseni tekemään tarvittavat valinnat ja asentamaan Debianin 64-bittisen version. Annoin Linux-virtuaalikoneelleni käytettäväksi 2 prosessoria, 4.096 GB RAM-muistia ja lisäksi 30GB kovalevytilaa.

![image](https://github.com/user-attachments/assets/349f94ca-e655-4753-8afc-154220fb60e1)
![image](https://github.com/user-attachments/assets/6764a9b9-17d7-456b-a6f3-68e46e1bfef6)

Finish-painikkeen jälkeen virtuaalikone oli luotu. Lisäksi VirtualBox-oli valmiiksi asettanut käyttämäni .iso-levykuvatiedoston virtuaalikoneen ROM-asemaan, joten sitä ei tarvinnut erikseen hakea.
![image](https://github.com/user-attachments/assets/53c8f20a-d1ce-4cf6-803f-9dd72ccc6189)
![image](https://github.com/user-attachments/assets/f27e875b-4028-497b-85ba-e5f466e12e31)

Käynnistin virtuaalikoneen VirtualBoxissa. Ruudulle tulleista alkuvalikoista valitsin ensimmäiset vaihtoehdot käynnistää Linux-kernelin ja Debianin Live-tilassa asentamatta ensin käyttöjärjestelmää. Tästä pienen latailun jälkeen pääsin Debianin työpöydälle, josta pystyin avaamaan verkkoselaimen.

![image](https://github.com/user-attachments/assets/26fe2691-c7c3-43e7-80bb-aab6d9a93079)

Kaikki näytti toimivan odotetusti, joten klikkasin työpöydän "Install Debian" -kuvaketta. Valitsin sijainniksi Suomen, kieleksi Englannin ja näppäimistöksi Finnish (Default) -näppäimistöasetukset. Seuraavaksi määräsin Debianin asennustyökalua tyhjentämään sille varatun virtuaalisen kovalevyn. Tein käyttäjätilistä ja tietokoneen nimestä yhtenevät jo aiemmin käyttämieni nimien kanssa. Lopuksi hyväksyin asennuksen ja annoin virtuaalikoneen suorittaa asennuksen, jossa kesti noin 10 minuuttia.

![image](https://github.com/user-attachments/assets/e6a57376-1cd7-4569-97a6-78ee1e7c0f7b)
![image](https://github.com/user-attachments/assets/fc89ba87-a24e-4829-a2fe-4d57d70606b0)
![image](https://github.com/user-attachments/assets/34e4f311-5702-4906-b01a-01bbb82abdb8)

Asennus onnistui, ja ehdotetun rebootin tekemisen jälkeen pääsin virtuaalikoneelle asennetun Linux Debianin työpöydälle omalla käyttäjätililläni.

![image](https://github.com/user-attachments/assets/8eb8c34e-8b4e-4f89-89dc-473dece411ac)

## Bonustehtävä - Suosikkiohjelmani Linuxilla

Minulla on häviävän vähän aiempaa Linux-osaamista, enkä ole vielä asentanut omia ohjelmiani Debianiin, joten mainitsen suosikkiohjelmakseni Firefox-selaimen.

Avasin Firefoxin, ja menin Settings-valikkoon Search-välilehdelle, jossa muutin selaimen oletushakukoneen Duckduckgo-hakumoottoriksi. Lisäksi avasin terokarvinen.com ja hs.fi -sivustot ja asetin nämä suosikkisivustot-työkalupalkkiin, ja asetin suosikkisivustot-palkin aina näkyväksi. Tämän jälkeen suljin selaimen ja sammutin virtuaalitietokoneen. 

## Artikkelien tiivistelmät:
### Raportin kirjoittaminen (Karvinen, 2006)
- Ohjeistus opintojaksolla palautettavien kotitehtävien raportointiin.
- Ydinperiaatteena toimenpiteiden toistettavuus, jolloin raportoidut toimenpiteet samassa ympäristössä tekemällä saa aina saman lopputuloksen.
- Lisäksi raporttien tulee olla täsmällisiä, helppolukuisia, sekä viitata lähteisiin.
- Raportteja ei saa sepittää tai plagioida, eikä niihin saa luvattomasti liittää kuvia Internetistä.

### What is Free software? (Free Software Foundation, 2021):
- Vapaa ohjelmisto ei välttämättä ole käyttäjälleen ilmainen, vaan toteuttaa neljää vapaan ohjelmiston periaatetta.
- Periaatteet ovat vapaus käyttää ohjelmistoa miten haluaa, vapaus perehtyä ja tehdä muokkauksia ohjelman lähdekoodiin, vapaus jakaa ohjelman kopioita, sekä vapaus jakaa muokattuja ohjelman kopioita.
- Kaikkia periaatteita tulee noudattaa vapaassa ohjelmistossa; muutoin ohjelma on ei-vapaa eli yksityisomistuksellinen, ja sen omistaja/kehittäjä käyttää valtaa ohjelmiston loppukäyttäjään.
- Vapaa ohjelmisto voi myös olla sopimuspohjaisten lisenssien alainen, jos sopimuksen ehdot eivät olennaisesti rajoita mainitun neljän periaatteen toteutumista.

## Lähteet:

Free Software Foundation. 2021. What is Free Software?. Luettavissa: https://www.gnu.org/philosophy/free-sw.html Luettu 19.1.2025

Karvinen, T. 2024. Install Debian on Virtualbox - Updated 2024. Luettavissa: https://terokarvinen.com/2021/install-debian-on-virtualbox/. Luettu 19.1.2025

Karvinen, T. 2006. Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ Luettu 19.1.2025

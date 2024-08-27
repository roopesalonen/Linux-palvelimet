# h1 - Oma Linux

## Raportin kirjoittaminen
- Raportissa pitää kertoa täsmällisesti mitä tekee ja mitä siitä seuraa
- Raportin tulee olla toistettava samanlaisessa ympäristössä
- Helppolukuisuuteen tulee panostaa
- Myös vikailmoitukset ja ongelmat tulee raportoida

## Free Software
- Suomeksi Vapaa ohjelmisto
- Tarkoittaa vapautta opiskella, käyttää, jakaa ja muokata ohjelmistoa


# Debianin asentaminen virtuaalikoneeseen
Ennen kuin aloitat, tarvitset virtualisointiohjelman, jolla voit luoda virtuaalikoneen. Voit ladata sen [täältä](https://www.virtualbox.org/wiki/Downloads) (VirtualBox), sekä Debian .iso tiedoston [tästä](https://www.debian.org/distrib/).

## Raportissa käytetty ympäristö
- Debian 12.6.0 Live Xfce
- VirtualBox 7.0.20
#### PC
- Ryzen 7 7800X3D
- GeForce 4060
- 32GB RAM
- Windows 11

# Aloitetaan asennus - kello 12.50
Klikkasin VirtualBoxissa "New" painiketta ja valitsin alhaalta "Expert Mode". Annoin virtuaalikoneelle nimen ja valitsin ISO Imageksi lataamani Debian 12.6.0 live .iso tiedoston. Klikkasin päälle kohdan "Skip Untattended Installation".

![screenshot](https://i.imgur.com/EMUgsRC.png)

Valitsin virtuaalikoneen käyttöön 4gb muistia ja 2 prosessoria, sekä 20gb tallennustilaa. Sen jälkeen viimeistelin virtuaalikoneen luonnin painamalla "Finish".

![screenshot](https://i.imgur.com/F2xzI0E.png)
![screenshot](https://i.imgur.com/T0OjbQK.png)

# Käynnistetään virtuaalikone - kello 13.00

Klikkasin virtuaalikoneen käyntiin "Run" painikkeesta. Boot menu aukeaa ja valitsin ylimmän vaihtoehdon, eli Live system (amd64).

![screenshot](https://i.imgur.com/AeUde39.png)

Virtuaalikone käynnistyi työpöydälle. Seuraavaksi testasin, että hiiri, näppäimistö ja internet toimivat. Avasin internet selaimen työpöydän alalaidasta ja testasin että pääsen eri nettisivuille. Seuraavaksi asensin Debianin koko version virtuaalikoneeseen valitsemalla työpöydän vasemmasta alakulmasta "Install Debian" kuvake. Ruudulle tuli "Untrusted application" ilmoitus, valitsin "Launch Anyway".

![screenshot](https://i.imgur.com/5jnabud.png)

Ruudulle aukeaa Debian installer. Jätin järjestelmän kieleksi englannin, mutta vaihdoin aikavyöhykkeen ja näppäimistön Suomeksi.

![screenshot](https://i.imgur.com/1XrZ5zS.png)

Partitions kohdassa valitsin "Erase disk".

![screenshot](https://i.imgur.com/4lrI6Fe.png)

Kun pääsin summary kohtaan, niin install nappula ei mahtunut ruutuun. Jouduin raahamaan ikkunaa hieman ylöspäin, jotta se tuli näkyviin. Valitsin "install" ja Debianin asennus käynnistyy.

![screenshot](https://i.imgur.com/slM4Vr1.png)
![screenshot](https://i.imgur.com/8HbxAE3.png)

# Asennus valmis - kello 13.28

Kun asennus oli valmis, valitsin "Restart now" ja virtuaalikone käynnistyi uudelleen. Testasin vielä varmuuden vuoksi, että näppäimistö, internet yms. toimii oikein. Virtuaalikone on nyt valmis käyttöön.

## Lähteet
Karvinen, T. 2024. Linux Palvelimet 2024 alkusyksy https://terokarvinen.com/linux-palvelimet/

Karvinen, T. 2023. Install Debian on Virtualbox https://terokarvinen.com/2021/install-debian-on-virtualbox/

Karvinen, T. 2006. Raportin kirjoittaminen https://terokarvinen.com/2006/raportin-kirjoittaminen-4/

Free Software Foundation 2024. What is Free Software? https://www.gnu.org/philosophy/free-sw.html

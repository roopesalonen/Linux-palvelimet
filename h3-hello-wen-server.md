# h3 - Hello Web Server

>### Raportissa käytetty ympäristö
>- Debian 12.6.0 Live Xfce
>- VirtualBox 7.0.20
>- CPU: AMD Ryzen 7 7800X3D
>- GPU: Asus GeForce RTX 4060 OC
>- RAM: 32GB DDR5 6000MHz
>- OS: Windows 11 Pro

## x) Lue ja tiivistä
### Name-based Virtual Host Support
- Yleensä yksinkertaisempi kuin IP-based
- Mahdollistaa saman IP-osoitteen käytön usealla eri hostilla
- Käyttäjän tulee ilmoittaa hostnimi osana HTTP headereita

### Name Based Virtual Hosts on Apache
- Apache web-palvelimen asennus `sudo apt-get -y install apache2`
- Oletussivuston korvaaminen `echo "Default"|sudo tee /var/www/html/index.html`
- Name-based virtual hostin luominen `sudoedit /etc/apache2/sites-available/pyora.example.com.conf`
- Sivun kytkeminen päälle `sudo a2ensite pyora.example.com`
- Sivun uudelleenkäynnistys, jotta muutokset tulevat voimaan `sudo systemctl restart apache2`
- Toimivuuden testaus `curl -H 'Host: pyora.example.com' localhost` ja `curl localhost`
- Web-sivun luonti normaalina käyttäjänä `mkdir -p /home/xubuntu/publicsites/pyora.example.com/` ja `echo pyora > /home/xubuntu/publicsites/pyora.example.com/index.html`

## a) Testaa, että weppipalvelimesi vastaa localhost-osoitteesta
Kerkesin asentamaan Apachen jo viime luennolla ja muokkailemaan myös oletussivun sisältöä. Apachen voi asentaa komennolla `sudo apt-get -y install apache2`

Testasin toimivuuden nettiselaimessa kirjoittamalla osoitepalkkiin `localhost`

![screenshot](https://i.imgur.com/lszMmWo.png)

Seuraavaksi testasin terminaalin kautta komennolla `curl localhost`

![screenshot](https://i.imgur.com/XweiJ2w.png)

## b) Etsi lokista rivit, jotka syntyvät, kun lataat omalta palvelimeltasi yhden sivun
Avasin lokin komennolla `sudo tail /var/log/apache2/access.log`

![screenshot](https://i.imgur.com/QohgaYF.png)

Ensimmäisenä localhost osoite eli `127.0.0.1`. 

Seuraavaksi päivämäärä, kello ja aikavyöhyke `10/Sep/2024:14:28:24 +0300`

Käyttäjän pyynnön tyyppi `GET / HTTP/1.1`

HTTP vastaus koodi `200`

Käyttäjälle palautetun objektin koko `500`

Ja viimeisenä tietoa selaimesta mitä käytettiin `Mozilla...` 

## c) Tee uusi name based virtual host
Korvasin ensin oletussivun komennolla `echo "Default"|sudo tee /var/www/html/index.html` ja testasin toimivuuden komennolla `curl localhost`

![screenshot](https://i.imgur.com/Z0fBCDs.png)

Loin uuden asetustiedoston komennolla `sudoedit /etc/apache2/sites-available/hattu.example.com.conf`

Ruudulle aukesi tyhjä tekstieditori johon tallensin seuraavat tiedot:

    <VirtualHost *:80>
        ServerName hattu.example.com
        ServerAlias www.hattu.example.com
        DocumentRoot /home/roope/publicsites/hattu.example.com
        <Directory /home/roope/publicsites/hattu.example.com>
            Require all granted
        </Directory>
    </VirtualHost>

![screenshot](https://i.imgur.com/dvMapFe.png)

Seuraavaksi kytkin sivun käyttöön ja uudelleenkäynnistin sen komennoilla `sudo a2ensite pyora.example.com` ja `sudo systemctl restart apache2`

![screenshot](https://i.imgur.com/PZeWjTu.png)

Testasin `curl localhost` ja jotain on selvästi tapahtunut

![screenshot](https://i.imgur.com/ehx9a89.png)

Kävin poistamassa muut `/etc/apache2/sites-available` kansiosta löytyvät sivut käytöstä komennolla `sudo a2dissite 000-default.conf`

![screenshot](https://i.imgur.com/wBuBwG6.png)

Loin sivulle kansion komennolla `mkdir -p /home/roope/publicsites/hattu.example.com/` ja `echo hattu > /home/roope/publicsites/hattu.example.com/index.html`

Sen jälkeen testasin, että sivu toimii komennolla `curl localhost`

![screenshot](https://i.imgur.com/3bjhcF5.png)

Seuraavaksi lisäsin `hattu.example.com` tiedostoon `/etc/hosts`

Käytin komentoa `sudoedit /etc/hosts`

![screenshot](https://i.imgur.com/XCoZhqV.png)

Testasin että example.hattu.com ja localhost toimivat komennoilla `curl hattu.example.com` ja `curl localhost`

![screenshot](https://i.imgur.com/BwXsToZ.png)

Testasin toimivuuden myös selaimessa

![screenshot](https://i.imgur.com/eBnXLVt.png)

## e) Tee validi HTML5 sivu
Muokkasin hattu sivuston index tiedostoa `/home/roope/publicsites/hattu.example.com/index.html` komennolla `micro index.html` ja lisäsin sinne sisältöä

![screenshot](https://i.imgur.com/TkEq8Ce.png)

Kävin katsomassa miltä sivu näyttää selaimessa

![screenshot](https://i.imgur.com/oS35M6j.png)

Testasin onko sivu validi osoitteessa https://validator.w3.org/

Ei näyttänyt olevan ongelmia

![screenshot](https://i.imgur.com/c320g6c.png)

## f) Anna esimerkit 'curl -I' ja 'curl' -komennoista

`curl` komennolla pystyy näyttämään sivun sisällön ilman sen avaamista selaimessa

![screenshot](https://i.imgur.com/4RFF2b7.png)

`curl -I` komennolla pystyy katsomana sivun headereita

Ensimmäisellä rivillä näkyy HTTP versio ja vastaus `HTTP/1.1 200 OK`

Päivämäärä, aika ja serveri `Date`, `Server`

Milloin sivua on muokattu viimeksi `Last-Modified`

![screenshot](https://i.imgur.com/0ySOH56.png)

## m) Hanki GitHub Education -paketti
Laitoin hakemuksen GitHub Educationiin heti edellisen luennon päätyttyä ja se on hyväksytty

![screenshot](https://i.imgur.com/9cG4WQO.png)

## Lähteet
Karvinen, T. 2024. Linux Palvelimet 2024 alkusyksy https://terokarvinen.com/linux-palvelimet/

The Apache Software Foundation 2024. Name-based Virtual Host Support https://httpd.apache.org/docs/2.4/vhosts/name-based.html

The Apache Software Foundation 2024. Log Files https://httpd.apache.org/docs/current/logs.html

Karvinen, T. 2018. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/

Apidog 2024. Curl -i Command https://apidog.com/articles/curl-i-command/

GitHub 2024. GitHub Education https://github.com/education

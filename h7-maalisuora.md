# h7 - Maalisuora

>### Raportissa käytetty ympäristö
>- Debian 12.6.0 Live Xfce
>- VirtualBox 7.0.20
>- CPU: AMD Ryzen 7 7800X3D
>- GPU: Asus GeForce RTX 4060 OC
>- RAM: 32GB DDR5 6000MHz
>- OS: Windows 11 Pro

## a) Kirjoita ja aja "Hei maailma" kolmella kielellä

Käytetty Karvisen ohjetta [Programming Languages on Ubuntu](https://terokarvinen.com/2018/hello-python3-bash-c-c-go-lua-ruby-java-programming-languages-on-ubuntu-18-04/)

#### Python

Pythonin saa asennettua komennolla `sudo apt-get install python3`

Ensin loin tiedoston `micro hellopython.py` ja laitoin sisällöksi `print("Hello Python!")`

Ajoin koodin komennolla `python3 hellopython.py`

![screenshot](https://i.imgur.com/CWrRyLx.png)

#### Bash

Asensin Bashin komennolla `sudo apt-get install bash`

Loin tiedoston `micro hellobash.sh` ja sisällöksi `echo "Hello Bash!"`

Ajoin koodin komennolla `bash hellobash.sh`

![screenshot](https://i.imgur.com/4YbMXFh.png)

#### Java

Asensin ensin javan komennolla `sudo apt-get install default-jre`

Loin tiedoston `micro hellojava.java` ja sisällöksi:

    public class hellojava
    {
        public static void main(String[] args)
        {
        System.out.println("Hello Java!");
        }
    }

Ajoin koodin komennolla `java hellojava.java`

![screenshot](https://i.imgur.com/0jLx8JS.png)

## b) Laita Linuxiin uusi komento niin, että kaikki käyttäjät voivat ajaa sitä

En keksinyt mitään luovaa, joten tein saman komennon kuin [Karvisen ohjeessa](https://terokarvinen.com/2007/12/04/shell-scripting-4/)

Menin tekemään tiedoston suoraan kansioon /usr/local/bin jotta kaikki voivat käyttää sitä

`micro lspwd` ja sisällöksi:

    #!/bin/bash
    ls
    pwd

Annoin tiedoston käyttöoikeudet komennolla `sudo chmod a+x lspwd`

Nyt komento toimi syöttämällä `lspwd`

![screenshot](https://i.imgur.com/vBUApXA.png)

![screenshot](https://i.imgur.com/deRpvFf.png)

## c) Ratkaise vanha arvioitava laboratorioharjoitus soveltuvin osin

Tein [tämän labran](https://terokarvinen.com/2024/arvioitava-laboratorioharjoitus-2024-linux-palvelimet/) kohdat d), e), g) ja asensin uuden virtuaalikoneen jolle ajoin seuraavat komennot:

![screenshot](https://i.imgur.com/B6KwMOb.png)

### lab d) 'howdy'
- Tee kaikkien käyttäjien käyttöön komento 'howdy'
- Tulosta haluamaasi ajankohtaista tietoa, esim päivämäärä, koneen osoite tms
- Pelkkä "hei maailma" ei riitä
- Komennon tulee toimia kaikilla käyttäjillä työhakemistosta riippumatta

Loin hakemistoon /usr/local/bin/ tiedoston `sudo nano howdy` ja lisäsin:

    #!/bin/bash
    hostname
    date
    hostname -I

Annoin tiedostolle oikeudet komennolla `sudo chmod a+x howdy`

![screenshot](https://i.imgur.com/9QjmxLO.png)

![screenshot](https://i.imgur.com/8fKZ0hh.png)

### lab e) Etusivu uusiksi
- Asenna Apache-weppipalvelin
- Tee yrityksellemme "AI Kakone" kotisivu
- Kotisivu tulee näkyä koneesi IP-osoitteella suoraan etusivulla
- Sivua pitää päästä muokkaamaan normaalin käyttäjän oikeuksin (ilman sudoa)

Asensin Apachen komennolla `sudo apt-get -y install apache2`

Tein conffi tiedoston komennolla `sudoedit /etc/apache2/sites-available/aikakone.example.com.conf`

    <VirtualHost *:80>
    ServerName aikakone.example.com
    ServerAlias www.aikakone.example.com
    DocumentRoot /home/roo/publicsites/aikakone.example.com
    <Directory /home/roo/publicsites/aikakone.example.com>
        Require all granted
    </Directory>
    </VirtualHost>

Laitoin uuden sivun päälle `sudo a2ensite aikakone.example.com` ja poistin oletuksen `sudo a2dissite 000-default.conf`

Käynnistin Apachen uudelleen `sudo sytemctl restart apache2`

![screenshot](https://i.imgur.com/O3h76Uq.png)

Tein sivulle kansion komennolla `mkdir -p /home/roo/publicsites/aikakone.example.com/`

Loin index.html tiedoston ja sinne tekstin "AI Kakone" komennolla `echo AI Kakone > /home/roo/publicsites/aikakone.example.com/index.html`

Nyt sivu näkyy localhostissa

![screenshot](https://i.imgur.com/O6eJlBm.png)

Lisäsin aikakone.example.com `sudoedit /etc/hosts`

![screenshot](https://i.imgur.com/OWxIwO2.png)

Nyt sivu näkyy myös aikakone.example.com osoitteessa

![screenshot](https://i.imgur.com/oAUh8wv.png)

### lab g) Salattua hallintaa
- Asenna ssh-palvelin
- Tee uusi käyttäjä omalla nimelläsi, esim. minä tekisin "Tero Karvinen test", login name: "terote01"
- Automatisoi ssh-kirjautuminen julkisen avaimen menetelmällä, niin että et tarvitse salasanoja, kun kirjaudut sisään. Voit käyttää kirjautumiseen localhost-osoitetta

Asensin ssh:n `sudo apt-get install ssh`

Loin uuden käyttäjän `sudo adduser rootest`

Loin avainparin `ssh-keygen`

Kopioin avainparin uudelle käyttäjälle `ssh-copy-id rootest@localhost`

Yhdistin localhostiin uudella käyttäjällä eikä salasanaa enään vaadita `ssh rootest@localhost`

![screenshot](https://i.imgur.com/iQXgK4K.png)


## Lähteet
Karvinen, T. 2024. Linux Palvelimet 2024 alkusyksy https://terokarvinen.com/linux-palvelimet/

Karvinen, T. 2018. Programming Languages on Ubuntu https://terokarvinen.com/2018/hello-python3-bash-c-c-go-lua-ruby-java-programming-languages-on-ubuntu-18-04/

Karvinen, T. 2007. Shell Scripting https://terokarvinen.com/2007/12/04/shell-scripting-4/

Karvinen, T. 2024. Final Lab for Linux Palvelimet 2024 Spring https://terokarvinen.com/2024/arvioitava-laboratorioharjoitus-2024-linux-palvelimet/

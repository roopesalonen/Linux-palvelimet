# h2 - Komentaja Pingviini

## Komentorivin perusteet
Komentorivi on ollut olemassa jopa kauemmin kuin internet.

Sillä pystyy mm. liikkumaan hakemistojen välillä, muokkaamaan ja katsomaan tiedostoja, sekä ylläpitämään järjestelmää. 

#### Hakemistossa liikkuminen
- `pwd` Näyttää nykyisen hakemiston
- `ls` Näyttää nykyisen hakemiston tiedostot
- `cd [nimi]` Toiseen hakemistoon siirtyminen
- `cd ..` Siirtyy hakemistossa ylöspäin
- `less [nimi].txt` Näyttää tekstitiedoston

#### Tiedostojen muokkaaminen
- `mkdir [nimi]` Luo uuden hakemiston
- `rmdir [nimi]` Poistaa tyhjän hakemiston
- `rm [nimi]` Poistaa tiedoston
- `rm -r [nimi]` Poistaa hakemiston ja sisällön

#### Paketinhallinta
- `sudo apt-get update` Lataa kaikki saatavilla olevat päivitykset
- `sudo apt-get -y install [nimi]` Asentaa ohjelman
- `sudo apt-get purge [nimi]` Poistaa ohjelman

## Asenna micro-editori
Asensin Micron komennolla `sudo apt-get install micro`

Testasin toimivuuden avaamalla sen komennolla `micro`

Microsta pääsee pois painamalla `ctrl + q`

![screenshot](https://i.imgur.com/j2wMKDN.png)

## Asenna kolme uutta komentoriviohjelmaa
Asensin kolme ohjelmaa samanaikaisesti komennolla `sudo apt-get install tldr figlet lshw`

Testasin että asentamani ohjelmat toimivat, lshw testataan myöhemmässä vaiheessa raporttia

![screenshot](https://i.imgur.com/kDFbdct.png)

![screenshot](https://i.imgur.com/dyyUYRj.png)

## Tärkeät hakemistot

`/` Eli juurihakemisto sisältää kaikki muut järjestelmän hakemistot

![screenshot](https://i.imgur.com/QugwuRG.png)

`/home/` Sisältää käyttäjien henkilökohtaiset hakemistot

![screenshot](https://i.imgur.com/lFX8Xtu.png)

`/home/roope/` Käyttäjän "roope" kotihakemisto

![screenshot](https://i.imgur.com/iO2uTqp.png)

`/etc/` Sisältää kaikki järjestelmäasetukset

![screenshot](https://i.imgur.com/TXGuOF0.png)

`/media/` Irroitettava media, kuten cd-levyt tai usb-muistit

`/var/log/` Järjestelmän lokitiedostot

## Grep-komento
Etsitään tiedostosta email-addresses kaikki rivit joissa esiintyy @-merkki

Syötin komennon `grep @ email-addresses`

![screenshot](https://i.imgur.com/m63l0P6.png)

Tässä vielä kuva miltä email-addresses tiedosto näytti

![screenshot](https://i.imgur.com/JlenhWh.png)

Muokkasin komentoa hieman ja syötin `grep -c @ email-addresses`

Tällä sain selville kuinka monessa rivissä @-merkki esiintyy

![screenshot](https://i.imgur.com/K9y7WhO.png)

## Esimerkki putkista

Testasin katsoa /etc/ kansion tiedostoja

Komennolla `ls`:

![screenshot](https://i.imgur.com/Nu2Gj6X.png)

Komennolla `ls | less`:

Tässä valikossa tiedostoja pystyi selaamaan ylöspäin näppäimellä `b` ja alaspäin `space`

![screenshot](https://i.imgur.com/JUmjX21.png)

## Koneen rauta
Asensin jo aiemmin ohjelman lshw, jolla pystyy tarkastelemaan laitteistoa

Listasin ohjelmalla laitteistotiedot komennolla `sudo lshw -short -sanitize`

Kuvasta voimme tulkita että käytössä on VirtualBox, jolle on annettu 8GB ram muistia. Koneen prosessorina toimii AMD Ryzen 7 7800X3D. 

![screenshot](https://i.imgur.com/BWKmTyW.png)

## Lähteet
Karvinen, T. 2024. Linux Palvelimet 2024 alkusyksy https://terokarvinen.com/linux-palvelimet/

Karvinen, T. 2020. Command Line Basics Revisited https://terokarvinen.com/2020/command-line-basics-revisited/

Sykes, A. 2023. CLI tools you won't be able to live without https://dev.to/lissy93/cli-tools-you-cant-live-without-57f6

DicitalOcean 2022. Grep Command in Linux/UNIX https://www.digitalocean.com/community/tutorials/grep-command-in-linux-unix

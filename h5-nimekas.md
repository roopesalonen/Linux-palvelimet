# h5 - Nimekäs

>### Raportissa käytetty ympäristö
>- Debian 12.6.0 Live Xfce
>- VirtualBox 7.0.20
>- CPU: AMD Ryzen 7 7800X3D
>- GPU: Asus GeForce RTX 4060 OC
>- RAM: 32GB DDR5 6000MHz
>- OS: Windows 11 Pro

## a) Kotisivu

Loin kotisivulle asetustiedoston komennolla `sudoedit /etc/apache2/sites-available/roo-pe.me.conf` ja täytin seuraavat tiedot

![screenshot](https://i.imgur.com/kkIhvd8.png)

Loin sivulle kansion komennolla `mkdir -p /home/roo/publicsites/roo-pe.me/` ja lisäsin kansioon tiedostot index.html, info.html ja contact.html sisältöineen micro editorilla

![screenshot](https://i.imgur.com/ImOawmh.png)

Kävin kytkemässä sivun käyttöön komennolla `sudo a2ensite roo-pe.me.conf` ja poistin oletussivun käytöstä komennolla `sudo a2dissite 000-default.conf`

Sen jälkeen käynnistin Apachen uudestaan komennolla `sudo systemctl restart apache2`

![screenshot](https://i.imgur.com/tLtM0nE.png)

Sivu antoi virheilmoituksen "Forbidden", joten jotain puuttui

![screenshot](https://i.imgur.com/Vjzx7Dk.png)

Annoin kaikille lukuoikeudet kotikansiooni komennolla `sudo chmod 755 /home/roo/` ja sivu rupesi näkymään

![screenshot](https://i.imgur.com/7yywvYI.png)

![screenshot](https://i.imgur.com/XK56hJv.png)

![screenshot](https://i.imgur.com/P5s5cTN.png)

## b) Alidomain
Lisäsin alidomainin "info" A-tietueella käyttäen palvelimen ip-osoitetta ja alidomainin "contact" CNAME-tietueella käyttäen palvelimen domainia roo-pe.me

![screenshot](https://i.imgur.com/MR2Jt0e.png)

Kumpikin alidomain osasi ohjata palvelimen etusivulle

![screenshot](https://i.imgur.com/ZMMitIi.png)

![screenshot](https://i.imgur.com/T7D8Edf.png)

## c) Pubkey
Generoin SSH-avaimet virtuaalikoneellani komennolla `ssh-keygen`

![screenshot](https://i.imgur.com/3HQVQFy.png)

Seuraavaksi kopioin SSH-avaimeni palvelimelle komennolla `ssh-copy-id roo@188.166.31.212`

![screenshot](https://i.imgur.com/wNyLpfR.png)

Sen jälkeen pääsin kirjautumaan palvelimelle ilman salasanaa

![screenshot](https://i.imgur.com/HnTBd43.png)

## d) Tutki jonkin nimen DNS-tietoja 'host' ja 'dig' -komennoilla
Tutkin omaa domainiani, Haaga-Heliaa ja Namecheapia host komennolla

Oma sivuni antoi vain yhden IP-osoitteen, kun taas Haaga-Helia antoi usean IP-osoitteen ja Haaga-Helia, sekä Namecheap antoi mail vastauksia

![screenshot](https://i.imgur.com/19fWVER.png)

Kokeilin myös dig komentoa samoihin domaineihin

Kaikki näistä näyttäisi käyttävän A-tietuetta

![screenshot](https://i.imgur.com/yOuOQmg.png)

![screenshot](https://i.imgur.com/WxwTdUN.png)

![screenshot](https://i.imgur.com/ytOT0o7.png)

## Lähteet
Karvinen, T. 2024. Linux Palvelimet 2024 alkusyksy https://terokarvinen.com/linux-palvelimet/

Salonen, R. 2024. Hello Web Server https://github.com/roopesalonen/Linux-palvelimet/blob/main/h3-hello-web-server.md

Uptimia 2024. How Can I Fix The "403 Forbidden" Error In Apache? https://www.uptimia.com/questions/how-can-i-fix-the-403-forbidden-error-in-apache

DigitalOcean 2021. How To Configure SSH Key-Based Authentication on a Linux Server https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server

# h4 - Maailma kuulee

>### Raportissa käytetty ympäristö
>- Debian 12.6.0 Live Xfce
>- VirtualBox 7.0.20
>- CPU: AMD Ryzen 7 7800X3D
>- GPU: Asus GeForce RTX 4060 OC
>- RAM: 32GB DDR5 6000MHz
>- OS: Windows 11 Pro

## x) Lue ja tiivistä
### Teoriasta käytäntöön pilvipalvelimen avulla
- Kannattaa hyödyntää [GitHub Educationia](https://education.github.com/), jolla saa ilmaista palvelinaikaa
- Palvelin tulee suojata palomuurilla (muista portin avaus `sudo ufw allow 22/tpc`)
- Apachea käytettäessä myös sen portti pitää avata (`sudo ufw allow 80/tcp`)

### First Steps on a New Virtual Private Server
- Ainoastaan ensimmäisellä kerralla kirjaudutaan käyttäen juurikäyttäjää (`ssh root@10.0.0.1`)
- Tee itsellesi käyttäjä tarvittavilla oikeuksilla ja lukitse juurikäyttäjä
- Älä käytä heikkoa salasanaa edes väliaikaisesti

## a) Vuokraa oma virtuaalipalvelin haluamaltasi palveluntarjoajalta
Tein tilin [DigitalOceaniin](https://www.digitalocean.com/) hyödyntäen [GitHub Educationin](https://education.github.com/) $200 ilmaista krediittiä vuodeksi tarjousta

![screenshot](https://i.imgur.com/frnsopG.png)

Loin halvan Debian palvelimen Amsterdamista tätä kurssia varten

![screenshot](https://i.imgur.com/dJWs3GL.png)

![screenshot](https://i.imgur.com/fLCfJNj.png)

## b) Tee alkutoimet omalla virtuaalipalvelimellasi
Otin yhteyden palvelimeen juurikäyttäjänä `ssh root@188.166.31.212` (muista käyttää oman palvelimesi osoitetta)

Asensin palomuurin komennolla `sudo apt-get install ufw`

Yritin avata portin palomuuria varten komennolla `sudo ufw allow 22/tcp` mutta sain virheilmoituksen `ERROR: Bad port`

Testasin toisella komennolla `sudo ufw allow from any to any port 22 proto tcp` ja se näytti toimivan (komentoa ehdotti ChatGPT)

Lopuksi laitoin palomuurin päälle komennolla `sudo ufw enable`

![screenshot](https://i.imgur.com/eMmFkPE.png)

Seuraavaksi loin itselleni käyttäjän nimeltä "roo" ja lisäsin sen ryhmiin sudo, adm ja admin

`sudo adduser roo`

`sudo adduser roo sudo`

`sudo adduser roo adm`

`sudo adduser roo admin`

Kirjauduin palvelimelle komennolla `ssh roo@188.166.31.212`

Lukitsin juurikäyttäjän komennolla `sudo usermod --lock root` ja muokkasin SSH asetuksista PermitRootLogin pois päältä `sudoedit /etc/ssh/sshd_config`

Uudelleenkäynnistin vielä SSH:n komennolla `sudo service ssh restart`

![screenshot](https://i.imgur.com/G9oWalt.png)
![screenshot](https://i.imgur.com/y6fk7UC.png)

Tarkistin vielä että kaikki on ajantasalla komennoilla `sudo apt-get update` ja `sudo apt-get upgrade`

![screenshot](https://i.imgur.com/ODmfxai.png)

## c) Asenna weppipalvelin omalle virtuaalipalvelimellesi
Asensin Apachen komennolla `sudo apt-get install apache2`

Avasin portin Apachea varten komennolla `sudo ufw allow 80/tcp`

![screenshot](https://i.imgur.com/lgAGhKO.png)

Sen jälkeen Apachen oletussivu näkyikin jo palvelimen ip osoitteella

![screenshot](https://i.imgur.com/3oPdKOD.png)

Asensin Micro editorin komennolla `sudo apt-get insall micro`

Avasin oletussivun tiedoston komennolla `micro /var/www/html/index.html` ja korvasin sisällön

![screenshot](https://i.imgur.com/PMKzkCk.png)

Näkymä selaimessa sekä kännykässä

![screenshot](https://i.imgur.com/NHOXOfn.png)

![screenshot](https://i.imgur.com/RfgmeEd.png)

## Lähteet
Karvinen, T. 2024. Linux Palvelimet 2024 alkusyksy https://terokarvinen.com/linux-palvelimet/

Lehto, S. 2022. Teoriasta käytäntöön pilvipalvelimen avulla https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/

Karvinen, T. 2017. First Steps on a New Virtual Private Server https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/

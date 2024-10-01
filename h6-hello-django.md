# h6 - Hello Django

>### Raportissa käytetty ympäristö
>- Debian 12.6.0 Live Xfce
>- VirtualBox 7.0.20
>- CPU: AMD Ryzen 7 7800X3D
>- GPU: Asus GeForce RTX 4060 OC
>- RAM: 32GB DDR5 6000MHz
>- OS: Windows 11 Pro

## a) Tee yksinkertainen esimerkkiohjelma Djangolla

Aloitin asentamalla alustan virtuaalisen ympäristön käyttöön komennolla `sudo apt-get -y install virtualenv`

Loin Python3 virtuaaliympäristön ja loin sille env kansion komennolla `virtualenv --system-site-packages -p python3 env/` ja siirryin käyttämään sitä `source env/bin/activate`

![screenshot](https://i.imgur.com/65E9y57.png)

Lisäsin requirements.txt tiedostoon rivin jossa lukee "django" ja suoritin komennon `pip install -r requirements.txt` joka asensi Djangon

Tarkistin vielä Djangon version komennolla `django-admin --version`

![screenshot](https://i.imgur.com/YoY5w0c.png)

Loin Django projektin komennolla `django-admin startproject rooco`

Vaihdoin projektikansioon ja laitoin sen päälle `cd rooco` ja `./manage.py runserver`

![screenshot](https://i.imgur.com/y9UsYu2.png)

Ja nyt Django projekti näkyykin jo localhost portissa 8000

![screenshot](https://i.imgur.com/BERjdKo.png)

Tein migraatiot komennoilla `./manage.py makemigrations` ja `./manage.py migrate`

![screenshot](https://i.imgur.com/RRr2NQ0.png)

Latasin paketin jolla voi generoida salasanoja komennolla `sudo apt-get install pwgen` ja generoin salasanan komennolla `pwgen -s 20 1`

Loin itselleni käyttäjän komennolla `./manage.py createsuperuser` ja käytin generoimaani salasanaa

![screenshot](https://i.imgur.com/VrnYJVO.png)

Laitoin projektin taas päälle komennolla `./manage.py runserver` ja menin testaamaan tunnuksiani osoitteessa http://localhost:8000/admin ja pääsin kirjautumaan sisään

![screenshot](https://i.imgur.com/J6zoeUb.png)

Loin admin käyttäjän generoidulla salasanalla ja painoin "continue editing" jolloin pystyin lisäämään käyttäjälle Staff ja Superuser oikeudet

![screenshot](https://i.imgur.com/C6zVemI.png)

Uusi käyttäjä toimii

![screenshot](https://i.imgur.com/KBGsS9q.png)

Loin crm app:in komennolla `./manage.py startapp crm`

Kävin lisäämässä crm maininnan "INSTALLED_APPS" kohtaan `micro rooco/settings.py`

![screenshot](https://i.imgur.com/hhZHjUv.png)

Lisäsin customer modelin `micro crm/models.py`

    from django.db import models

    class Customer(models.Model):
        name = models.CharField(max_length=300)

Sen jälkeen ajoin taas migraatiot `./manage.py makemigrations` ja `./manage.py migrate`

![screenshot](https://i.imgur.com/dnevbZR.png)

Sitten rekisteröin Customer tietokannan admin sivulle `micro crm/admin.py`

    from django.contrib import admin
    from . import models

    admin.site.register(models.Customer)

Käynnistin projektin `./manage.py runserver`

![screenshot](https://i.imgur.com/f7DtOWm.png)`

Sivulla näkyy nyt Customers tietokanta, mutta nimet näkyvät tyhmästi (Customer object)

![screenshot](https://i.imgur.com/YnvMuUU.png)

Menin takaisin muokkaamaan customer modelia `micro crm/models.py` ja lisäsin kaksi alimmaista riviä

    from django.db import models

    class Customer(models.Model):
        name = models.CharField(max_length=300)

        def __str__(self):
            return self.name

Laitoin projektin taas päälle `./manage.py runserver` ja nyt nimet näkyivät oikein

![screenshot](https://i.imgur.com/4nx81bg.png)

## b) Tee Djangon tuotantotyyppinen asennus

tba

## Lähteet
Karvinen, T. 2024. Linux Palvelimet 2024 alkusyksy https://terokarvinen.com/linux-palvelimet/

Karvinenm T. 2022. Django 4 Instant Customer Database Tutorial https://terokarvinen.com/2022/django-instant-crm-tutorial/

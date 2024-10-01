# h6 - Hello Django

>### Raportissa käytetty ympäristö
>- Debian 12.6.0 Live Xfce
>- VirtualBox 7.0.20
>- CPU: AMD Ryzen 7 7800X3D
>- GPU: Asus GeForce RTX 4060 OC
>- RAM: 32GB DDR5 6000MHz
>- OS: Windows 11 Pro

## a) Tee yksinkertainen esimerkkiohjelma Djangolla

Tehty [Karvisen ohjeen](https://terokarvinen.com/2022/django-instant-crm-tutorial/) mukaan

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

Tehty [Karvisen ohjeen](https://terokarvinen.com/2022/deploy-django/) mukaan

Loin sivulle tiedostot komennoilla `mkdir -p publicwsgi/rooco/static/` ja `echo "testitesti"|tee publicwsgi/rooco/static/index.html`

![screenshot](https://i.imgur.com/3KqSCju.png)

Tein sivulle VirtualHost tiedoston `sudoedit /etc/apache2/sites-available/rooco.conf`

    <VirtualHost *:80>
	Alias /static/ /home/roope/publicwsgi/rooco/static/
	<Directory /home/roope/publicwsgi/rooco/static/>
		Require all granted
	</Directory>
    </VirtualHost>

Laitoin muut sivut pois käytöstä `sudo a2dissite` komennolla ja otin uuden käyttöön `sudo a2ensite rooco.conf`

![screenshot](https://i.imgur.com/5N8YDb9.png)

Käynnistin apachen uudelleen komennolla `sudo systemctl restart apache2` ja testasin että kaikki toimii tähän mennessä `curl http://localhost/static/`

![screenshot](https://i.imgur.com/oTiEGCQ.png)

Siirryin publicwsgi hakemistoon `cd publicwsgi/` ja loin uuden Python3 virtuaaliympäristön `virtualenv -p python3 --system-site-packages env/`

![screenshot](https://i.imgur.com/u8szx2R.png)

Siirryin käyttämään virtuaaliympäristöä komennolla `source env/bin/activate`

Lisäsin requirements.txt tiedostoon tekstin "django" `micro requirements.txt`

Asensin Djangon `pip install -r requirements.txt`

Djangon version tarkistus `django-admin --version`

![screenshot](https://i.imgur.com/1gz8cBr.png)

Kopioin aikaisemmassa tehtävässä tehdyn django projektin [tällä ohjeella](https://www.freecodecamp.org/news/how-to-copy-a-directory-in-linux-use-the-cp-command-to-copy-a-folder/)

Menin muokkaamaan VirtualHost tiedostoa `sudoedit /etc/apache2/sites-available/rooco.conf`

Kopioin sisällön Teron ohjeesta ja muokkasin vain neljä ylintä riviä

Kävin tarkastamassa jokaisen polun ja huomasin että python versio oli päivittynyt 3.9 -> 3.11

    Define TDIR /home/roope/publicwsgi/rooco
    Define TWSGI /home/roope/publicwsgi/rooco/rooco/wsgi.py
    Define TUSER roope
    Define TVENV /home/roope/publicwsgi/env/lib/python3.11/site-packages
    # See https://terokarvinen.com/2022/deploy-django/

    <VirtualHost *:80>
            Alias /static/ ${TDIR}/static/
            <Directory ${TDIR}/static/>
                    Require all granted
            </Directory>

            WSGIDaemonProcess ${TUSER} user=${TUSER} group=${TUSER} threads=5 python-path="${TDIR}:${TVENV}"
            WSGIScriptAlias / ${TWSGI}
            <Directory ${TDIR}>
                WSGIProcessGroup ${TUSER}
                WSGIApplicationGroup %{GLOBAL}
                WSGIScriptReloading On
                <Files wsgi.py>
                    Require all granted
                </Files>
            </Directory>

    </VirtualHost>

    Undefine TDIR
    Undefine TWSGI
    Undefine TUSER
    Undefine TVENV

Asensin Apachen SWGI moduulin `sudo apt-get -y install libapache2-mod-wsgi-py3` ja käynnistin apachen uudelleen `sudo systemctl restart apache2`

Testasin että sivu näkyy oikein `curl -s localhost|grep title` ja `curl -sI localhost|grep Server`

![screenshot](https://i.imgur.com/CL6DWBF.png)

![screenshot](https://i.imgur.com/d0hSmUt.png)

Menin kytkemään debug ominaisuuden pois käytöstä `micro publicwsgi/rooco/rooco/settings.py`

    DEBUG = False
    ALLOWED_HOSTS = ["localhost"]

Päivitin muutokset `touch publicwsgi/rooco/rooco/wsgi.py`

Nyt sivu ei anna käyttäjille ylimääräistä tietoa

![screenshot](https://i.imgur.com/qEGtk2M.png)

http://localhost/admin toimii, mutta on ruman näköinen

Seuraavaksi annoin sille paremman ilmeen

![screenshot](https://i.imgur.com/jkfeZZr.png)

Lisäsin asetuksiin `micro publicwsgi/rooco/rooco/settings.py`

    import os
    STATIC_ROOT = os.path.join(BASE_DIR, 'static/')

Seuraavaksi menin /publicwsgi/rooco/ ja ajoin komennot `source env/bin/activate` ja `./manage.py collectstatic`

![screenshot](https://i.imgur.com/wZ3g15H.png)

Nyt admin sivu näyttää taas paremmalta

![screenshot](https://i.imgur.com/gM3wsgU.png)

## Lähteet
Karvinen, T. 2024. Linux Palvelimet 2024 alkusyksy https://terokarvinen.com/linux-palvelimet/

Karvinenm T. 2022. Django 4 Instant Customer Database Tutorial https://terokarvinen.com/2022/django-instant-crm-tutorial/

Karvinen, T. 2022. Deploy Django 4 - Production Install https://terokarvinen.com/2022/deploy-django/

Olumide S. 2023. How to Copy a Directory in Linux – Use the cp Command to Copy a Folder https://www.freecodecamp.org/news/how-to-copy-a-directory-in-linux-use-the-cp-command-to-copy-a-folder/

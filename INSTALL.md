# DebianUSB

This Finnish document explains how to install Debian 8.1 to USB and make it a base for an Abitti (http://abitti.fi) server replacement.

* Ladataan [Debian 8.1 netinstall](https://www.debian.org/CD/netinst/) (amd64)
* Bootataan kone sillä
* Valitaan boottivalikosta Install

Tässä ohjeessa on käytetty tekstiasennusta.

* Asennuskieleksi English (oletus)
* Country > other > Europe > Finland
* Locale settings > United States (oletus)
* Keymap > Finnish (oletus)
* Hostname: `ktp-moodle`
* Domain name: `moodle-offline.fi`
* Root password: `moodle`
* Full name for the new user: `Moodle Server`
* Username: `moodle`
* Password: `moodle`

Salasanat ja käyttäjän nimet voivat tietysti olla mitä tahansa.

* Partitioning method > Manual

Valitse levyksi USB-levy. Ole huolellinen, ettet yritä asentaa 
käyttöjärjestelmää samalla USB-muistille, jolta olet bootannut koneen ja
jolta siis asennat konetta. Ota levyn polku (esim. `/dev/sdc`) muistiin, koska
tarvitset sitä myöhemmin.

Create new empty partition table > Yes

Nyt luotuun FREE SPACE -alueeseen tehdään vain yksi partitio. Mene ko. 
riville ja paina Enter > Create a new partition > paina Enter > Primary >
Done setting up the partition > Finish partitioning and write changes to disk
> No (ehdottaa paluuta osiovalikkoon) > Yes (kirjoitetaan muutokset levylle).

Nyt asennusohjelma kopioi tiedostot äsken luodulle USB-muistin levyosiolle.
Tämä kestää jonkin aikaa riippuen USB-muistien nopeudesta.

* Mirror country > Finland (oletus)
* Archive mirror > ftp.fi.debian.org (oletus)
* Http proxy: paina Enter (ellei käyttämässäsi verkossa sitten tarvita proxyä)
* Package usage survey > No (oletus)

Valitaan asennettaviksi ohjelmiksi ainostaan "standard system utilities". Eli
ssh-palvelin pois asennettavien listalta.

Nyt palvelin lähtee hakemaan asennettavia paketteja verkosta ja asentaa ne.
Tämä kestää.

Install the GRUB boot... > Yes (oletus)

Valitse valikosta se levy, jolle teit osiot (esim. `/dev/sdc`).

Installation complete > Continue

Kun asennuskäyttöjärjestelmä on ajanut itsensä alas, irrota koneesta
asennusmedian sisältävä USB-muisti. Nyt kone käynnistyy asennettuun Debianiin
ja näet näytössä kirjautumiskehotteen.

ktp-moodle login: `root`
Password: `moodle`

root@ktp-moodle: ln -sf /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

Tämä estää eri koneiden verkkokorttien tallentumisen udev-ohjelman
muistiin.

root@ktp-moodle: apt-get install git

Asentaa git-ohjelman, jolla kopioidaan palvelimelle muutetut tiedostot

root@ktp-moodle: apt-get install dnsmasq

Asentaa dnsmasq-ohjelman, jota käytetään DHCP- ja DNS-palvelimena.

root@ktp-moodle: git clone https://github.com/mplattu/DebianUSB.git

Nyt kopioit Githubista ne tiedostot, jotka on kopioitava asennukseen.
Voit tutkia tiedostoja DebianUSB-hakemistossa.

root@ktp-moodle: cp -r DebianUSB/root/* /

Tämä kopioi kyseisessä hakemistossa olevat tiedostot levyjärjestelmään
oikeille paikoilleen.

Nyt perusasennus on valmis. Voit kokeilla asennusta seuraavasti:
1. Rakenna LAN, jossa on tämä palvelin ja vähintään yksi kokelaan Abitti
-työasema.
2. Käynnistä palvelin.
3. Käynnistä kokelaan Abitti-työasema(t).
4. Avaa kokelaan Abitti-työasemasta pääteikkuna (Applications Menu >
System > Terminator).
5. Anna komento `/sbin/ifconfig`. Jos verkkokortti `eth0` on saanut
IP-osoitteen verkosta 10.10.1.* niin Abitti-työasema on saanut
palvelimelta verkko-osoitteen.
6. Anna komento `cat /etc/resolv.conf`. Jos tiedostossa on rivi
`nameserver 10.10.0.1` on palvelin nimipalvelin.
7. Anna komento `wget http://koe:443`. Jos ohjelma tuloste päättyy
seuraavasti, toimii nimipalvelu oikein:
`Connecting to koe (koe)|10.10.0.1|:443... failed: Connection refused`
Epäonnistuminen johtuu siitä, että palvelimella ei vielä ole ko. porttia
kuuntelelevaa palvelinta.

Tämän jälkeen voit asentaa haluamasi koejärjestelmän. Varmista, että se
kuuntelee nimenomaan porttia 443.

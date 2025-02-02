## Tehtävissä käytetty ympäristö

- Käyttöjärjestelmä: Windows 11 Home 64-bit
- Suoritin: AMD Ryzen 7 5800X 8-Core Processor
- Muisti: 32GB RAM
- Näytönohjain: NVIDIA GeForce RTX 3080
- Virtualisointiohjelmisto: Oracle VM VirtualBox 7.0

- Internet yhteys: DNA Netti 400M (Download 400Mbps / Upload 200Mbps)

## Tehtävä-X

### Name-based & IP-based Virtual Hosts

- IP-based Virtual Hosts
  - Käyttävät yhteyden IP-osoitetta oikean virtuaalipalvelimen valintaan.
  - Jokaiselle virtuaalipalvelimelle tarvitaan oma IP-osoite.
    
- Name-based Virtual Hosts
  - Palvelin nojaa asiakkaan ilmoittamaan hostnameen HTTP headereissa.
  - Useat eri palvelimet voivat jakaa saman IP-osoitteen.
  - Yleensä yksinkertaisempi toteutus.
  - Riittää, kun DNS-palvelimen määrittää liittämään Host-nimet oikeaan IP-osoitteeseen ja Apache HTTP palvelin tunnistaa ne.
    
- Jokaisesta <VirtualHost> lohkosta (block) täytyy löytyä vähintään ServerName ja DocumentRoot määritykset.

### Name-based Virtual Hosts on Apache

- Edellytykset: komentorivikäyttöliittymä, Apache
- Tiivistettynä vaiheittain:
  - Asenna ja määritä Web palvelin
  - Lisää uusi Name-based Virtual Host
  - Luo uusi Web sivusto peruskäyttäjänä
  - Testaus
- Step-by-step ohjeet: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/

## Tehtävä-A

- Testataan, että weppipalvelin vastaa localhost-osoitteesta. Index.html polussa /var/www/

![image](https://github.com/user-attachments/assets/87d02a07-5f0b-4956-905c-9ec15378cb9e)

- Siirrytään selaimeen ja osoiteriviin 'localhost'. Näyttää toimivan, ei tosin tykännyt ääkkösistä.

![image](https://github.com/user-attachments/assets/50a4eaf3-93f6-43d1-917f-55d5719ff43c)

## Tehtävä-B

- Apache lokirivit löytyvät polusta /var/log/apache2
- Komennolla 'sudo ls -l apache2' voidaan tarkastella sisältöä

/var/log/apache2 lokit

![image](https://github.com/user-attachments/assets/07b007e7-907b-4033-bdd4-ac95144daf87)

sudo tail -komennolla nähdään tiedoston 10 viimeisintä riviä. Esimerkkinä access.log

![image](https://github.com/user-attachments/assets/7a0f8486-0f4b-498b-9a9d-7d7182ab9a9d)

Lokeista osaan tulkita ainakin seuraavaa:
- käyttäjän IP-osoite 127.0.0.1
- lokin pyynnön ajankohta
- aikavyöhyke löytyy (+0200)
- HTTP protokollan versio (HTTP/1.1)
- koodi 200 kertoo, että pyyntö onnistui
- Lopussa on selaimen ja käyttöjärjestelmän tietoja (Firefox/128.0, Linux x86_64)

## Tehtävä-C

### Uuden Name-based Virtual Hostin lisääminen

- Ajetaan komento 'sudoedit /etc/apache2/sites-available/hattu.example.com.conf'
- Luodun konfiguraatiotiedoston sisältö tulisi määritellä seuraavanlaiseksi:

![image](https://github.com/user-attachments/assets/0adbfbe1-044c-45ee-bfcf-b60200aa2fca)

- Konfiguraatiotiedoston käyttöön otto tapahtuu komennolla: 'sudo a2ensite hattu.example.com'
- "Vanhojen" konfiguraatiotiedostojen disablointi tapahtuu komennolla: 'sudo a2dissite vanha.example.com'
- Tämän jälkeen "potkaistaan", eli käynnistetään Apache HTTP-palvelin uudelleen: 'sudo systemctl restart apache2'

### Uusi sivu: hattu.example.com

Uuden weppisivuston luominen normaalikäyttäjänä:
  - 'mkdir -p /polku/hattu.example.com'
  - 'echo hattu > /polku/hattu.example.com/index.html'
  
Sivua täytyy pystyä muokkaamaan normaalikäyttäjänä.

Komento 'echo $USER' tulostaa käyttäjätunnuksesi.

Käyttöoikeudet käyttäjälle 'ap' tapahtuu komennolla 'sudo chown -R ap:ap hattu.example.com'

 - 'chown' -> vaihtaa tiedoston tai hakemiston omistajuuden.
 - '-R' -> rekursiivinen valinta, komento periytyy kaikille tiedostoille ja hakemistoille kyseisen hakemiston alihakemistoissa.
 - 'ap:ap' -> uusi omistaja & ryhmä

Omistaja ja ryhmä vaihtunut 'root' -> 'ap'

![image](https://github.com/user-attachments/assets/8a43e435-598e-4d97-afea-c26125139f53)

Simuloidaan nimipalvelua:

- 'sudoedit /etc/hosts/
- lisätään # This host address -listan jatkoksi: '127.0.0.1 hattu.example.com'

![image](https://github.com/user-attachments/assets/f096981c-f53f-4fa1-958e-90198c289b8e)

Päästiin haluttuun lopputulokseen:

![image](https://github.com/user-attachments/assets/e4f701ed-1059-49be-ae33-5b4f0c6db2f5)

## Tehtävä-E

Validi HTML5 sivu näyttää jotakuinkin tältä: 

![image](https://github.com/user-attachments/assets/3e74435a-0418-4281-9919-4dbed0abd8fd)

Näkymä selaimessa:

![image](https://github.com/user-attachments/assets/0354072d-220f-4422-9a9b-5f57512de096)

## Tehtävä-F

Komennolla 'curl' voidaan ladata verkkosivuston sisältö (esimerkiksi 'curl https://google.com')

Komento 'curl -I' lähettää HTTP-pyynnön, joka hakee header-tiedot (metatiedot).

Esimerkki 'curl -I' komennosta:

![image](https://github.com/user-attachments/assets/e6c645af-f975-4415-8c8d-d1a0ec09b717)

Tiedot riveittäin annetun komennon jälkeen:
1. Palvelin vastasi onnistuneesti (koodi 200)
2. Date kertoo milloin palvelin käsitteli pyynnön
3. Palvelimen tiedot
4. Edellinen muokkaus pvm
5. Resurssin uniikki tunniste
6. Kertoo, tukevatko palvelimet osittaista tietojen lataamista
7. Vastaussisällön koko tavuina
8. Kertoo palvelimen käyttäytymisestä välimuistin käsittelyssä pyydetyn resurssin esitystavan osalta.
9. Vastaussisällön tyyppi

### Lähteet:

The Apache Software Foundation. s.a. Name-based Virtual Host Support. Luettavissa: https://httpd.apache.org/docs/2.4/vhosts/name-based.html. Luettu 31.1.2025.

Karvinen, T. 10.4.2018. Name Based Virtual Hosts on Apache - Multiple Websites to Single IP Address. Luettavissa: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/. Luettu 31.1.2025.










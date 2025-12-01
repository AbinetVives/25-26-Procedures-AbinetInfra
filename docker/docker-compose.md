# 🐳 **Docker Compose op een Ubuntu server**


# **1. Werk je Ubuntu server bij**

Open je terminal of SSH naar de server en voer uit:

```bash
sudo apt update
sudo apt upgrade -y
```

**Best practice:**
--> Zorg dat je systeem up-to-date is om compatibiliteitsproblemen te vermijden.


# **2. Installeer Docker en Docker Compose (beste methode)**
Gebruik altijd de officiële Docker-documentatie om Docker en Docker Compose te installeren.  
Deze pagina wordt regelmatig bijgewerkt, waardoor je installatie steeds up-to-date en veilig blijft:

🔗 **Officiële Docker-installatie voor Ubuntu:**  
https://docs.docker.com/engine/install/ubuntu/

## 2.1 Docker’s apt-repository instellen
### 2.1.1 De officiële GPG-sleutel van Docker toevoegen

```bash
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
### 2.1.2 De repository toevoegen aan Apt-bronnen
```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```
## 2.2 Docker-packages installeren
### 2.2.1 Om de nieuwste versie te installeren, voer uit:
```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
> **ℹ️ Opmerking**  
> De Docker-service start automatisch na installatie.
> Controleer of Docker actief is met: 
>
> ```bash
> sudo systemctl status docker
> ```
>
> Op sommige systemen is automatisch starten uitgeschakeld. Dan moet je Docker handmatig starten:
>
> ```bash
> sudo systemctl start docker
> ```

## 2.3 Controleren

```bash
docker --version
docker compose version
```


# **3. Geef je gebruiker toegang tot Docker (aanrader)**

Zodat je geen `sudo` hoeft te typen bij elke opdracht:

```bash
sudo usermod -aG docker $USER
```

--> Log daarna *uit en opnieuw in*.


# **4. Map voorbereiden met je docker-compose bestand**

Zorg dat je `docker-compose.yml` of `compose.yml` op de server staat in de gezamenlijke projectmap.

Als je het vorige stappenplan hebt gevolgd, kun je dankzij de symlink eenvoudig naar de projectmap navigeren:

```bash
cd projectmap
```
Dit brengt je automatisch in:

```bash
/home/ubuntu/projectmap/
```

Ga naar die map:

```bash
/home/ubuntu/projectmap
```

Plaats hier je docker-compose bestand of maak het aan:

```bash
sudo nano docker-compose.yml
```
Controleer of het bestand aanwezig is:
```bash
ls -l
```


# **5. .env-bestand gebruiken**

Een `.env`-bestand wordt vaak gebruikt om configuratievariabelen op te slaan die je niet rechtstreeks in je `docker-compose.yml` wilt zetten. Denk hierbij aan:

- poorten  
- wachtwoorden  
- databasegegevens  
- API-instellingen  

Het `.env`-bestand moet in **dezelfde map** staan als `docker-compose.yml`.

Maak of bewerk het `.env`-bestand:

```bash
nano .env
```

**Best practices:**
- Zet *nooit* wachtwoorden, tokens of andere gevoelige gegevens in `docker-compose.yml`.
- Houd je `.env`-bestand privé en upload het *nooit* naar GitHub of een andere publieke repository.
- Voeg `.env` altijd toe aan `.gitignore`, zodat het niet per ongeluk wordt meegestuurd.
- Een `.env.example` mag wél gepubliceerd worden, maar altijd **zonder echte geheimen**.


# **6. Docker Compose uitvoeren**

Start alle services:

```bash
docker compose up -d
```

Uitleg:

* `up` → maakt containers aan en start ze
* `-d` → draait ze op de achtergrond


# **7. Status controleren**

### 7.1 Welke containers draaien:

```bash
docker compose ps
```

### 7.2 Alle Docker containers:

```bash
docker ps
```


# **8. Logs bekijken (handig bij fouten)**

```bash
docker compose logs -f
```

Stop logs volgen met **CTRL + C** (containers blijven draaien).


# **9. Applicatie testen**

Als je docker-compose een webservice heeft, staat er meestal zoiets in het bestand:

```yaml
ports:
  - "8080:80"
```

Dat betekent dat je naar je browser gaat en:

```
http://SERVER-IP:8080
```

invoert.


# **10. Services opnieuw starten**

Als je een wijziging maakte aan je compose-bestand:

```bash
docker compose down
docker compose up -d --build
```


# **11. Stoppen en opruimen**

### 11.1 Containers stoppen:

```bash
docker compose down
```

### 11.2 Ook volumes verwijderen (pas op: dit wist data!)

```bash
docker compose down -v
```


# 🔐 **12. Best Practices**

### **Security**

* Gebruik `.env` voor wachtwoorden

* Open enkel de poorten die nodig zijn: geen poorten van de database, backend naar buiten mappen!!


### **Stabiliteit**

* Gebruik volumes voor data (bv. databases):

  ```yaml
  volumes:
    - db_data:/var/lib/mysql
  ```

* Herstart policies instellen:

  ```yaml
  restart: unless-stopped
  ```


### **Onderhoud**

* Regelmatig updates uitvoeren:

  ```bash
  docker compose pull
  ```

* Gebruik duidelijke servicenamen:

  ```yaml
  services:
    web:
    database:
    cache:
  ```

---


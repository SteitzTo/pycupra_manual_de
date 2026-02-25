# Anleitung für pycupra

Vollständige Installation von pycupra von [WulfgarW](https://github.com/WulfgarW/pycupra)<br> 
um Cupra/Seat Fahrzeuge abfragen


## 1. Abhängigkeiten:
Python 3.12.0<br>
uv für die Abhängikeiten zu installieren<br>
pycupra library von [WulfgarW](https://github.com/WulfgarW/pycupra)<br>

## 2. Installation von Python 3.12.0

```
sudo apt update && sudo apt upgrade
```

### 2.1  Installation von Python 3.12.0 in einer virtuellen Umgebung

Als erstes überprüfen wir die aktuelle Python Installation:

```
python --version
```
Bei mir ist das 3.11.2, wir benötigen aber Python 3.12.0 mit einer virtuellen Umgebung.

### 2.1.1 Abhängigkeiten installieren

```
sudo apt install build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev curl \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev -y
```
### 2.1.2 Python 3.12.0 herunterladen und entpacken
```
wget https://www.python.org/ftp/python/3.12.0/Python-3.12.0.tgz
tar -xf Python-3.12.0.tgz
cd Python-3.12.0
```
### 2.1.3 Python kompilieren

```
./configure --enable-optimizations
make -j$(nproc)
sudo make altinstall
```
Das ganze dauert jetzt etwas.....

## 3. Installation von pycupra

```
cd ~
git clone https://github.com/WulfgarW/pycupra.git
```


### 3.1 . uv installieren

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```
Wenn alles installiert ist, müssen wir uv noch einbinden:
```
source $HOME/.local/bin/env
```
danach überprüfen wir mit:
```
uv --version
```
die installierte Version.

Nun wechsel wir ins pycupra Verzeichnis und führen aus:
```
uv sync
```
um die Abhängikeiten des pycupra Packetes zu installieren. 


### 3.2 Erstellen eine virtuellen Umgebung mit dem Namen pycupra_env im pycupra Verzeichnis
```
cd pycupra
python3.12 -m venv pycupra_env
```
Aktiviere die Python 3.12.0 Umgebung
```
source pycupra_env/bin/activate
```
Die Umgebung ist aktiviert wenn am Zeilenanfang im Terminal (pycupra_env) steht.

Überprüfe die Version mit
```
python --version
```
Wenn jetzt die Anzeige "Python 3.12.0" erschienen ist, bis bis jetzt alles richtig gelaufen.



## 4. Konfigurationsfile erstellen

im Verzeichnis pycupra:

```
cp pycupra_credentials.json.demo pycupra_credentials.json
nano pycupra_credentials.json
```
```
{"username": "your_user_name_for_MyCupra_or_MySeat", 
 "password": "your_password_for_MyCupra_or_MySeat",
 "spin": "1234",
 "brand": "cupra",
 "apiKey": "a_valid_API_key_for_the_maps.googleapis.com",
 "csvFileName":  "c:/users/myusername/Documents/drivingData.csv",
 "_comment": "Delete the line apiKey from your credentials file if you don't have a valid apiKey."
}
```
Jetzt folgendes Editieren:

username: Hier die E-Mailadresse eintragen welches ihr in der Cupra/Seat App benutzt.<br>
password: Das passenden Passwort dazu.<br>
spin: Hier die 4 stellige S-PIN eintragen, welches ihr für das Fahrzeug festgelegt habt.<br>
brand: Hier den Fahrzeughersteller eintragen, cupra oder seat.<br>
apiKey: Hier einen API-Key von Google Maps eintragen um diese Feature zu nutzen. Falls man keinen Key hat, diese Zeile bitte löschen.<br> 
csvFileName: Hier den Pfad eintragen wo die ExelTabelle mit den Kraftstoffverbräuchen angelegt werden soll. <br>
<br>
Jetzt alles mit "Strg" + "O" Speichern und Nano mit "Strg"+"X" verlassen.

Danach die Datei pycupra_credentials.json nach ./pycupra/example kopieren.

## 5. Daten abrufen

```
cd ~
cd pycupra
cd example
```
und die Datei mit Python ausführen:
```
python PyCupra.py
```
Das Script sollte nun laufen und den Server nach allen möglichen Werten abfragen.

Alle Werte und Zustände werden in der Datei pycupra_all_attributtes.txt gespeichert,
und können somit weiterverarbeitet werden.


## 6. Firebase Massaging abschalten 

Um das Firebase-Messaging abzuschalten bitte folgendes vornehmen:

In der Datei PyCupra.py in der Zeile 270 folgendes eintragen:

```
# Suche diese Zeilen:
if vehicle == connection.vehicles[0]: 
    # Diese Zeile unten sorgt dafür, dass Firebase Messaging gestartet wird:
    newStatus = await vehicle.initialiseFirebase(FIREBASE_CREDENTIALS_FILE_NAME_AND_PATH, vehicle.update)
    print('########################################')
    print('#      Initialisation of firebase      #')
    # ... usw.
```
ersetze durch:
```
# if vehicle == connection.vehicles[0]: 
#     newStatus = await vehicle.initialiseFirebase(FIREBASE_CREDENTIALS_FILE_NAME_AND_PATH, vehicle.update)
#     print('########################################')
#     print('#      Initialisation of firebase      #')
#     print(txt.center(40, '#'))
#     print(f"New status of firebase={newStatus}")

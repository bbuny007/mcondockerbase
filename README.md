# mcondockerbase
eine basis configuration um auf der docker container plattform ein minecraft server zu machen
# Variablen (setup part 0.5) 

Die Datei mit einem beliebigen text editor öffnen und 

einfach auf die code kommentare achten die fangen mit # an und es ist immer das drunter gemeint 
es sollte so aussehen:

```
version: "3.4"
services:
  mc:
#das so zu agen abbild/code des containers mit :latest um die letztn updates immer zu bekommen
    image: itzg/minecraft-server:latest
    restart: unless-stopped
    container_name: minecraftserver-mc
    environment:
#ram minimum 2 gigabyte weil es sonst sehr instabil
      MEMORY: 2G
#der typ vom server paper ist ein leistungstarker server file der erlaubt plugin von spigot und bukkit zu installieren (für mehr siehe github)
      TYPE: paper
#die AGBs von mojang/microsoft die sind hier https://www.minecraft.net/de-de/eula
      EULA: true
#letzte version ist aber auf dem paper typ nur 1.21.3 (16:22 05.12.2024)
      VERSION: LATEST 
# maximale spieler anzahl
      MAX_PLAYERS: 10
#man kann seeds googlen das ist nur für den zufall generator dass der immer das gleiche macht wenn man es ändert kann die weld anders weden (um zufällig zu generieren kann man auch leer lassen)
      SEED: 7598363429859286000 
#nicht wichtig aber sollte man auch ändern wenn man mehrere server hat
      SERVER_NAME: server 
#der resourcen paket link ist faithful x64 kann man aus kommentieren wenn es zu laggs führ oder nicht nötig gestellt werden siehe 2 zeilen runter
      RESOURCE_PACK: https://download.mc-packs.net/pack/ecf781013a05fbd19605f57f2eecf9b2c0a5b8aa.zip 
#der sha1 vom resourcen paket der ist updates vom paket zu erfassen 
      RESOURCE_PACK_SHA1: ecf781013a05fbd19605f57f2eecf9b2c0a5b8aa 
# kann man auf false ändern dass der server nicht zwingt den recourcen paket zu aktiviren
      RESOURCE_PACK_ENFORCE: true 
#sollte auch wenn das wenig ist so bleiben weil sonst zu viel cpu vom pi verwendet wird und es laggt
      SIMULATION_DISTANCE: 4 
# der radius den admins haben vom welt spawn aus die admins nur rechte haben ab/zu zu bauen
      SPAWN_PROTECTION: 0 
#command blöcke aktiviren sollte anbleiben weil eh ken normaler spieler an sie dran kann mit überlebens modus
      ENABLE_COMMAND_BLOCK: true
#extra performance 
      USE_AIKAR_FLAGS: true 
#das was in der zweiten reihe in minecraft in der server liste steht
      MOTD: Ein Mineraft Server.(auch aendern) 
    ports:
#port ändern wenn man mehrere server hat (man muss es auch in den server.properties ändern)
      - 25565:25565
#nur entkommentieren wenn man crossplay mit konsole und windows edition machen will siehe im github
      #- "19132-19133:19132" 
    networks:
      - minecraftservernet
    volumes:
#das vorm :/data ändern auf den physischen ort im dateien system z.B /home/pi/mc mann muss es aber mit mkdir erstellen
      - /opt/stacks/mc:/data
networks:
  minecraftservernet:
```

## Setup (rpios)
(erstmal die datei in den releases configuriert weil das ist user spezifisch z.B. der ort für die dateien)

Natürlich ersmal im terminal oder mit ssh zugriff updaten alless

`sudo apt update && sudo apt upgrade`

Danach die abhängigen prüfen mit [diesem link](https://docs.docker.com/engine/install/) 

um sicher zu sein 

`sudo docker run hello-world`

### Man kann alles ignorieren obere im setup wenn man casa os oder ein anderes docker webui hat oder oder docker und docker compose schon installiert ist
## setup part 2 

die oben genannte ordner erstellen und

einfach im web ui die docker-compose.yml datei aus wählen und ausführen oder im ordner der datei 

`sudo docker compose up -d`

# Extra nach insterlations setup

## plugins 

plugins sind mods die nicht jeder herunter laden muss die befinden sich nur im server deswegen sind die auch limitiert z.B können sie keine neuen blöcke und so hinzufügen
aber dennoch sind sie noch wichtig!
wie schon in der config kann man plugins von [hier(spigot)](https://www.spigotmc.org/resources/) und [hier(bukkit)](https://dev.bukkit.org/bukkit-plugins) und [hier(hangar)](https://hangar.papermc.io/) herunter laden
manche alte plugins können nicht aktuell sein aber manche sind auch wenn sie seid jahren geupdated wurden z.B Vault (ein econome und berechtigung zusammenfür plugin)
die macht man zur info in ./datenordner/plugins sehr einfach ich weiß und die configuriet man dann in dem selben ordner aber dann in einem ordner das wie das plugin heißt
###wichtie plugins

- [geyser](https://geysermc.org/download)
  
  ein plugin der cross play erlaubt aber dafür braucht man auch floodgate und ein extra port der offen ein muss flood gate erlaubt es einen ohne java account beizutreten.

- [essentialsX](https://essentialsx.net/downloads.html)

  ein administrator plugin der sehr viele qualitäts admin commandos die mit / im chat anfangen hinzufügt

- [luckperms](https://luckperms.net/)
  ein plugin mit einem editor der berechtigungen serleicht macht der editor kann mit (/lp editor)  geöffnet erdem man muss den link im chatan klicken zum glück hat es auch ne deutsche website



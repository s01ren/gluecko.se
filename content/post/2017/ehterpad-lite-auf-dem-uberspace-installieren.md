+++
title = "Etherpad-Lite auf dem Uberspace installieren"
date = "2017-12-23"
layout = ["article"]
author = "soeren"
tags = ["uberspace", "open-source"]
+++

Dieser Tage war ich auf der Suche nach einem Tool zur gemeinsamen *Idennsammlung*. Konkret wollte ich was, in dem ich Links und Stichpunkte zur Vorbereitung einer Reise sammeln kann. Idealerweise sollte das natürlich web-basiert sein, das gleichzeitige Bearbeiten von mehreren Personen unterstützen und [open source](/tags/open-source) zur Verfügung stehen. Entschieden habe ich mich für [Etherpad-Lite](https://github.com/ether/etherpad-lite).

Hier zeige ich euch, wie ihr das Pad auf eurem [uberspace](/tags/uberspace) zum Laufen bekommt. Doch vornweg: **Ja,** ich weiß, dass das [uberspace Wiki](https://wiki.uberspace.de/cool:etherpadlite) bereits eine Anleitung dazu enthält. Wenn ihr das Pad jedoch auf einer Subdomain betreiben, hübsche URLs haben und gleich noch zwei, drei nützliche Plugins installieren wollt - solltet ihr hier weiterlesen :-)

## Voraussetzungen

Du kannst dich via SSH auf deinem [uberspace](/tags/uberspace) einloggen. Falls du einen uberspace nutzt und nicht weißt wie das geht (unwahrscheinlich!), hier nochmal der Link zum [Wiki](https://wiki.uberspace.de/system:ssh). Im folgenden Beispiel gehen wir davon aus, dass dein Nutzername `batman` ist und dein Account auf `helium.uberspace.de` liegt. Batman hat natürlich auf dem [allerersten uberspace](https://wiki.uberspace.de/faq?s%5B%5D=hosts#wonach_benennt_ihr_eure_server) einen Account bekommen ;-)

Du kennst deine Datenbankpasswort für MySQL. Falls nicht, kannst du es mit folgendem Befehl herausfinden: `[batman@helios ~] cat ~/.my.cnf` (Zeile, die mit `password=` beginnt). Lies zur Sicherheit noch einmal den [Wiki-Artikel](https://wiki.uberspace.de/database:mysql).

## Vorbereitungen

Öffne Adminer und erstelle eine neue Datenbank für Etherpad-Lite. Adminer findest du stehts unter `https://adminer.<dein_uberspace>.uberspace.de`, in unserem Beispiel also unter: https://adminer.helium.uberspace.de.

Logge dich dort mit deinem uberspace-Namen und deinem Datenbankpasswort (s.o.) ein und erstelle eine neue Datenbank. **Beachte dabei die [Namenskonventionen](https://wiki.uberspace.de/database:mysql#weitere_datenbanken).**


Wir müssen noch einen freien Port finden. Überlege dir dafür eine Zahl **zwischen 61000 und 65535**. Wenn du dich beispielsweise für *61234* entschieden hast, verbinde dich per SSH mit deinem uberspace und teste, ob dieser Port noch frei ist. Setze dafür die Portnummer nach `grep` im Befehl ein: 

```
[batman@helium ~] netstat -tulpen | grep 61234
``` 

Wenn auf den Befehl keine Ausgabe erfolgt, ist der Port noch frei. **Notiere dir den Port!**

## Los gehts

Stelle zunächst eine SSH-Verbindung zu deinem uberspace her:

```
[batman@local] ssh batman@helium.uberspace.de
```

Dann legen wir eine Konfigurationsdatei für npm an. Diese wird benötigt da, Etherpad-Lite einige Module mittels npm nachinstallieren möchte und dafür wissen muss wo die hin und welche Rechte die haben sollen:

```
[batman@helium ~] cat > ~/.npmrc <<__EOF__
prefix = $HOME
umask = 077
__EOF__
```

Prüfe nun, welche MySQL-Version auf deinem uberspace zur Verfügung steht:

```
[batman@helium ~] mysql -V
```

Als nächstes installieren wir Etherpad-Lite. Das klonen wir am einfachsten aus dem [Git-Repository](https://github.com/ether/etherpad-lite/). Von deiner MySQL-Version hängt ab, **welches Release du laden solltest**. Steht dir Version 5.1 zur Verfügung, darfst du **maximal Version 1.5.7** von Etherpad-Lite installieren. Hast du die MySQL Version 5.5, kannst du das neuste Release von Etherpad-Lite laden. **Tausche also ggf. im folgenden Befehl die Versionsnummer aus:**

```
[batman@helium ~] git clone -b release/1.5.7 https://github.com/ether/etherpad-lite.git
```

{{< rawhtml >}}
<div style="padding:20px;margin:0px;box-shadow:0 10px 6px -6px #777;background-color:#4cb770;color:white;">
<span style="font-size:100%;"><b>Achtung:</b> wie im <a href="https://wiki.uberspace.de/cool:etherpadlite">Uberspace-Wiki</a> angegeben, kannst du derzeit nur dann die aktuellste Version von Ehterpad-Lite installieren, wenn du bereits auf einem Uberspace 7 bist. Solltest du noch MySQL 5.1 haben, musst du Etherpad-Lite in Version 1.5.7 installieren.</span>
</div><br>
{{< /rawhtml >}}

Wechlse in den Etherpad-Lite Ordner, stelle sicher, dass der richtige Branch gewählt ist und installiere Etherpad-Lite:
```
[batman@helium ~] cd etherpad-lite
[batman@helium etherpad-lite] git checkout release/1.5.7
[batman@helium etherpad-lite] bin/installDeps.sh
```

Im nächsten Schritt passen wir die Konfiguration für Etherpad-Lite an. **Achtung:** Das ist der Punkt an dem am meisten schief geht. Sei hier also bitte besonders akribisch. 

**Bevor** du weitermachst, prüfe noch einmal, ob

  * du deine MySQL-Zugangsdaten parat hast,
  * eine MySQL-Datenbank für Etherpad-Lite angelegt hast,
  * dir den freien Port notiert hast

Erstelle eine Kopie der Vorlage und öffne diese in einem Texteditor (bspw. nano):

```
[batman@helium etherpad-lite] cp settings.json.template settings.json
[batman@helium etherpad-lite] nano settings.json
```

Innerhalb der `settings.json` passe folgende Zeilen an:

* ersetze in folgendem Block den Port mit dem, den du dir eingangs notiert und für noch nicht belegt befunden hast. Aktuell steht das in Zeile 19.
  ```
  ...
  //Ip and port which etherpad should bind at
  "ip": "0.0.0.0",
  "port" : 61234,
  ...
  ```
* Scrolle bis zu folgendem Block und lösche den gesamten Absatz zu `"dbType": "dirty"`. Aktuell betrifft das die Zeilen 41 bis 47.
  ```
  ...
  "dbType" : "dirty",
  //the database specific settings
  "dbSettings" : {
                   "filename" : "var/dirty.db"
                 },
  ...
  ```
* Passe die Zugangsdaten und den Datenbanknamen für MySQL an. Das steht derzeit ab Zeile 48.
  ```
  ...
  "dbType" : "mysql",
  "dbSettings" : {
                  "user"    : "<dein Uberspace Name>",
                  "host"    : "localhost",
                  "password": "<dein Datenbank Passwort>",
                  "database": "<dein Datenbank Name>"
                 },
  ...
  ```
* Definiere das Administrator-Passwort. Der Block sollte schon angelegt sein, du musst lediglich das Password ändern (default: `changeme1`). Entferne die Kommentierung (`/*` bzw. `*/`) um den Block und bereits angelegte user.
  ```
  ...
  /* Users for basic authentication. is_admin = true gives access to /admin.
  If you do not uncomment this, /admin will not be available! */

  "users": {
    "admin": {
      "password": "BatmanRocks!",
      "is_admin": true
    }
  },
  ...
  ```
* Speichere die Änderungen mit `STRG + O` und schließe nano mit `STRG + X`
* Die Konfigurationsdatei erlaubt noch wesentlich mehr Einstellungen. In einer ruhigen Minute kannst du sie dir ja mal bei einer Tasse Kaffe durchlesen.

Als nächstes richten wir Etherpad-Lite als Dienst ein:

```
[batman@helium etherpad-lite] cd ~
[batman@helium ~] test -d ~/service || uberspace-setup-svscan
[batman@helium ~] uberspace-setup-service etherpad-lite node ~/etherpad-lite/node_modules/ep_etherpad-lite/node/server.js
```

Öffne die `run`-Datei des Dienstes:

```
[batman@helium ~] nano ~/service/etherpad-lite/run
```

Füge unter `# Now let's go!` die folgenden beiden Zeilen ein.
```
...
# Now let's go!
cd ~/etherpad-lite
bin/installDeps.sh 2>&1
exec <... bleibt unverändert>
```

Speichere die Änderungen mit `STRG + O` und schließe nano mit `STRG + X`
Starte den Dienst neu:

```
[batman@helium ~] svc -du ~/service/etherpad-lite
```

## Die harte Arbeit ist getan. Jetzt haben wir uns eine kleine Verschnaufspause verdient.

{{< rawhtml >}}
<img src="https://media.makeameme.org/created/take-a-break-09utnz.jpg" alt="Take a Break!">
{{< /rawhtml >}}


## Weiter gehts

Bevor wir uns mit der Brücke zum Webserver beschäftigen, testen wir noch einmal, ob wir alles richtig gemacht haben:

```
[batman@helium ~] svstat ~/service/etherpad-lite
```

Idealerweise sollte in der Ausgabe eine Zeit > 10 Sekunden stehen. Führe den Befehl im Zweifel mehrfach aus. Wenn sich die pid ändert, ist das ein schlechtes Zeichen.

Wenn alles läuft, springe zum nächsten Schritt. Ansonsten, wirf einen Blick ins [Log](https://wiki.uberspace.de/system:daemontools#logging) - das sollte Aufschluss über den Fehler geben (vermutlich stimmt was in der `settings.json` nicht).

```
[batman@helium ~] tail ~/service/etherpad-lite/log/main/current | tai64nlocal
```

Fahre erst fort, wenn alles passt!

## Wenn er Dienst fehlerfrei läuft

Dann fehlen uns jetzt bis zum ersten Pad nur noch zwei Dinge: 1) Eine Subdomain unter der wir Etherpad-Lite erreichen wollen und 2) eine `.htaccess`-Datei.

Erzeugen wir zunächst die Subdomain. Im Beispiel gehen wir davon aus, dass dir die Domain https://batman.io gehört (auf deinem [uberspace aufgeschaltet](https://wiki.uberspace.de/domain:verwalten) ist) und du das Pad gern unter https://pad.batman.io betreiben willst. Erstelle also den folgenden Ordner (und passe dabei den Teil nach `/virtual/` an deine Gegebenheiten an):

```
[batman@helium ~] mkdir /var/www/virtual/batman/pad.batman.io
```

Lege in diesem Ordner die `.htaccess` an und öffne sie mit nano:

```
[batman@helium ~] cd /var/www/virtual/batman/pad.batman.io
[batman@helium pad.batman.io] touch .htaccess
[batman@helium pad.batman.io] nano .htaccess
```

Füge nun folgenden Text in die Datei ein und **setze unter `# Bridge to Proxy port` deinen eingangs gewählten Port** (bspw. *61234*) ein.

```
# Rewrite on
RewriteEngine On

# Force https
RewriteCond %{HTTPS} !=on
RewriteCond %{ENV:HTTPS} !=on
RewriteRule .* https://%{SERVER_NAME}%{REQUEST_URI} [R=301,L]

# Bridge to Proxy port
RewriteRule (.*) http://localhost:<dein_port>/$1 [P]
```

***Update 26.Juli 2018:** In einer früheren Version stand in der `.htaccess` ein zusätzlicher Block, um die URLs schöner zu formatieren. Dieser ist nicht (mehr?) notwendig. Vielen Dank für den Hinweis.*

Falls du das Pad nicht über **https** nutzen **willst**, lösche einfach den Absatz `# Force https`. Falls du es (noch) nicht **kannst**, lies diesen Artikel im [uberspace Wiki](https://wiki.uberspace.de/webserver:https).

## Starte dein erstes Pad

Navigiere zu der eben erstellten Subdomain und erstelle dein erstes Pad.

## Genieße deine neue Etherpad-Installation

{{< rawhtml >}}
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ8RxBAaUrsQ7dsDFBUW-MUCpE8ngaSiCiYyGl60mG7SMruGXTX" alt="Enjoy">
{{< /rawhtml >}}

## Installiere ein paar nützliche Plugins

Um ein bisschen mehr Abwechslung in dein Pad zu bekommen, solltest du noch ein paar Plugins installieren. Das geht ganz einfach über die Admin-Oberfläche. Dorthin kommst du, indem du `/admin` an die URL deiner Etherpad-Installation anfügst und dich mit den in der `settings.json` definierten Zugangsdaten anmeldest.

Ich empfehle folgende Plugins:

  * `adminpads`: Zeigt dir eine Liste aller Pads auf deiner Installation, u.a. mit der Möglichkeit diese zu löschen.
  * `font_color`: Erlaubt es, die Schriftfarbe im Pad zu ändern.
  * `font_family`: Erlaubt es, die Schriftart im Pad zu ändern.
  * `font_size`: Erlaubt es, die Schriftgröße im Pad zu ändern.
  * `headings2`: Erlaubt das Setzen von Überschriften im Pad.

## Yeah!

Geschafft. Du hast jetzt ein lauffähiges Pad auf deinem Uberspace :)

{{< rawhtml >}}
<img src="http://mymemes.biz/wp-content/uploads/2017/10/meme-success.png" alt="Yeah!">
{{< /rawhtml >}}

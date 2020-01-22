+++
title = "Update Ghost (> 1.X.X) auf dem uberspace"
date = "2017-09-24"
layout = ["article"]
author = "soeren"
tags = ["uberspace", "open-source", "blog", "ghost"]
+++

Bei [Ghost](https://ghost.org/de/) geht es in letzter Zeit richtig vorwärts. Kaum zwei Monate ist es her, dass nach langem Warten (3 Jahre??) endlich [Version 1.0.0](https://github.com/TryGhost/Ghost/releases/tag/1.0.0) veröffentlicht wurde. Als ich letzte Woche [dieses Blog](https://gluecko.se) neu eingerichtet habe, waren wir bereits bei Version 1.8.irgendwas - und heute erhielt ich bereits den Hinweis, dass [Version 1.9.0](https://github.com/TryGhost/Ghost/releases/tag/1.9.0) verfügbar sei.

Ein guter Anlass auszuprobieren, wie das Update auf die neue Version auf dem uberspace klappt. Ein kleiner Tipp vorweg - so einfach wie in der [Ghost Documentation](https://docs.ghost.org/docs) beschrieben ist es zwar nicht. Ein Hexenwerk aber auch nicht.

{{< rawhtml >}}
<div style="padding:20px;margin:0px;box-shadow:0 10px 6px -6px #777;background-color:#4cb770;color:white;">
<span style="font-size:100%;"><b>[update] Jetzt noch einfacher:</b><br>
    Lies den <a href="/post/2017/bringe-ghost-mittels-shell-skript-auf-den-neusten-stand">neuen Beitrag</a> und lerne, wie du Ghost über ein Shell-Skript noch einfacher auf den neusten Stand bringen kannst.</span>
</div>
<br>
{{< /rawhtml >}}

## Voraussetzungen

Du kannst dich via SSH auf deinem [uberspace](/tags/uberspace) einloggen. Falls du einen uberspace nutzt und nicht weißt wie das geht (unwahrscheinlich!), hier nochmal der Link zum [Wiki](https://wiki.uberspace.de/system:ssh). Im folgenden Beispiel gehen wir davon aus, dass dein Nutzername `batman` ist und dein Account auf `helium.uberspace.de` liegt. Batman hat natürlich auf dem [allerersten uberspace](https://wiki.uberspace.de/faq?s%5B%5D=hosts#wonach>*benennt*ihr*eure*server) einen Account bekommen ;-)

Du weißt, in welchem Verzeichnis du Ghost installiert hast. Im Beispiel gehen wir von `~/ghost` aus.

Du weißt, in welchem Verzeichnis du den Ghost-Dienst eingerichtet hast. Im Beispiel gehen wir von `~/service/ghost` aus.

 ## Ghost updaten

  1. Stelle zunächst eine SSH-Verbindung zu deinem uberspace her:
     ```
     [batman@local] ssh batman@helium.uberspace.de
     ```
  1. Stoppe zur Sicherheit den Ghost-Dienst:
     ```
     [batman@helium ~] svc -d /service/ghost/
     ```
  1. Wechsle in das Ghost-Verzeichnis:
     ```
     [batman@helium ~] cd ~/ghost/
     ```
  1. Sichere deine Konfigurationsdatei:
     ```
     [batman@helium ghost] mv core/server/config/env/config.production.json core/server/config/env/config.production.json_bak
     ```
  1. Lade die neuste Version herunter:
     ```
     [batman@helium ghost] curl -L <https://ghost.org/zip/ghost-latest.zip> -o ghost.zip
     ```
  1. Entpacke die zip-Datei und lösche sie danach:
     ```
     [batman@helium ghost] unzip -o ghost.zip && rm ghost.zip
     ```
  1. Überschreibe die neue Konfigurationsdatei mit dem zuvor angelegten Backup
     ```
     [batman@helium ghost] mv -f core/server/config/env/config.production.json_bak core/server/config/env/config.production.json
     ```
  1. Aktualisiere die Abhängigkeiten:
     ```
     npm install --python="usr/local/bin/python2.7" --production
     ```
  1. Migriere die Datenbank:
     ```
     NODE_ENV=production knex-migrator migrate
     ```
  1. Starte den Ghost-Dienst neu:
     ```
     svc -du ~/service/ghost
     ```
  1. Erfreue dich an deiner aktualisierten Ghost-Installation :)



Danke an [Ben](https://dev.kampfq.eu/2017/08/02/update-von-ghost-1-0-auf-dem-uberspace/).
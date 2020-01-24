+++
title = "Bringe Ghost mittels Shell Skript auf den neusten Stand"
date = "2017-11-13"
layout = ["article"]
author = "soeren"
tags = ["uberspace", "open-source", "blog", "ghost"]
+++

Wie bereits in meinem [letzten Beitrag](/post/2017/update-ghost-1.x-auf-uberspace) angedeutet, dreht Ghost aktuell ordentlich an der Update-Schraube. Das manuelle Update ist auf dem uberspace zwar kein großer Aufwand - es geht aber natürlich noch einfacher. Lerne jetzt, wie du deine Ghost-Installation mittels Shell-Skript auf dem neusten Stand hältst.

{{< rawhtml >}}
<div style="padding:20px;margin:0px;box-shadow:0 10px 6px -6px #777;background-color:#4cb770;color:white;">
<span style="font-size:100%;">Lies zur Vollständigkeit am besten noch einmal den <a href="/update-ghost-1-x-x-auf-dem-uberspace/">ersten Beintrag</a> zum Thema.</span>
</div>
<br>
{{< /rawhtml >}}

Jetzt aber nichts wie los! Und weil es ja Zeit sparen soll, hier nur die einzelnen Schritte:

  * Stelle zunächst eine SSH-Verbindung zu deinem uberspace her:
    ```
    [batman@local] ssh batman@helium.uberspace.de
    ```
  * Lade mein Skript herunter in dein HOME-Verzeichnis:
    ```
    batman@helium ~] wget -O ~/update*ghost.sh https://gitlab.com/s01ren/glueckose/code/raw/master/update_ghost.sh
    ```
  * Mache das Skript ausführbar:
    ```
    [batman@helium ~] chmod 700 update_ghost.sh
    ```
  * Starte das Skript:
    ```
    [batman@helium ~] ./update_ghost.sh
    ```
  * Erfreue dich an deiner aktualisierten uberspace-Installation :)

Nächster Schritt: Skript automatisch ausführen lassen sobald eine neue Version verfügbar ist.

+++
title = "Hilf mir (Rechtschreib-)Fehler zu beheben"
date = "2019-11-11"
layout = ["article"]
author = "soeren"
tags = ["github", "git", "hugo", "blog", "open-source"]
+++

Manchmal, aber wirklich nur manchmal, schreiben mir aufmerksame Leserinnen und Leser, dass ich mal wieder einen Text voller Rechtschreibfehler online gestellt hab. Meistens fällt mir mindestens einer direkt nach dem Veröffentlichen selbst auf ... 🙄

## Ein Online-Editor für Hugo wäre cool

Aktuell arbeite ich hier mit [Hugo](/tags/hugo). Das gefällt mir immer noch sehr gut. Ich finde es aber etwas umständlich, für jeden kleinen Schreibfehler den Computer hochzufahren, die Datei zu öffnen, einen Commit abzusetzen und die Änderungen auf den Server zu laden. Für diese Fälle wäre ein Online-Editor praktisch.

Sehr gefreut habe ich mich daher über diesen Artikel bei fryboyter.de: *<a href="https://fryboyter.de/dritte-koennen-nun-aenderungen-an-meinen-artikeln-vornehmen/" target="_blank">Dritte können nun Änderungen an meinen Artikeln vornehmen</a>*. 

Darin wird beschrieben, wie man in das Template eines [Hugo](/tags/hugo)-Blogs einen Link zum entsprechenden File in [GitHub](https://github.com/s01ren) einfügt. Gesagt. Getan. Unterhalb der Überschrift, direkt bevor der Text los geht, findet ihr ab sofort bei allen Artikeln einen Link zu GitHub. 

<div class="imagebox" style="width:75%;margin:auto">
    <img src="/img/github_link.png"/>
    <div style="float:right;">
        <b>Neuer Artikel-Heder mit GitHub Verweis</b>
    </div>
    <br>
</div>
<br>

Der Charme besteht eindeutig darin, dass GitHub einen Online-Editor für Textdateien bietet, in dem man direkt kleinere Schreibfehler korrigieren und ins Repository pushen kann. Besser geht's kaum. DANKE.

## Wie gehts?

Eingebaut ist der Link auch super-fix.

  1. Als erstes ergänzt ihr in eurer ``config.toml`` die folgenden Zeilen. Dabei müssen ``<username>`` und ``<reponame>`` natürlich angepasst werden.
  ```
  [params]
  github_link = "https://github.com/<username>/<reponame>"
  ```
  Wenn ihr bereits einen ``[params]``-Abschnitt in eurer Konfigurationsdatei habt, fügt den GitHub-Link einfach als neue Zeile in diesem Abschnitt ein.

  1. Anschließend müsst ihr nur noch eine Stelle in eurem Template finden, an die der Link soll. Dort fügt ihr den Verweis mittels ``<a href="{{.Site.Params.github_link}}edit/master/content/{{.File.Path}}"> Bei Github bearbeiten</a>`` ein. 
  [Fryboyter](ttps://fryboyter.de/) platziert es beispielsweise sowohl auf der Start- als auch auf der Artikelseite. Ich habe es lediglich [im Artikel selbst](https://github.com/s01ren/gluecko.se/blob/master/themes/base16/layouts/partials/article.html) eingefügt.


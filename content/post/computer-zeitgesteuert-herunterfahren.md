+++
title = "Computer zeitgesteuert herunterfahren"
date = "2019-10-27"
layout = ["article"]
author = "soeren"
tags = ["windows", "ubuntu"]
+++

Heute gibt es einen sehr kurzen Artikel mit einem, wie ich finde, immer recht hilfreichen Tipp. 

Ich bin häufiger in der Situation, dass ich eigentlich weg vom PC muss/will, er aber noch nicht alle Aufgaben erledigt hat. Vielleicht wird noch ein Backup erstellt, oder ein Modell rechnet noch, oder ich möchte zum Einschlafen einen Film anschauen und verhindern, dass der Computer die ganze Nacht durchläuft.

Für dieses Problem gibt es eine einfache Lösung: zeitgesteuerte Shutdown-Befehle.

## Ubuntu

[Ubuntu](/tags/ubuntu) kann man beispielsweise mit den folgenden Terminal-Befehlen entweder um 22:30 Uhr oder in 30 min herunterfahren.

```
[Batman@lokal ~]$ shutdown -h 22:30
[Batman@lokal ~]$ shutdown +30
```

Nochmal anders überlegt? Du kannst den Befehl jederzeit abbrechen.

```
[Batman@lokal ~]$ shutdown -c
```

Eine sehr ausführliche Übersicht über die Möglichkeiten gibt es [hier](https://vitux.com/how-to-shut-down-ubuntu/). 

## Windows

Auch unter [Windows](/tags/windows) gibt es ähnliche Befehle.

Dafür drückst du den *Windows*-Button auf der Tastatur, um das Startmenü zu öffnen und suchst nach `cmd`. Wenn du in die Eingabeaufforderung den folgenden Befehl eingibst, fährt der PC in einer Stunde herunter.

```
C:\Users\Batman> shutdown -s -t3600
```

Den Befehl abbrechen kannst du unter Windows mit dem Zusatz `-a`.

```
C:\Users\Batman> shutdown -a
```

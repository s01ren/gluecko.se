+++
title = "TidyTuesday Submission CW26"
date = "2019-07-01"
tags = ["R", "TidyTuesday"]
layout = ["article"]
author = "soeren"
+++

Letzte Woche habe ich zum ersten Mal an einem [TidyTuesday](https://thomasmock.netlify.com/post/tidytuesday-a-weekly-social-data-project-in-r/) teilgenommen und dabei einiges über UFO-Sichtungen gelernt.

Der TidyTuesday ist ein von der [R4DS Community](https://github.com/rfordatascience) (sprich: "R for Data Science") veranstaltes wöchentliches Projekt, das dabei helfen soll gängige Datenaufbereitungsprozesse zu üben. Grundlage sollen nach Möglichkeit die Pakete des `tidyverse` sein. 

Jeden Montag wird ein Datensatz auf GitHub veröffentlicht, gemeinsam mit einem Artikel und einer Visualisierung. Ziel ist es dann die zur Verfügung gestellten Daten aufzubereiten und anschließend selbst eine Visualisierung zu bauen. Der Phantasie sind dabei keine Grenzen gesetzt. Es kann die *Originalgrafik* reproduziert oder was völlig neues entworfen werden. 

Warum das Spaß macht? Weil die `R` Community seit einigen Jahren sehr inkludierend ist. Ziel ist nicht die Autoren des Originalartikels oder vermeintlich einfache Visualisierungen von anderen Teilnehmerinnen und Teilnehmern vorzuführen sondern sich einfach jede Woche selbst herauszufordern. 

> “I came for the software, but I stayed for the community” [@revodavid](https://twitter.com/maxheld/status/1148268443101290496)

Letzte Woche nun hatte ich ein bisschen Zeit und war motiviert. Naiverweise dachte ich, dass mir ein paarh locker reichen würden ... Pustekuchen. Aber der Ehrgeiz hat mich schnell gepackt, sodass ich zwei Abende bis spät in die Nacht saß.

In KW26 standen [UFO-Sichtungen auf dem Programm](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-06-25). Als Grundlage diente ein Datensatz mit mehr als 80.000 (vermeintlichen?) UFO-Sichtungen auf der ganzen Welt, jeweils mit Angaben zum Beobachtungsort, der UFO-Form, einer Beschreibung und wie lang die Sichtung gedauert hat. 

Um die Auswahl ein bisschen einzuschränken, habe ich mich zunächst auf die Sichtungen aus Deutschland konzentriert. Realtiv schnell kam dabei die Frage auf, wo in Deutschland eigentlich UFOs gesichtet werden. In den Städten? Auf dem Land? Im Norden? Im Süden? ... und weil sich so etwas immer schlecht aus einer Tabelle ablesen lässt, habe ich mich für die Darstellung auf einer Karte entschieden. Wird schon nicht so schwer sein...

Für mich war es das erste Projekt mit shapefiles, also Polygonen die die Grenzen eines bestimmten Gebiets (bspw. einer Stadt oder eines Bundeslandes) beinhalten. Und leider waren die Städte im Datensatz nicht immer korrekt geschrieben. Es war also ein bisschen manuelle Arbeit notwendig. Zwei Ausreißer mussten auch noch raus geschmissen werden, da sonst Bamberg die Stadt mit der längsten Beobachtung (ich meine es waren 20 Tage am Stück) gewesen wäre.

### Ergebnis hier. 

<div class="imagebox">
	<img src="https://raw.githubusercontent.com/s01ren/TidyTuesdaySubmissions/master/20190625/ufo_germany.png" />
</div>

Für mich erstaunlich war, dass die Rangliste nicht von den größten Städten (Berlin, Hamburg, München) angeführt wird, sondern auch "kleinere" Städte in den Top 10 vertreten sind. Wer es genau wissen will, kann in der Tabelle unten nachschauen. 

Insgesamt sind UFOs hierzulande aber wohl doch [nicht](https://twitter.com/LarissaKostiw/status/1145707365922156549) [so](https://twitter.com/shafayet_shafee/status/1145118906874335233) [der Renner](https://twitter.com/spren9er/status/1148282207842312192) ;-)


***

<table>
	<colgroup>
		<col width="75%" />
		<col width="25%"/>
	</colgroup>
	<thead>
		<tr class="header">
			<th style="text-align:left">Stadt/Landkreis</th>
			<th>Beobachtungsdauer</th>
		</tr>
	</thead>
	<tbody>
		<tr><td>Kaiserslautern</td><td style="text-align:right">10.922 Sek. (3,03h)</td></tr>
		<tr><td>Bamberg</td><td style="text-align:right">10.180 Sek. (2,83h)</td></tr>
		<tr><td>Bremen</td><td style="text-align:right">10.090 Sek. (2,80h)</td></tr>
		<tr><td>Darmstadt</td><td style="text-align:right">10.017 Sek. (2,78h)</td></tr>
		<tr><td>Potsdam</td><td style="text-align:right">10.003 Sek. (2,78h)</td></tr>
		<tr><td>Hamburg</td><td style="text-align:right">10.000 Sek. (2,78h)</td></tr>
		<tr><td>Nürnberg</td><td style="text-align:right">10.000 Sek. (2,78h)</td></tr>
		<tr><td>Rhein-Hunsrück-Kreis</td><td style="text-align:right">7.200 Sek. (2,00h)</td></tr>
		<tr><td>Bergstraße</td><td style="text-align:right">3.360 Sek. (0,93h)</td></tr>
		<tr><td>Berlin</td><td style="text-align:right">3.105 Sek. (0,86h)</td></tr>
	</tbody>
</table>

***


## Code auf Github

Code zum reproduzieren gibt's wie immer auf Github: [https://github.com/s01ren/TidyTuesdaySubmissions](https://github.com/s01ren/TidyTuesdaySubmissions/tree/master/20190625)



# So ist mein SDR-Training gebaut

Stand Oktober 2026. Das hier ist das System, mit dem ich bei einem Kunden drei SDRs trainiere, ohne dass ich danebensitze. Gezeigt beim MyEO Experience Share am 13.10.2026. Der Rollenspiel-Trainer in diesem Repo ist ein Teil davon, der Rest ist hier beschrieben, damit ihr es nachbauen könnt.

Keine Software zum Herunterladen. Das Scoring-Skript und das Board gehören dem Kunden. Was ihr hier bekommt, sind die Bausteine, die Felder und die Reihenfolge.

## Das Problem

Ein neuer SDR ruft an, die Zentrale fragt "Worum geht's", und das war's. Von 256 ausgewerteten Calls blieben 52 Prozent in der Zentrale hängen, 7 Prozent wurden ein echtes Gespräch. Selber coachen kostet Zeit, die keiner hat. Rollenspiele im Team macht keiner gern und keiner regelmäßig. Calls anhören: bei 150 am Tag hört sie niemand.

## Das Modell in fünf Schritten

1. **Messen.** Ein Board in Google Sheets, Ampel je Rep gegen das Tagesziel.
2. **Bewerten.** Jeder Call wird über Nacht transkribiert und auf sechs Dimensionen benotet, 1 bis 10, mit festen Ankerpunkten. Das Raster liegt in [`sdr-ki-trainer/references/bewertung.md`](sdr-ki-trainer/references/bewertung.md).
3. **Diagnose.** Kann er es nicht, oder macht er zu wenig? Das Board beantwortet die zweite Frage, die Bewertung die erste.
4. **Üben.** Drills und Rollenspiel gegen den Trainer, Persona nach Schwäche.
5. **Routine.** Fester Tagesablauf, vom Board um neun bis zum Coaching um halb sechs.

## Drei Schichten

**Schicht 1, Board.** Ein Google Sheet, das jeder im Team morgens aufmacht. Grün ab 100 Prozent vom Tagesziel, Gelb ab 80, Rot darunter. Ein Apps-Script-Knopf zieht die Zahlen frisch. Um 17:30 bekommt jeder Rep seine drei Punkte für morgen in Slack, mit einem wörtlichen Alternativsatz.

Tabs im Sheet:

| Tab | Was drinsteht | Wann frisch |
| --- | --- | --- |
| Dashboard | Calls, Connects, Entscheidergespräche, Termine je Rep, Wrong-Number-Rate, Ampel je Stufe | 09:00, 13:00, 18:00 |
| Leaderboard | Wer liegt wo, Woche und Monat | täglich |
| Call-Analyse | Jeder Call mit Transkript, Noten, Stärken, Verbesserungen, erkannten Einwänden, Key Moment, Trainingsempfehlung | 18:30 |
| Coaching je Rep | Ampel, Notizen, Trainingsvorschläge, Verlauf | 17:30 |
| Best Calls | Die besten Gespräche des Jahres zum Nachhören | täglich |
| Trainingsfokus | Schwächste Dimension je Rep, Thema der Woche | 17:30 |

Die Wrong-Number-Rate steht bewusst auf dem Dashboard. Sie zeigt Datenprobleme, bevor jemand den SDR dafür verantwortlich macht.

**Schicht 2, Automatik.** Ein Cron auf AWS (EventBridge, Montag bis Freitag) startet eine Lambda-Funktion in Python mit vier Aufgaben: Zahlen ziehen, Calls bewerten, Coaching schreiben, Meeting-Notizen einsammeln. Die Aufnahmen kommen aus dem CRM (Close.com, alles ab 30 Sekunden). Ein Sprachmodell bekommt das Audio und gibt Transkript plus sechs Noten als JSON zurück. Für Tonlage und Sprechtempo läuft zusätzlich eine Sprachanalyse. Alles schreibt ins Sheet. Kein Mensch im Lauf.

Zeitplan:

| Uhrzeit | Was |
| --- | --- |
| 09:00 | Snapshot, Dashboard frisch |
| 13:00 | Snapshot |
| 17:30 | Coaching je Rep nach Slack, Trainingsfokus |
| 18:00 | Tagesabschluss |
| 18:30 | Scoring aller Calls des Tages |

**Schicht 3, Wissen.** Eine eigene Bibliothek aus rund 200 Quellen, davon 184 Bücher, als durchsuchbarer Index (RAG). Claude greift per MCP darauf zu. Daraus entstanden das Bewertungsraster, die Einwand-Skripte, die Personas und dieser Trainer. Ohne diese Schicht benotet das Modell nach Gefühl.

## Tagesablauf des Reps

| Uhrzeit | Was |
| --- | --- |
| 09:00 | Board auf: Ampeln von gestern, Fokus für heute |
| 09:15 | Warm-up gegen den Trainer: zwei Opener, ein Tagesfokus, drei Einwand-Blitzrunden |
| 09:30 | Calling-Block, jeder Call mit Outcome getaggt, Zentrale nach zehn Sekunden raus |
| 17:30 | Drei Punkte in Slack lesen |
| 18:30 | Noten des Tages in Slack, zu Hause lesen |

Die Routine ist der Teil, den die meisten Teams nach zwei Wochen fallen lassen. Deshalb läuft sie automatisch.

## Was vor dem Kaltanruf im CRM liegt

Research macht kein SDR. Ein Clay-Workflow reichert jeden Lead an, das Ergebnis liegt am Lead im CRM:

- Trigger: offene Stelle, wie lange offen, Funding der letzten Monate
- Firma: Branche, Größe, Sitz, Spezialisierung
- Entscheider: Hiring Manager mit Durchwahl und LinkedIn, Vernetzung 24 bis 48 Stunden vor dem Anruf
- Bewerbungslage: geschätzte Zahl der Bewerbungen, welche Recruiter schon dran sind
- Hypothese: ein Satz, warum die Stelle gerade Druck macht, plus die häufigsten Einwände mit Antwort

Der SDR liest das in einer Minute und wählt.

## Diagnose und Nachschub

Schwache Zahlen haben zwei Ursachen. Entweder macht einer zu wenig, das zeigt das Board nach zwei Wochen. Oder er kann es nicht, das zeigt die Bewertung. Trainiert wird, wer die Menge bringt und das Können noch nicht hat. Wer zu wenig macht und keinen Bock hat, wird ersetzt.

Deshalb läuft die Recruiting-Pipeline immer, nicht erst, wenn einer geht: Cold Outreach über LinkedIn an Studenten und Absolventen, Chance plus schnelles Geld mit variabler Vergütung auf Termine, Sammelinterviews mit zehn Leuten und Rollenspiel live, Start sofort mit Board ab Tag eins. Churn ist eingepreist. Eine Regel für die Variable: jede Zeile im Vertrag nennt das Feld im CRM, das sie misst. Sonst streitet ihr nach drei Monaten über Geld.

## Was es kostet

| Posten | Je Monat |
| --- | --- |
| Sprachmodell fürs Scoring, rund 0,005 Euro je Call bei 3.000 Calls | 15 Euro |
| AWS, Cron und Lambda | unter 5 Euro |
| Claude-Abo für den Trainer | Abo |
| Mein Aufwand | 2 Stunden die Woche |

Zum Vergleich: dasselbe Coaching als Mandat durch mich kostet 20.000 Euro.

Was es bringt, ist mein Richtwert, nicht gemessen: rund 30 Prozent mehr Termine und mehr Abschlüsse. Messt es selbst: gleiche Kennzahlen vier Wochen vorher und nachher.

## Nachbauen in sechs Schritten

1. Bewertungsraster festlegen. Nehmt das aus diesem Repo oder schreibt euer eigenes, aber mit Ankerpunkten je Note. Ohne Anker benotet das Modell jeden Tag anders.
2. Aufnahmen aus dem CRM holen. Prüft vorher, ob euer CRM Aufnahmen per API hergibt, und klärt die Einwilligung.
3. Scoring-Skript: Audio rein, Transkript und Noten als JSON raus, ins Sheet schreiben. Ein Nachmittag Arbeit mit Claude Code.
4. Sheet mit den Tabs oben, Ampel-Schwellen je Rep.
5. Cron für 18:30 Scoring und 17:30 Coaching. Slack-Webhook für die Posts.
6. Trainer aus diesem Repo installieren, eigenes Angebot in `references/mein-angebot.md` eintragen, Warm-up in die Morgenroutine.

Fangt mit Schritt 1 an. Wer bei Schritt 3 anfängt, baut ein Skript ohne Maßstab.

## Was nicht funktioniert

- Tonfall und Pausen sieht der Trainer nicht. Die Bewertung beruht auf Text, Tonalität ist eine Schätzung.
- Das Rollenspiel ist kein Anruf. Echte Menschen reagieren anders.
- Die Wirkung auf die Quote ist nicht gemessen. Siehe oben.
- Noten schwanken. Der Trend über zehn Calls zählt, nie ein einzelner Wert.
- Aufzeichnung echter Telefonate braucht Einwilligung. Keine echten Namen in Transkripten. Das ist keine Rechtsberatung, das klärt ihr selbst.

Das Coaching ist ein Hinweis, kein Leistungsurteil. Ob einer bleibt, entscheidet ein Mensch.

## Fragen

Chris Erler, chris@erlerventures.org

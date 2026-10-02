# SDR KI-Trainer

Ein Claude-Skill, der deine SDRs (und dich selbst) im Kaltakquise-Telefonat trainiert. Du sprichst oder schreibst in der Claude-App mit einem simulierten Gesprächspartner, bekommst danach eine Bewertung mit Ankerpunkten und wörtliche Alternativen.

Gebaut von Chris Erler für das MyEO Experience Share am 13.10.2026. Frei nutzbar.

## Was drin ist

- Rollenspiel mit vier Personas: skeptischer Geschäftsführer, ausweichende Fachbereichsleitung, preissensibler Startup-CEO, Gatekeeper
- Schnellhilfe: "Was sage ich bei Einwand X?" mit je drei wörtlichen Antworten (LARC)
- Call-Analyse aus einem Transkript
- Bewertung auf zwei Ebenen: Schnellbewertung auf 4 Achsen (1 bis 5) und Tiefenbewertung auf 6 Dimensionen (1 bis 10)
- Training-Session zu einem Thema und Tages-Warm-up vor dem Calling-Block
- Eine Datei, in die du dein eigenes Angebot einträgst. Der Trainer arbeitet danach mit deinen Zahlen, deinem Kunden und deinen Einwänden. Als Beispiel liegt eine fiktive Firma drin (Nordlicht Software GmbH).

## Installation

**a) claude.ai oder Claude Desktop**

1. Den Ordner `sdr-ki-trainer` (der innere, mit der `SKILL.md`) als ZIP packen. Die `SKILL.md` muss direkt im ZIP-Ordner liegen.
2. In Claude: Einstellungen, Capabilities bzw. Skills, ZIP hochladen, Skill aktivieren.
3. Neuen Chat öffnen und loslegen.

**b) Claude Code**

Ordner `sdr-ki-trainer` nach `~/.claude/skills/sdr-ki-trainer` kopieren. Fertig, beim nächsten Start ist der Skill da.

Hinweis: Menüpunkte heißen je nach App-Version leicht anders. Im Zweifel in der Claude-Hilfe nach "Skills hochladen" suchen.

## Erste Schritte

Drei Prompts zum Ausprobieren:

1. `Trainier mich: Rollenspiel mit skeptischem Geschäftsführer`
2. `Was sage ich bei "Wir haben schon einen Anbieter"?`
3. `Bewerte dieses Transkript: ...` (Text einfügen, zum Testen geht `examples/beispiel-transkript.md`)

Für ein Sprach-Rollenspiel: Sprachmodus der Claude-App einschalten und den ersten Prompt sprechen. Die Bewertung danach beruht auf dem Text des Gesprächs.

## Dein Angebot eintragen

Öffne `sdr-ki-trainer/references/mein-angebot.md` und ersetze die Platzhalter in den eckigen Klammern: Firma, Produkt, Ticketgröße, ICP, Personas, Nutzen, Beweise, typische Einwände, Terminziel. Je konkreter, desto besser das Rollenspiel. Ohne Änderung läuft der Trainer mit der fiktiven Nordlicht Software GmbH.

Tipp: Echte Einwände aus deinem Alltag zuerst eintragen. Daran merkt der Trainer am meisten.

## Grenzen

- Es ist kein echter Anruf. Echte Menschen reagieren anders als jede Simulation.
- Die Bewertung beruht auf Transkript-Text. Tonfall, Tempo und Pausen sieht der Trainer nur, wenn sie im Text stehen. Sie ist ein Coaching-Hinweis, kein Leistungsurteil.
- Ob der Trainer die Abschlussquote deiner SDRs hebt, ist nicht gemessen. Miss es selbst: gleiche Kennzahlen vor und nach vier Wochen.
- Die Modelle bewerten nicht jedes Mal identisch. Für Trends mehrere Calls vergleichen, nicht einen.

## Datenschutz

Lade keine echten Kundendaten und keine Transkripte mit Namen von Gesprächspartnern hoch. Namen, Firmen und Zahlen vorher ersetzen. Aufzeichnung echter Telefonate braucht Einwilligung. Das klärst du selbst, der Trainer macht dazu keine Rechtsberatung.

## Lizenz

MIT, siehe `LICENSE`.

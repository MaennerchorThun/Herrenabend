# Herrenabend Männerchor Thun – App (Prototyp)

Klickbarer Prototyp mit **fiktiven Demo-Daten**. Eine einzige Datei (`index.html`), keine Abhängigkeiten.
Oben rechts lässt sich die Rolle wechseln: Teilnehmer, Gruppenchef, Organisation.

## Funktionen

| Bereich | Teilnehmer | Gruppenchef | OK |
|---|---|---|---|
| Angemeldete, Suche, Filter | ✓ | ✓ | ✓ + «ohne Platz» |
| Saalplan / Tischplan (Layout wie Tischplan 2025) | ansehen | Plätze der eigenen Gruppe bearbeiten | alle bearbeiten |
| Tischeinteilung / Platzzuteilung | – | eigene Gruppe platzieren, tauschen | Gruppen nach Tischplan 2025 oder automatisch verteilen |
| Apéro (16:30 Biertheke) + Gruppen-Apéro | Zusage | Gruppen-Apéro festlegen | Übersicht |
| Geladene Gäste | ✓ | ✓ | ✓ |
| Menu + Essenswunsch (Normal/Vegi) | ✓ | ✓ | + Catering-Übersicht pro Tisch |
| Programm | öffentlich | öffentlich | + Drehbuch & Verantwortliche |
| Gesangsrepertoire | ✓ | ✓ | ✓ |
| Weinkarte | Wein vorschlagen | bestellen für Tisch der Gruppe | Service-Board, Abrechnung pro Gruppe |
| Feedback (auch anonym) | ✓ | ✓ | Eingang |
| Umfrage nach dem Anlass | nach Freischaltung | nach Freischaltung | freischalten, Auswertung |
| CSV-Import Webling, Link-Export | – | – | ✓ |

Gruppen: Teilnehmer sind Gruppen zugeteilt (Webling-Feld «Herrenabend-Tisch»), meist benannt nach dem Gruppenchef. An einem Tisch sitzen oft 2–3 Gruppen, grosse Gruppen (z.B. Bautisch) belegen mehrere Tische. Gruppenchefs verwalten nur ihre eigene Gruppe (Plätze, Weinbestellung, optionaler Gruppen-Apéro).

Import: Webling-Export direkt in der Verwaltung hochladen. Übernommen werden nur Vorname, Name, Gruppe und Typ (Sänger, Passivmitglied, Sponsorgast, Ehrengast) – keine E-Mail, Telefonnummern oder Bemerkungen. Gruppenchefs werden aus dem Gruppennamen bzw. der Webling-Gruppe «Gruppenchef» erkannt. Danach werden die Gruppen nach Tischplan 2025 verteilt.

Apéro: für alle um 16:30 in der Empfangshalle an der Biertheke; Gruppen können zusätzlich einen eigenen Apéro eintragen.

Saal: 21 Tische à 16 Plätze, Anordnung wie Tischplan 2025 (Bühne links, 3 Spalten à 7 Tische) = 336 Plätze für ca. 330 Gäste. Platznummern fortlaufend wie im Sitzplan 2025: Tisch n = Plätze 16·(n−1)+1 bis 16·n, Tisch 4 (49–64) = Gästetisch.

Logo: im `index.html` eingebettet; `mct-logo.png` liegt separat bei (transparent).

Zugang: persönlicher Link `?t=<token>`. Ein Gruppenchef-Link schaltet automatisch die eigene Gruppe frei.

## Auf GitHub Pages veröffentlichen

1. Neues Repository anlegen, z.B. `herrenabend`.
2. `index.html` hochladen.
3. Settings → Pages → Branch `main`, Ordner `/ (root)` → Save.
4. App läuft unter `https://<benutzer>.github.io/herrenabend/`.

**Datenschutz:** Keine echten Teilnehmerdaten ins Repository legen. Der Prototyp hält importierte Daten nur im offenen Browserfenster (Reload = Demo-Zustand). Echte Daten kommen erst in Phase 2 in Supabase, geschützt durch Row-Level-Security.

## Phase 2 – Supabase

- Tabellen: `events`, `tables`, `groups`, `people` (inkl. `token`, `group_id`, `table_id`, `seat`, `menu`), `apero`, `wines`, `orders`, `order_items`, `proposals`, `feedback`, `survey_responses`, `program_items`, `songs`
- Zugriff: Token → Supabase Edge Function stellt ein kurzlebiges JWT mit `role` und `group_id` aus; RLS beschränkt Gruppenchefs auf ihre Gruppe.
- Realtime auf `orders` für das Service-Board.
- Import: Webling-CSV über die Verwaltung → `people` (upsert).
- QR-Codes für Tischkarten aus dem Link-Export.

## Offene Inputs

- Programm 2026, Gruppen-Zuteilung 2026, Menu und Weinkarte vom Caterer, Liedauswahl

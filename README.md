# TrainFlex PWA

TrainFlex ist eine vollständig lokale, offlinefähige Progressive Web App für Trainingsplanung, Durchführung, Geräteverwaltung, Fortschritt und Kalenderexport. Es gibt kein Benutzerkonto und keinen Server für Trainingsdaten. Die Daten liegen in IndexedDB des verwendeten Browsers.

## Projektstruktur

- `index.html` – Einstieg und mobile App-Struktur
- `styles.css` – responsive, iPhone-optimierte Oberfläche
- `db.js` – IndexedDB-Speicherung
- `app.js` – Navigation, Planung, Live-Training, Anpassungslogik, Auswertung, Import/Export und ICS
- `manifest.webmanifest` – PWA-Metadaten
- `service-worker.js` – Offline-Cache
- `icons/` – App-Icons in 180, 192 und 512 Pixel

## Lokal starten

Service Worker benötigen HTTP oder HTTPS und funktionieren nicht zuverlässig über einen direkten `file://`-Aufruf.

1. ZIP entpacken.
2. Im Projektordner einen lokalen Webserver starten, zum Beispiel mit Python 3: `python3 -m http.server 8080`
3. Im Browser `http://localhost:8080` öffnen.

Für das iPhone muss die Seite über HTTPS erreichbar sein oder über einen Rechner im gleichen Netzwerk bereitgestellt werden. Für eine dauerhafte Installation ist die kostenlose HTTPS-Veröffentlichung die einfachste Variante.

## Kostenlos veröffentlichen mit GitHub Pages

1. Kostenloses GitHub-Konto verwenden und ein neues öffentliches Repository anlegen.
2. Alle Dateien aus diesem Ordner in die oberste Ebene des Repositorys hochladen.
3. Unter `Settings` > `Pages` als Quelle `Deploy from a branch` wählen.
4. Branch `main`, Ordner `/ (root)` auswählen und speichern.
5. Die angezeigte HTTPS-Adresse in Safari öffnen.

Alternativ funktionieren statische Hostingdienste wie Cloudflare Pages oder Netlify. Es ist kein Build-Schritt erforderlich.

## Auf dem iPhone installieren

1. Die veröffentlichte HTTPS-Adresse in Safari öffnen.
2. Auf das Teilen-Symbol tippen.
3. Nach unten scrollen und `Zum Home-Bildschirm` wählen.
4. Namen prüfen und `Hinzufügen` tippen.
5. TrainFlex anschließend über das neue Home-Bildschirm-Symbol starten.

Safari zeigt keinen identischen Installationsdialog wie viele Desktop-Browser. Die Installation über `Zum Home-Bildschirm` ist deshalb die vorgesehene iPhone-Methode.

## Erster Einsatz

1. Unter `Mehr` > `Einstellungen & Daten` Athletenprofil, Dauer und Ziele eintragen.
2. Unter `Geräte & Verfügbarkeit` Geräte und Orte erfassen.
3. In der Übungsbibliothek Übungen deaktivieren oder Alternativen festlegen.
4. Unter `Training` eine Vorlage mit Übungen, Sätzen und Zielwerten anlegen.
5. Einen Termin planen. Zukünftige Termine reagieren auf Geräteverfügbarkeit, deaktivierte Übungen und kurze Zeitfenster.
6. Im Training tatsächliche Werte eingeben. Änderungen werden nach jeder Eingabe automatisch in IndexedDB gespeichert.

## Version 3

- Geräte lassen sich bearbeiten und löschen.
- Weitere Kraft-, Core-, Functional- und Ausdauerübungen wurden ergänzt.
- Tatsächliche Gewichte und Wiederholungen werden je Satz gespeichert und ausgewertet.

## Leistungsprofil

Unter `Mehr` > `Leistungsprofil` können Kraftwerte, Laufzeiten und Conditioning-/HYROX-Referenzwerte gepflegt werden. Unbekannte Werte bleiben leer; die App erzeugt keine fiktiven Leistungen.

## Datenexport und Datensicherung

- `Mehr` > `Einstellungen & Daten` > `JSON-Sicherung exportieren` erstellt eine vollständige Sicherung.
- Beim Import wird die Versionsnummer und grundlegende Struktur validiert.
- Vor dem Überschreiben vorhandener Daten erscheint eine Warnung.
- Backups regelmäßig außerhalb des Browsers sichern, etwa in iCloud Drive.
- Das Löschen von Safari-Websitedaten kann lokale App-Daten entfernen.

## Kalenderexport

- Ein einzelnes Training kann in seiner Detailansicht als ICS exportiert werden.
- Im Kalender können alle zukünftigen Einheiten gemeinsam exportiert werden.
- Die ICS-Datei in iOS öffnen und dem Apple Kalender hinzufügen.
- Erinnerungen werden als VALARM-Einträge mit den in den Einstellungen festgelegten Vorlaufzeiten ausgegeben.
- Dies ist ein Export, keine automatische oder bidirektionale Synchronisierung.

## Anpassungsregeln

Nur zukünftige, nicht abgeschlossene Einheiten werden angepasst:

1. Deaktivierte Übungen werden bevorzugt durch die hinterlegte Alternative ersetzt.
2. Fehlt das benötigte Gerät, wird eine verfügbare Übung derselben Kategorie gewählt.
3. Ist weniger Zeit als die Vorlagendauer verfügbar, bleiben die vorderen Prioritätsübungen anteilig erhalten.
4. Manuelle Ersetzungen werden begründet protokolliert.
5. Abgeschlossene Einheiten sind unveränderliche Verlaufseinträge und werden nicht rückwirkend angepasst.

Die Regeln treffen keine medizinischen Entscheidungen.

## Testcheckliste

- [x] App startet ohne Build-Schritt über einen statischen Webserver.
- [x] Navigation und mobile Bottom-Navigation sind implementiert.
- [x] Responsive Layouts für iPhone und Desktop sind vorhanden.
- [x] Vorlagen und Termine lassen sich erstellen, bearbeiten und verschieben.
- [x] Übungen lassen sich hinzufügen, deaktivieren und austauschen.
- [x] Eingaben im Live-Training werden automatisch gespeichert.
- [x] Daten bleiben durch IndexedDB nach Neustart erhalten.
- [x] JSON-Export, validierter Import und Überschreibwarnung sind implementiert.
- [x] Service Worker cached alle Kernressourcen für Offline-Nutzung.
- [x] Manifest und Icons sind eingebunden.
- [x] Einzel- und Mehrfach-ICS-Export mit Erinnerungen sind implementiert.
- [x] Volumen, Abschlussquote, Monatswerte und Bestleistungen werden nur aus Nutzerdaten berechnet.
- [x] Nicht verfügbare oder deaktivierte Übungen werden regelbasiert ersetzt.
- [x] Es werden keine Trainingsleistungen vorgegeben oder erfunden.
- [x] Alle sichtbaren Schaltflächen haben eine Funktion.

## Bekannte technische Einschränkungen

- iOS-Web-Apps können den Apple Kalender nicht zuverlässig direkt und dauerhaft synchronisieren. Die App nutzt deshalb ICS-Dateien.
- Browser dürfen Webspeicher bei manuellem Löschen von Websitedaten entfernen. JSON-Backups schützen davor.
- Der Service Worker wird erst nach dem ersten erfolgreichen Online-Aufruf installiert. Danach sind die Kernfunktionen offline verfügbar.
- iOS kann Hintergrundprozesse, dauerhafte Timer und Vibrationen einschränken. Der Trainingstimer läuft nur zuverlässig, solange die App aktiv bleibt.
- Ohne Benutzerkonto gibt es bewusst keine automatische Synchronisierung zwischen iPhone und Desktop. Daten können per JSON übertragen werden.

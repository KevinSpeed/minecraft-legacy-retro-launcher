# Prüfbericht – aktualisiert am 20. September 2026

## Aktualisierung: Historisches Grasblock-Symbol

- Nachbesserung des Explorer-Symbols: sieben klassische Windows-Bitmap-Icon-Größen von 16 bis 256 Pixeln statt einer einzelnen PNG-Icon-Größe eingebettet. Die neue Download-Datei heißt `RetroCraft-Launcher.exe`, damit der bisherige Dateiname nicht denselben alten Symbolcache-Eintrag verwendet.
- Die fertige neue EXE mit Windows `SHGetFileInfo` abgefragt: sowohl das kleine 16-Pixel- als auch das große 32-Pixel-Dateisymbol zeigen den Grasblock. Alle sieben eingebetteten Größen erfolgreich dekodiert und den tatsächlichen Windows-Symbolabruf visuell geprüft.

- Das ursprüngliche 32-Pixel-Grasblock-Symbol als unveränderte PNG-Ressource übernommen und für Fenster, Dialoge und Windows-EXE eingebunden. Die Windows-App-Kennung gruppiert den Launcher separat in der Taskleiste.
- Fenster-Symbol und tatsächliche Icon-Ressource der gebauten EXE pixelgenau gegen die Originalgrafik geprüft. Die einzelne EXE aus einem Ordner ohne Begleitdateien im Offline-Vorschaumodus gestartet (Exitcode 0); temporäre Dateien anschließend entfernt. Keine erneute Prüfung der unveränderten Netzwerk- und Spielfunktionen für diese reine Symboländerung.

## Aktualisierung: Kompakte einzelne EXE

- Qt WebEngine einschließlich eigener Chromium-Kopie durch die vorhandene Microsoft-WebView2-Runtime ersetzt. Der kleine eigene C#-Host und die unveränderten SDK-Bibliotheken sind eingebettet. Kein zusätzliches Konsolenfenster und keine automatische Runtime-Installation.
- PyInstaller-Einzeldatei mit rund 38 MB statt des bisherigen großen Programmordners erstellt. Nicht verwendete Qt-WebEngine-, QML-, PDF- und Software-OpenGL-Komponenten sowie unbenötigte Übersetzungen sind nicht enthalten. Logo, Oberfläche, feste Microsoft-App-ID und bestehendes Benutzerverzeichnis bleiben erhalten.
- Die tatsächlich gebaute EXE aus einem Verzeichnis gestartet, das ausschließlich diese Datei enthielt. Öffentliche Tumblr-Seite erfolgreich mit WebView2 153.0.4234.32 geladen; echte DOM-Metriken und Browser-Bild aufgenommen. Prozessende mit Exitcode 0, anschließend keine temporären `_MEI`-Dateien mehr vorhanden. Temporär entpackte Laufzeit im Test rund 89 MB; separat installierte WebView2-Runtime, Cache und Minecraft-Dateien sind darin nicht enthalten.
- 20 Navigation-, Browserzustands- und Oberflächentests erfolgreich. Darunter echte Navigation-Regeln des kompilierten C#-Hosts, blockierte lokale/Anwendungs-URLs, Behandlung fehlender WebView2-Runtime und unveränderte Verwendung der festen Microsoft-App-ID mit simulierten Kontodaten.
- Native Browser-Fläche bei 960 × 600 und 1440 × 900 Pixeln geprüft: Webseitenbereich korrekt auf 960 × 498 beziehungsweise 1440 × 798 angepasst. Optionen geöffnet und Browser-Hilfsprozess beim Schließen mit Exitcode 0 beendet.
- Die Angaben zur fehlenden Prüfung eines echten Microsoft-Logins und vollständigen Spielstarts gelten weiterhin. Die neue Änderung betrifft Browser-Anbindung und Auslieferung. Die folgenden Abschnitte dokumentieren frühere Versionen.

## Aktualisierung: Originale Tumblr-Webansicht

- Die eigene RSS-/Rich-Text-Darstellung durch Qt WebEngine ersetzt. Der Launcher lädt die tatsächliche Website einschließlich des veröffentlichten Themes, Bildern und JavaScript.
- Die Webansicht im Browser und im Launcher verglichen: „Minecraft Update Hub“, 16-Pixel-Grundschrift, 32-Pixel-Überschrift, 189-Pixel-Seitenleiste und dieselbe Andesit-Hintergrundgrafik. Der echte Abruf sowie CSS, JavaScript und die Hintergrundressource wurden im Launcher geprüft.
- 18 Tests für Oberfläche, Navigation und Link-Behandlung erfolgreich. Bloginterne Links bleiben eingebettet; nur bewusst angeklickte externe Weblinks öffnen den Standardbrowser. Lokale Dateipfade, Datei-Uploads und automatische externe Pop-ups sind nicht freigeschaltet.
- Die neu gebaute Windows-Version mit Live-Website in einem separaten Prüfverzeichnis gestartet. Die Browser-Komponente hat die echte Seite erfolgreich geladen; Prozessende mit Exitcode 0. Den Screenshot der fertigen Anwendung einschließlich deutschem Tumblr-Cookie-Hinweis visuell geprüft.
- Der frühere News-Cache wird nicht mehr gelesen. Die Website nutzt einen separaten Browser-Speicher; lokal gespeicherte Website-Beiträge aus anderen Browsern werden nicht synchronisiert. Tumblr-Hinweise und Fehler des veröffentlichten Website-Themes bleiben auch im Launcher sichtbar.

## Aktualisierung: Eigener Tumblr-Blog

- Nachrichtenquelle auf `https://mcretroupdates.tumblr.com/` umgestellt. Der Launcher liest den öffentlichen RSS-Feed, damit veröffentlichte Beiträge unabhängig von der Umsetzung des Website-Themes erscheinen.
- Blog und Feed erfolgreich mit HTTP 200 abgerufen. Der Feed enthielt beim Abruf noch keine Beiträge. Das Website-Formular speichert Beiträge nur im Browser; diese Einträge sind nicht Bestandteil des öffentlichen Feeds.
- Leere Feeds werden korrekt dargestellt. Fremde/alte Blog-Caches werden ignoriert; bei Fehlern bleiben nur bereits geladene Meldungen der aktuellen Quelle erhalten.
- 17 News- und Oberflächentests erfolgreich, darunter HTML-/Linkfilter, leere Feeds, unerwartete und zu große Antworten, Quellenbindung des Caches sowie der bestehende feste Microsoft-App-Bezug. Beispielbeiträge wurden nur lokal zum Testen verwendet und nicht veröffentlicht.
- Neu gebaute Windows-EXE mit Live-News in einem separaten Prüfverzeichnis gestartet. Der echte Abruf und das Speichern des neuen Feeds waren erfolgreich; Prozessende mit Exitcode 0. Screenshot der gebauten Anwendung visuell geprüft.

## Aktualisierung: Fest eingebundene Microsoft-App

- Die vom Nutzer bereitgestellte Microsoft-App-ID ist als Konstante in `source/core.py` hinterlegt. Login, Sitzungswiederherstellung und Erneuerung vor dem Spielstart verwenden diese ID.
- App-ID-Eingabe, Validierung und technische Registrierungshinweise aus den Optionen entfernt. Alte App-ID-Einträge werden beim Laden der Einstellungen verworfen und beim nächsten Speichern nicht mehr mitgeschrieben; übrige Einstellungen bleiben erhalten.
- Vorhandene Anmeldungen derselben App bleiben verwendbar. Tokens anderer Apps werden nicht für die feste App wiederverwendet.
- 17 relevante Tests erfolgreich: Oberfläche, Auswahl/Speichern von Optionen, alte Einstellungsdateien, fester App-Bezug bei Anmeldung und Spielstart sowie lokaler Browser-Callback mit PKCE. Externe Anmelde- und Token-Antworten wurden simuliert; die API-Freigabe und ein echter Kontologin wurden nicht geprüft. Der frühere DPAPI-Test wurde für diese Änderung nicht erneut ausgeführt.
- Windows-Anwendung neu gebaut und im isolierten Vorschaumodus gestartet (Exitcode 0). Die feste ID im gebündelten Programmcode nachgewiesen und die aktualisierte Optionen-Ansicht visuell geprüft.

## Aktualisierung: RetroCraft-Logo

- Das vom Nutzer gewählte RetroCraft-Logo ersetzt den Minecraft-Schriftzug in der unteren Leiste.
- Die transparente Originaldatei wird mitgeliefert. Bei der Anzeige werden transparente Außenränder ausgeblendet und das Seitenverhältnis beim Einpassen in 256×49 Pixel beibehalten.
- Sieben vorhandene Oberflächenprüfungen erfolgreich, einschließlich Logo-Ressource, Optionen und Old-Version-Auswahl.
- Neu gebaute Windows-EXE im isolierten Vorschaumodus gestartet (Exitcode 0); den Screenshot der gebauten Anwendung visuell geprüft. Gebündelte und ursprüngliche RetroCraft-Grafik haben denselben SHA-256-Hash.
- Die untenstehenden früheren Prüfungen und Einschränkungen betreffen jeweils die damaligen Änderungen.

## Aktualisierung: Englischer Unabhängigkeits-Hinweis

- Sichtbaren englischen Hinweis in die untere Launcher-Leiste aufgenommen: `NOT AN OFFICIAL MINECRAFT PRODUCT. Not approved by or affiliated with Mojang or Microsoft.`
- Der Hinweis wird zusammen mit dem Logo angezeigt und bleibt auch bei Offline- oder News-Fehlern sichtbar.

## Aktualisierung: Optionen, Old Versions und Originalhintergrund

- Version, Snapshot-/Old-Version-Filter und „Angemeldet bleiben“ in die Optionen verschoben. Installation ohne Login ebenfalls von dort erreichbar. Änderungen werden erst beim Speichern übernommen.
- Die geprüfte offizielle Versionsliste enthält 26 `old_beta`- und 35 `old_alpha`-Einträge; diese erscheinen bei eingeschaltetem Old-Versions-Filter. Ältere Releases bleiben auch ohne diesen Filter auswählbar.
- Originale Erdtextur eingebunden; Kachelgröße und dunkler Verlauf am historischen Launcher orientiert.
- Tumblr-Schaltflächen und dauerhafte News-Werkzeugleiste entfernt. Automatischer Abruf und Fehlerhinweis bleiben erhalten.
- Sechs Oberflächentests erfolgreich, einschließlich Filterkombinationen, Auswahl und Speichern einer Beta-Version sowie Abbrechen ohne Übernahme. Hauptfenster und Optionen visuell geprüft.
- Keine erneute Abnahme von Login oder historischem Spielstart; die unten beschriebenen Einschränkungen gelten weiterhin.

## Aktualisierung: Original-News und Logo

- Nachrichtenquelle auf https://mcupdate.tumblr.com/ umgestellt; echter Abruf erfolgreich. Die Meldungen und Original-Links werden als inaktiver HTML-Inhalt im Launcher dargestellt. Die Seite wird beim Start sowie manuell aktualisiert. Bereits geladene News bleiben bei Verbindungsfehlern im Cache.
- Historisches Minecraft-Logo als unveränderte PNG-Grafik eingebunden, auch in die Windows-EXE-Paketierung.
- 7 für die Änderung relevante Tests erfolgreich: Oberfläche, Hintergrundaufgaben, Versionsfilter, Sitzungsspeicher-Ersatzweg, Logo-Ressource, News-Fehlerfall und HTML-/Link-Filter inklusive unerwarteter Serverantworten.
- Aktualisierte EXE gestartet, Live-News und Logo im Screenshot der gebauten Anwendung überprüft. Exitcode 0.
- Die nachfolgend dokumentierten Grenzen des echten Logins und der Windows-Sitzungsspeicherung bleiben bestehen.

## Aktualisierung: Minecraft-Logo im Stil von 2013

- Den Schriftzug in der unteren Leiste durch die vom Nutzer bestätigte Screenshot-Variante ersetzt.
- Das Logo pixelgenau aus dem Referenzbild freigestellt und transparent auf 256×49 Pixel gesetzt, damit es auf dem Dirt-Hintergrund sauber sitzt.
- Die Logo-Ressource und die tatsächlich gebaute EXE erneut geprüft; die UI-Tests bleiben erfolgreich.

## Erfolgreich

- Eigenständige Windows-x64-Anwendung mit PyInstaller erstellt und gestartet; Prozess beendet sich im Screenshot-Prüfmodus mit Exitcode 0.
- Screenshot der tatsächlich gebauten EXE erzeugt und visuell kontrolliert.
- Offizielle Mojang-Versionsliste abgerufen: 915 Einträge; zu diesem Prüfzeitpunkt meldete sie `26.3` als neuestes Release.
- Diese Version vollständig in ein isoliertes Testverzeichnis installiert: Client, Bibliotheken, Assets und Java-Laufzeit.
- SHA-1 des heruntergeladenen Clients gegen die offizielle Versionsmetadatei geprüft: Übereinstimmung.
- Heruntergeladene Microsoft-OpenJDK-Laufzeit gestartet: Java 25.0.1, Exitcode 0.
- Startkommando für diese Installation erzeugt und Vorhandensein der vorgesehenen Hauptklasse geprüft. Es wurde dabei kein Spiel mit künstlichen Zugangsdaten gestartet.
- 12 automatisierte Tests bestanden: Zustandsprüfung und Rückleitungsadresse beim Login, Ablehnung unvollständiger Antworten, lokaler Browser-Callback mit simuliertem Token-Austausch und PKCE, Login-Abbruch, Schutz vor Token-Ausgabe in Fehlermeldungen, Installations-Callbacks, Startargumente ohne Shell, Fehler bei fehlendem Java, Einstellungsdateien, Versions-/Snapshotfilter, Hintergrundaufgaben und sicherer Ersatzweg bei fehlgeschlagener Sitzungsspeicherung.

## Nicht erfolgreich bzw. nicht nachgewiesen

- Ein zusätzlicher Test zum echten Windows-DPAPI-Speichern/Entschlüsseln schlägt in der abgeschotteten Ausführungsumgebung mit WinError 2 fehl. Ein unabhängiger Gegenversuch über .NET ProtectedData scheitert ebenfalls mit einem Hinweis auf ein nicht geladenes Benutzerprofil. Die vollständige Testsuite meldet deshalb **12 bestanden, 1 Fehler**. Der erfolgreiche Speicherrundlauf muss in einer normalen Windows-Benutzersitzung überprüft werden. Bei Speicherfehlern deaktiviert die Oberfläche das Merken der Sitzung; dieser Ersatzweg ist getestet. Es gibt keine Klartextspeicherung als Ausweichlösung.
- Echter Microsoft-/Xbox-/Minecraft-Login: nicht durchgeführt, da keine eigene freigegebene App-ID und kein Nutzerkonto bereitgestellt wurden. Der lokale Rückleitungsweg wurde getestet, der externe Token-Austausch dabei simuliert.
- Vollständiger Minecraft-Spielstart und Multiplayer: nicht durchgeführt; dafür sind der echte Login und ein berechtigtes Konto erforderlich.
- Ältere Releases, sämtliche Snapshots, Hardware-/Treiberkombinationen und Windows 10 wurden nicht systematisch getestet.

Die Anwendung ist eine erste benutzbare Launcher-Implementierung mit erfolgreichem realem Installationstest, keine vollständig abgenommene Veröffentlichung. Die Minecraft-Testinstallation befindet sich außerhalb des gelieferten Pakets; Minecraft-Spieldateien werden nicht mitverteilt.

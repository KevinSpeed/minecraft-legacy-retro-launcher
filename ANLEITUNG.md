# Minecraft Retro Launcher

Ein eigenständiger Windows-Launcher für **Minecraft Java Edition**, optisch an den alten Launcher angelehnt. Im Launcher steht sichtbar: **“NOT AN OFFICIAL MINECRAFT PRODUCT. Not approved by or affiliated with Mojang or Microsoft.”** Die untere Leiste verwendet das RetroCraft-Logo; der Nachrichtenbereich und die Erdtextur bleiben im Retro-Stil.

Die Nachrichtenfläche lädt https://mcretroupdates.tumblr.com/ automatisch beim Start als echte Webseite mit Microsoft WebView2. Das Tumblr-Theme bestimmt Schriftarten, Abstände, Seitenleiste, Bilder und Inhalte; CSS und JavaScript werden wie im Browser ausgeführt. Änderungen an der veröffentlichten Website erscheinen beim nächsten Launcher-Start. Blog-interne Links bleiben im Launcher, externe Links öffnen sich nach einem Klick im Standardbrowser. Bei Verbindungsfehlern erscheint ein Hinweis; eine vollständige Offline-Ansicht wird nicht zugesichert.

Die Ansicht entspricht der öffentlichen Website bei derselben Fensterbreite. Auch Hinweise von Tumblr, etwa zu Cookies, können erscheinen. Die Website verwendet einen eigenen Browser-Speicher im Launcher. Inhalte, die das Website-Formular ausschließlich im lokalen Speicher deines normalen Browsers ablegt, werden dadurch nicht zwischen Geräten oder Browsern synchronisiert. Die eingebundene Seite erhält keinen Zugriff auf die Microsoft-Anmeldedaten oder den Python-Code des Launchers.

## Starten

Nur `RetroCraft-Launcher.exe` herunterladen und öffnen. Es wird kein `_internal`-Ordner neben der EXE benötigt. Python muss nicht installiert sein. Unterstützt: Windows 10/11, 64 Bit, mit .NET Framework ab 4.6.2 und Microsoft WebView2 Runtime. Diese Windows-Komponenten sind auf dem geprüften PC vorhanden. Fehlt WebView2 auf einem anderen PC, zeigt der Nachrichtenbereich einen Link zur offiziellen Installation an. Der Launcher installiert die Runtime nicht ungefragt.

Die einzelne EXE ist rund 38 MB groß. Sie verwendet die gemeinsam genutzte WebView2-Installation statt einer eigenen vollständigen Browser-Kopie. Ihre übrigen Bibliotheken werden während der Laufzeit automatisch in einen temporären Ordner entpackt (im Test rund 89 MB) und beim normalen Beenden entfernt. Einstellungen, Sitzung, Browser-Cache und Minecraft-Spieldateien bleiben wie bisher separat gespeichert. Ein Browser-Cache kann mit der Nutzung wachsen; die EXE-Größe ist nicht der gesamte Speicherbedarf einschließlich Minecraft und der gemeinsam genutzten Windows-Komponenten.

Die bisherige Programmmappe wird zum Starten der neuen EXE nicht benötigt. Bestehende Einstellungen und Kontodaten im unten beschriebenen Benutzerverzeichnis werden weiterverwendet. Das optionale Quellcode-Archiv dient nur der Weiterentwicklung. Lizenztexte sind auch unter **Optionen → Lizenzen** verfügbar.

1. Unter **Optionen** eine Version auswählen. Die Versionsliste kommt direkt von Mojang. Dort lassen sich **Snapshots** sowie **Old Versions (Alpha / Beta / Classic)** zuschalten. Ältere reguläre Releases sind immer enthalten.
2. **Microsoft-Login** öffnen und im Browser anmelden. Die Microsoft-App-ID ist bereits fest eingebunden.
3. **Spielen** drücken. Fehlende Spieldateien und die von Mojang angegebene Java-Laufzeit werden installiert.

**Ausgewählte Version installieren** in den Optionen speichert die Einstellungen und lädt die Version auch ohne Anmeldung herunter. Alle Einstellungen einschließlich „Angemeldet bleiben“ befinden sich in den Optionen. **Speichern** übernimmt Änderungen; **Abbrechen** verwirft sie. Zum Spielen ist ein berechtigtes Microsoft-Konto mit Minecraft Java Edition erforderlich. Es gibt keinen Login- oder Lizenz-Bypass.

„Old Versions“ zeigt die von Mojang bereitgestellten alten Einträge an, nicht jede jemals veröffentlichte Vorabversion. Historische Versionen können eine manuell gewählte Java-8-Laufzeit benötigen. Verwende für sie ein separates Spielverzeichnis. Ein vollständiger Spielstart dieser historischen Versionen wurde nicht überprüft.

## Voraussetzung für die Microsoft-Anmeldung

Die vom Projektbetreiber bereitgestellte Microsoft-App-ID ist fest im Launcher hinterlegt und wird nicht in den Optionen angezeigt oder bearbeitet. Nutzer müssen keine eigene App registrieren oder ID eintragen.

Die Freigabe dieser App für die Minecraft-Dienste bleibt Voraussetzung für den echten Login. Das Einbinden der ID bestätigt keine API-Freigabe; diese muss der Projektbetreiber separat erhalten. Meldet der Launcher eine fehlende Freigabe, kann das nicht in den Optionen behoben werden.

Für den Projektbetreiber: Die App muss persönliche Microsoft-Konten unterstützen und als öffentliche Desktop-Anwendung mit `http://localhost:28563/callback` als Rückleitungsadresse eingerichtet sein. Der Launcher verwendet kein Client-Secret. Die feste ID wird in `source/core.py` definiert.

Anleitungen und Quellen:

- [Microsoft-App und Minecraft-API-Freigabe](https://minecraft-launcher-lib.readthedocs.io/en/stable/tutorial/microsoft_login.html)
- [Microsoft: Rückleitungsadressen](https://learn.microsoft.com/en-us/entra/identity-platform/reply-url)
- [Minecraft-Installationsbibliothek](https://github.com/JakobDev/minecraft-launcher-lib)

## Dateien und Einstellungen

Standardverzeichnis: `%APPDATA%\RetroMinecraftLauncher`.

- `settings.json`: RAM, Spielverzeichnis, Java-Pfad, Versionsauswahl, Versionsfilter und „Angemeldet bleiben“. Alte App-ID-Einträge werden beim Laden ignoriert und beim nächsten Speichern entfernt.
- `account.bin`: ausschließlich der Refresh-Token und die zugehörige App-ID, verschlüsselt über Windows DPAPI für deinen Windows-Benutzer. Kein Passwort wird abgefragt oder gespeichert.
- `versions.json`: lokal zwischengespeicherte offizielle Versionsliste.
- `web/webview2`: eigener Browser-Speicher und Cache für die Tumblr-Seite. Der frühere Qt-Browser-Speicher unter `web/tumblr` und alte `news.json`-Dateien werden nicht mehr verwendet oder automatisch gelöscht.
- `minecraft`: eigener Spielordner mit Downloads, Spielständen und Minecraft-Protokollen.

**Abmelden** entfernt die gespeicherte Sitzung. „Angemeldet bleiben“ ist abschaltbar. Access-Tokens verbleiben im Arbeitsspeicher und werden beim Spielstart als vom Spiel benötigtes Argument übergeben; der Launcher schreibt sie nicht in ein Protokoll.

Alte Versionen ohne Laufzeitangabe können eine separat installierte Java-Version erfordern. Unter Optionen `javaw.exe` auswählen. Für ältere Releases typischerweise Java 8 verwenden. Ein manuell gesetzter Java-Pfad überschreibt die automatische Wahl für alle Versionen.

Ein eigenes Spielverzeichnis pro Versionswechsel schützt davor, eine neue Welt mit einer alten Spielversion zu öffnen. Bedrock, Modloader, Modpacks, mehrere Konten und ein Offline-Spielmodus sind nicht implementiert.

## Quellcode und eigener Build

Das optionale Archiv `RetroCraft-Quellcode.zip` enthält Quellcode, Browser-Hilfsprogramm, Build-Anleitung und Lizenzen. Nach dem Entpacken mit Python 3.12 (64 Bit):

```powershell
python -m venv .venv
.\.venv\Scripts\python -m pip install -r source\requirements.txt
.\.venv\Scripts\python source\launcher.py
.\.venv\Scripts\python -m unittest discover -s source -p "test_*.py" -v
```

Für eine Windows-Anwendung zusätzlich PyInstaller installieren und ausführen:

```powershell
.\.venv\Scripts\python -m pip install pyinstaller==6.22.3
.\.venv\Scripts\python -m PyInstaller --noconfirm source\RetroCraft.spec
```

Das Ergebnis ist die einzelne Datei `dist\RetroCraft.exe`. Sie ist nicht mit einem Herausgeberzertifikat signiert.

Zum Neubauen des mitgelieferten Browser-Hilfsprogramms das offizielle NuGet-Paket `Microsoft.Web.WebView2` in Version `1.0.4191.47` entpacken und `python source/browser/build.py PFAD_ZUM_ENTPACKTEN_SDK` aufrufen. Der vorhandene Windows-.NET-Framework-C#-Compiler wird verwendet; kein zusätzliches .NET-SDK ist notwendig. Bezugsquelle: https://www.nuget.org/packages/Microsoft.Web.WebView2/1.0.4191.47 .

Die Qt-Bibliotheken sind unverändert und dynamisch geladen. Mit dem mitgelieferten Anwendungsquellcode und der Build-Datei kann die EXE gegen kompatible, selbst geänderte Qt-/PySide6-Bibliotheken neu erstellt werden. Reverse Engineering zur Fehlersuche an solchen Änderungen ist erlaubt.

## Stand der Überprüfung

Siehe `PRUEFUNG.md` für die tatsächlich durchgeführten Prüfungen und verbleibende Grenzen. Die fest eingebaute App-ID wurde in den Anmelde- und Sitzungserneuerungsabläufen mit simuliertem Token-Austausch geprüft. Ein erfolgreicher echter Login samt Spielstart ist weiterhin nicht nachgewiesen.

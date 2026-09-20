# RetroCraft

Ein Minecraft-Java-Launcher für Windows im Stil des klassischen Launchers: Grasblock-Symbol, Erdtextur, RetroCraft-Logo und eine eingebettete Tumblr-Nachrichtenseite.

**NOT AN OFFICIAL MINECRAFT PRODUCT. Not approved by or affiliated with Mojang or Microsoft.**

## Funktionen

- Microsoft-Anmeldung im Standardbrowser mit automatischer Rückleitung zum Launcher.
- Auswahl von Releases, Snapshots und den von Mojang angebotenen alten Alpha-/Beta-/Classic-Versionen.
- Download von Spieldateien, Bibliotheken, Assets und der in den Versionsdaten angegebenen Java-Laufzeit.
- Optionen für Arbeitsspeicher, Spielverzeichnis, eigene Java-Laufzeit und „Angemeldet bleiben“.
- Die veröffentlichte [Tumblr-Seite](https://mcretroupdates.tumblr.com/) wird mit ihrem tatsächlichen Theme über Microsoft WebView2 angezeigt.
- Auslieferung als einzelne EXE mit rund 38 MB. Neben der EXE ist kein `_internal`-Ordner erforderlich.

## Voraussetzungen und Start

- Windows 10/11, 64 Bit.
- .NET Framework ab 4.6.2 und [Microsoft WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/).
- Internetverbindung für Anmeldung, Nachrichten und Downloads.
- Ein Microsoft-Konto mit Berechtigung für Minecraft Java Edition sowie eine für die Minecraft-Dienste freigegebene Launcher-App.

Wenn eine fertige Version angeboten wird, `RetroCraft-Launcher.exe` aus den GitHub **Releases** herunterladen und öffnen. Python ist dafür nicht nötig. Fehlt WebView2, zeigt der Launcher einen Link zur Installation an.

1. Unter **Optionen** die gewünschte Minecraft-Version auswählen.
2. Über **Microsoft-Login** anmelden.
3. **Spielen** wählen. Fehlende Dateien werden automatisch heruntergeladen.

Die EXE entpackt ihre eingebetteten Bibliotheken beim Start vorübergehend und entfernt sie beim normalen Beenden. Die separat installierte WebView2-Runtime, Browser-Cache und Minecraft-Dateien zählen nicht zur EXE-Größe.

## Anmeldedaten und lokaler Speicher

Der Launcher fragt kein Microsoft-Passwort ab und speichert keines. Die Anmeldung findet auf der Microsoft-Seite im Standardbrowser statt.

Bei aktiviertem **„Angemeldet bleiben“** wird ein Refresh-Token zusammen mit der öffentlichen App-ID lokal gespeichert. Windows DPAPI verschlüsselt diese Datei für das aktuelle Windows-Benutzerkonto. Abmelden oder Deaktivieren der Option entfernt die gespeicherte Sitzung. Zur Anmeldung und Sitzungserneuerung werden die benötigten Tokens mit Microsoft-/Xbox-/Minecraft-Diensten ausgetauscht; der Launcher betreibt dafür keinen eigenen Kontoserver.

Standardverzeichnis: `%APPDATA%\RetroMinecraftLauncher`.

| Pfad | Inhalt |
| --- | --- |
| `account.bin` | Verschlüsselte gespeicherte Anmeldung |
| `settings.json` | Launcher-Einstellungen |
| `versions.json` | Cache der offiziellen Versionsliste |
| `web/webview2/` | Separater Browser-Speicher für Tumblr |
| `minecraft/` | Standard-Spielverzeichnis mit Downloads und Spielständen |

Das Spielverzeichnis kann in den Optionen geändert werden. `RETRO_LAUNCHER_HOME` überschreibt das Launcher-Datenverzeichnis, etwa für isolierte Tests. Die Tumblr-Seite verwendet ihren eigenen Browser-Speicher und erhält keine Schnittstelle zu den Microsoft-Anmeldedaten des Launchers. Eine gespeicherte Anmeldung im normalen Browser wird durch das Abmelden im Launcher nicht gelöscht.

## Aus dem Quellcode starten

Die folgenden PowerShell-Befehle werden im Repository-Hauptverzeichnis ausgeführt. Benötigt werden Python 3.12 für Windows x64 und die oben genannten Windows-Komponenten.

### 1. Python-Abhängigkeiten installieren

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r source\requirements.txt
```

### 2. Browser-Hilfsprogramm bauen

Generierte EXE-/DLL-Dateien werden nicht im Git-Repository gespeichert. Deshalb muss der kleine WebView2-Host nach dem Klonen einmal gebaut werden. Das Skript verwendet den C#-Compiler des Windows-.NET-Frameworks; ein zusätzliches .NET-SDK ist dafür nicht erforderlich.

```powershell
New-Item -ItemType Directory -Force vendor | Out-Null
Invoke-WebRequest -Uri "https://api.nuget.org/v3-flatcontainer/microsoft.web.webview2/1.0.4191.47/microsoft.web.webview2.1.0.4191.47.nupkg" -OutFile "vendor\webview2-sdk.zip"
Expand-Archive -LiteralPath "vendor\webview2-sdk.zip" -DestinationPath "vendor\webview2-sdk" -Force
.\.venv\Scripts\python.exe source\browser\build.py vendor\webview2-sdk
```

Verwendetes SDK: [Microsoft.Web.WebView2 1.0.4191.47](https://www.nuget.org/packages/Microsoft.Web.WebView2/1.0.4191.47).

### 3. Launcher starten

```powershell
.\.venv\Scripts\python.exe source\launcher.py
```

## Einzelne Windows-EXE bauen

Nach den beiden Vorbereitungsschritten oben:

```powershell
.\.venv\Scripts\python.exe -m pip install pyinstaller==6.22.3
.\.venv\Scripts\python.exe -m PyInstaller --noconfirm source\RetroCraft.spec
```

Das Ergebnis liegt unter `dist\RetroCraft.exe`. Die Datei kann für eine Veröffentlichung in `RetroCraft-Launcher.exe` umbenannt und als GitHub-Release-Datei angehängt werden. Der Build ist nicht mit einem Herausgeberzertifikat signiert.

Die Datei `source/RetroCraft.spec` ist Teil des Quellcodes und muss im Repository bleiben. `build/`, `dist/`, heruntergeladene SDKs und die generierten Browser-Dateien werden von `.gitignore` ausgeschlossen.

## Tests

Zuerst den Browser-Host bauen. Anschließend lassen sich die vorhandenen Tests mit einem getrennten Datenverzeichnis ausführen; eine vorhandene Launcher-Anmeldung wird dadurch nicht verwendet:

```powershell
$previousLauncherHome = $env:RETRO_LAUNCHER_HOME
try {
    $env:RETRO_LAUNCHER_HOME = Join-Path $PWD ".test-data"
    .\.venv\Scripts\python.exe -m unittest discover -s source -p "test_*.py" -v
} finally {
    $env:RETRO_LAUNCHER_HOME = $previousLauncherHome
}
```

Die Tests verwenden unter anderem simulierte Microsoft-Antworten, einen lokalen Browser-Callback und Windows DPAPI. Sie ersetzen keinen echten Kontologin oder vollständigen Minecraft-Spielstart. Den dokumentierten Prüfstand beschreibt [PRUEFUNG.md](PRUEFUNG.md).

## Microsoft-App für eigene Builds

Die App-ID ist in `source/core.py` als `MICROSOFT_CLIENT_ID` festgelegt und wird nicht in den Einstellungen angezeigt. Eine Client-ID ist eine öffentliche Kennung, kein Passwort und kein Client-Secret.

Für eine eigene App-Registrierung müssen persönliche Microsoft-Konten unterstützt werden. Die Rückleitungsadresse lautet `http://localhost:28563/callback`; sie verweist auf den jeweiligen PC des Nutzers. Es handelt sich um eine öffentliche Desktop-Anwendung ohne Client-Secret. Die Freigabe für die Minecraft-Dienste bleibt erforderlich; das bloße Eintragen einer ID erteilt diese Freigabe nicht.

## Aktuelle Grenzen

- Unterstützt wird Vanilla Minecraft Java Edition. Bedrock, Modloader, Modpacks und mehrere Launcher-Konten sind nicht implementiert.
- Es gibt keinen Offline-Spielmodus und keine Umgehung der Minecraft-Lizenzprüfung.
- Historische Versionen können eine manuell ausgewählte Java-8-Laufzeit erfordern. Separate Spielverzeichnisse vermeiden, dass eine neue Welt versehentlich mit einer älteren Spielversion geöffnet wird.
- Ein erfolgreicher echter Microsoft-Login mit anschließendem vollständigem Spielstart ist im bisherigen Prüfbericht nicht nachgewiesen.

## Projektstruktur

```text
source/
  assets/             Logo, Texturen und Windows-Symbol
  browser/            C#-WebView2-Host und Build-Skript
  core.py             Anmeldung, Sitzungsablage, Installation und Spielstart
  launcher.py         Oberfläche und Optionen
  news.py             Einbettung der Tumblr-Seite
  requirements.txt    Python-Abhängigkeiten
  RetroCraft.spec     Build-Konfiguration für die einzelne EXE
  test_*.py           Vorhandene Tests
licenses/             Lizenztexte der verwendeten Fremdkomponenten
ANLEITUNG.md          Ausführliche Bedienungs- und Build-Hinweise
PRUEFUNG.md           Bisherige Prüfungen und Einschränkungen
THIRD-PARTY.md        Fremdkomponenten und Herkunft der Grafiken
```

## Lizenzen und Hinweise

Für den eigenen Launcher-Code ist bislang keine Projektlizenz festgelegt. Die Lizenzen der Fremdkomponenten und Hinweise zur Herkunft der Minecraft-Grafiken stehen in [THIRD-PARTY.md](THIRD-PARTY.md) und [licenses/](licenses/). In der Anwendung sind Lizenztexte unter **Optionen → Lizenzen** erreichbar.

Minecraft ist eine Marke von Mojang/Microsoft. Dieses unabhängige Fanprojekt ist nicht mit Mojang oder Microsoft verbunden.

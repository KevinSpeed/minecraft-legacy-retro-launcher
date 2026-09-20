# Fremdkomponenten

Die Anwendung verwendet unveränderte Bibliotheken. Deren Lizenztexte befinden sich im optionalen Quellcode-Archiv unter `licenses` und sind in der einzelnen EXE unter „Optionen → Lizenzen“ enthalten. Bibliotheken werden beim Start temporär entpackt.

- Qt / PySide6 Essentials / Shiboken 6.11.2: Qt for Python, LGPL/GPL bzw. jeweilige Modul-Lizenzen. Die Bibliotheken sind dynamisch geladen. Anwendungsquellcode und Build-Datei liegen bei, sodass eine EXE mit kompatiblen, geänderten Bibliotheken erstellt werden kann. Quellen: https://download.qt.io/official_releases/QtForPython/ und https://code.qt.io/cgit/pyside/pyside-setup.git/ sowie https://download.qt.io/official_releases/qt/.
- minecraft-launcher-lib 8.0: BSD-2-Clause. https://github.com/JakobDev/minecraft-launcher-lib
- requests, urllib3, idna, charset-normalizer, certifi: jeweilige Lizenztexte beiliegend.
- Python 3.12: Python Software Foundation License; Laufzeit-Lizenz liegt im Paket.
- Microsoft.Web.WebView2 SDK 1.0.4191.47: unveränderte .NET-Core-Bibliothek und x64-Loader, Lizenz im Quellcode unter `source/browser/bin/WebView2-LICENSE.txt`, ebenfalls in der EXE enthalten. Quelle: https://www.nuget.org/packages/Microsoft.Web.WebView2/1.0.4191.47 . Der eigene C#-Host liegt als Quellcode bei. Die separat installierte Microsoft WebView2 Runtime und das Windows-.NET-Framework werden nicht in der EXE mitverteilt. Qt WebEngine und dessen Chromium-Kopie sind nicht mehr enthalten.
- PyInstaller 6.22.3 dient zur Paketierung; Bootloader-Ausnahme gilt für erzeugte Programme. https://pyinstaller.org/

Der eigene Launcher-Quellcode wird vollständig unter `source` mitgeliefert. Die Qt-Bibliotheken wurden nicht verändert; Änderungen an diesen Bibliotheken können separat gebaut und ausgetauscht werden. Reverse Engineering zur Fehlersuche an Änderungen dieser LGPL-Komponenten wird nicht eingeschränkt.

Minecraft-Spielinhalte, Java-Downloads und Bibliotheken des Spiels werden erst bei der Installation von den in Mojangs Versionsdaten genannten Servern geladen und gehören nicht zu diesem Paket. Minecraft ist eine Marke von Mojang/Microsoft. Dieses Fanprojekt ist nicht mit ihnen verbunden.

## RetroCraft-Logo und Tumblr

`source/assets/grass-block.png` ist das unveränderte 32-Pixel-Grasblock-Symbol des historischen Launchers aus https://github.com/lretq/legacy-launcher/blob/master/src/main/resources/favicon.png (Mojang/Microsoft). Es wird als Fenster-, Taskleisten- und EXE-Symbol verwendet. Die Windows-ICO-Datei enthält davon Bitmap-Versionen in 16, 24, 32, 48, 64, 128 und 256 Pixeln; die 32-Pixel-Version bleibt pixelgenau erhalten.

`source/assets/retrocraft-logo.png` ist das für den Nutzer generierte RetroCraft-Logo. Die Originaldatei
wird unverändert mitgeliefert und für die Darstellung transparent und proportional in die untere
Launcher-Leiste eingepasst. Sie ersetzt den bisherigen Minecraft-Schriftzug.

Die vom Nutzer angegebene Website https://mcretroupdates.tumblr.com/ wird zur Laufzeit mit ihrem veröffentlichten HTML, CSS, JavaScript und ihren Bildern geladen. Das Website-Theme wird nicht im Launcher nachgebaut. Inhalte der Seite werden nicht als Artikelarchiv im Paket mitverteilt.

`source/assets/dirt.png` ist die unveränderte historische Erdtextur aus https://github.com/lretq/legacy-launcher/blob/master/src/main/resources/dirt.png (Mojang/Microsoft). Die Darstellung verwendet wie der historische Launcher eine 64-Pixel-Kachel, eine helle Oberkante und eine nach unten zunehmende Abdunklung.

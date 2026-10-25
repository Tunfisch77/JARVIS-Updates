# JARVIS Linux v15.3.0

Komplettpaket für Linux Mint 22.3 Cinnamon / Ubuntu 24.04, x86-64, Python 3.12. Diese Ausgabe ergänzt die fehlenden Linux-Adapter. **Vollständige Gleichheit mit Windows ist noch nicht nachgewiesen.** Die grafischen Funktionen müssen in deiner VMware-VM geprüft werden. Version 16 gehört nicht zu diesem Update.

## Neu installieren

1. ZIP in Linux in deinen persönlichen Ordner entpacken, z. B. `~/JARVIS_Linux_v15_3_0`. Nicht im ZIP oder unter `/mnt/hgfs` starten.
2. Entpackten Ordner öffnen → Rechtsklick → „Im Terminal öffnen“.
3. `bash INSTALLIEREN.sh` ausführen, ohne `sudo` davor. Nur Systempakete verlangen dein Linux-Passwort. Internet ist nötig; Python-Pakete und Chromium werden nachgeladen.
4. Deinen OpenAI-API-Schlüssel bei der lokalen, unsichtbaren Abfrage eingeben. Enter überspringt die Eingabe. Später: `bash API_SCHLUESSEL_EINTRAGEN.sh`.
5. `bash STARTEN.sh` ausführen. Mikrofon erlauben und Sprachverbindung einschalten. Die Oberfläche muss **15.3.0** anzeigen.

JARVIS öffnet ein eigenes Chromium-Appfenster ohne normale Browserleiste. Verwendet werden vorhandenes Brave/Chrome/Chromium oder das von Playwright installierte Chromium. Die Sandbox bleibt aktiv. `bash STARTEN.sh --browser` öffnet die Standardbrowser-Ansicht. Das neue Fenster hat ein eigenes Profil; Mikrofonberechtigungen dort erneut erteilen.

## Von Linux v15.2 aktualisieren

Im bisherigen JARVIS-Ordner:

```bash
bash UPDATE_JARVIS.sh
bash STOPPEN.sh
bash INSTALLIEREN.sh
bash STARTEN.sh
```

Der Installer ist bei diesem Update erforderlich: GI, AT-SPI, Audio- und Portal-Werkzeuge kommen hinzu. Er übernimmt vorhandene Einstellungen und Daten und prüft eine neue Python-Umgebung vor dem Umschalten. Alte Umgebungen bleiben als Rückfallmöglichkeit erhalten und benötigen zusätzlichen Speicher.

## Lösungen für die sieben Lücken

| Funktion | In v15.3 ergänzt | Voraussetzung / Grenze |
| --- | --- | --- |
| „Speichern“ per Namen anklicken | AT-SPI sucht ein eindeutig benanntes, sichtbares, aktiviertes Bedienelement. Mehrdeutige Treffer werden abgelehnt. | Die Ziel-App muss Linux-Barrierefreiheit unterstützen. Für andere Oberflächen bleiben bestätigte Koordinaten. |
| Installierte Programme finden | Gio durchsucht registrierte Startmenü-Apps, auch korrekt exportierte Flatpak-Starter. App-IDs lösen Namenskonflikte. | Programme ohne registrierten Starter werden nicht automatisch erkannt. Terminal-/Skriptstarter bleiben gesperrt, solange die Terminalberechtigung fehlt. |
| Eigenes JARVIS-Fenster | Chromium-Appansicht mit eigenem Profil über STARTEN.sh. | Browsertechnologie bleibt die Grundlage, kein Port der Windows-App-Shell. |
| WhatsApp | Web-Integration für JARVIS; WHATSAPP_APP.sh für ein eigenes manuelles Webfenster. | Keine offizielle native Linux-App. Windows-spezifische Funktionen, Anrufe und sämtliche Web-Abläufe sind nicht garantiert. |
| Windows-Systemmodule | AT-SPI für UI; PulseAudio/PipeWire-Pulse für Lautstärke/Stummschaltung; BlueZ für gekoppelte Bluetooth-Geräte; lokale Benachrichtigungen. | Konkrete Aufgaben werden ersetzt, nicht komplette Windows-SDKs. UFO²/PyWinRT sind nicht vollständig portiert. |
| Wayland | RemoteDesktop-Portal für Eingaben, Screenshot-Portal für Bilder; eine Freigabesitzung pro Plan. | Desktop muss das jeweilige Portal unterstützen; dessen Freigabedialog muss bestätigt werden. Bei fehlendem Backend Cinnamon mit X11 verwenden. |
| Optionale Module | MODULE_INSTALLIEREN.sh baut Umgebungen getrennt auf, prüft Abhängigkeiten und erhält bei Fehlern die bisherige Umgebung. | Nicht jedes experimentelle Drittanbieterpaket läuft unter Linux. Installation richtet keine Konten, Modelle oder vollständigen Integrationen automatisch ein. |

Wenn ausnahmslos jede Windows-Funktion unverändert benötigt wird, bleibt eine Windows-Instanz für Windows-Programme erforderlich. **Eine Windows-Brücke ist in diesem Paket nicht implementiert.** Microsoft UFO³ bietet auch Linux-Geräteagenten an; diese eigene Integration macht den vorhandenen UFO²-Eintrag nicht automatisch Linux-fähig.

## Desktop und Systemaktionen

Die Desktopsteuerung wie bisher im PC-Agenten aktivieren. Aktionen bleiben bestätigt; Not-Aus verhindert weitere Schritte, macht bereits ausgeführte Aktionen aber nicht rückgängig.

Der Installer aktiviert die Barrierefreiheitsschnittstelle von Cinnamon/GNOME, sofern verfügbar. Bereits laufende Ziel-Apps danach neu starten. Qt-Starter und das JARVIS-Appfenster bekommen die entsprechende Unterstützung. Trotzdem kann eine App keine brauchbaren Bedienelemente anbieten.

X11-Koordinaten beziehen sich auf den gesamten X11-Desktop. Unter Wayland **genau einen Monitor** freigeben; Koordinaten sind dessen logische Pixel. Keine Koordinaten aus anders skalierten Bildern übernehmen. Wayland kann Fensterfokus ablehnen; in dem Fall das Zielfenster selbst in den Vordergrund holen. Der aktive Portalauftrag bleibt stoppbar, besitzt aber keine Fortschrittsanzeige pro Einzelschritt.

Neue strukturierte Planaktionen:

- `set_volume`: `volume` 0–100, `audio_scope` `sink` oder `stream`, `audio_target` mit Kennung. `@DEFAULT_SINK@` bezeichnet die Standardausgabe.
- `set_mute`: dieselben Zielangaben, dazu `mute` als true/false.
- `bluetooth_connect` / `bluetooth_disconnect`: genaue `address` eines bereits in Mint gekoppelten Geräts.
- `notify`: lokale Desktop-Benachrichtigung mit `title` und `text`.

JARVIS liest Geräte-/Streamkennungen über seine Systemdiagnose. App-Streams erscheinen erst, wenn die Anwendung Audio ausgibt. In VMware braucht Bluetooth gegebenenfalls einen durchgereichten USB-Adapter; der virtuelle Lautsprecher allein stellt keinen Bluetooth-Adapter bereit.

## Bildschirm und Datenschutz

X11- und Browseraufnahmen verarbeitet JARVIS im Arbeitsspeicher. Das Wayland-Screenshot-Portal liefert eine **vom Desktop verwaltete Bilddatei**. JARVIS erstellt keine zusätzliche Kopie und löscht keine möglicherweise vom Nutzer gewählte Datei. Die Analyseantwort weist darauf hin. Für KI-Analyse wird das bestätigte Standbild an den konfigurierten Cloud-Dienst geschickt. Kein permanenter Bildschirmmitschnitt.

## Zusatzmodule

```bash
bash MODULE_INSTALLIEREN.sh
bash MODULE_INSTALLIEREN.sh voice
bash MODULE_INSTALLIEREN.sh agents memory vision-media
```

Ohne Argumente werden Gruppen angezeigt. `all` versucht alle Gruppen einschließlich großer experimenteller Pakete. Open Interpreter (vorhandene 0.4-Integration), OpenAdapt und OpenVoiceOS erhalten getrennte Umgebungen. Fehler werden pro Umgebung gemeldet, funktionierende Umgebungen bleiben erhalten. Installierte Versionen werden dort dokumentiert. Diese optionalen Gruppen wurden nicht alle praktisch unter Mint getestet.

Ollama, Home Assistant, Frigate, OmniParser und UI-TARS sind eigene Dienste/Modelle. URLs, Zugangsdaten und Hardware müssen weiterhin separat eingerichtet werden. Das alte Windows-SQLCipher-Paket wird nicht als funktionsfähige Linux-Datenbankverschlüsselung ausgegeben. Windows-SDKs bleiben im Modulbereich als nicht unterstützt markiert, mit ihrem Linux-Ersatz. Experimentelle Recorder/Agenten starten nicht automatisch.

## Konten

Das Paket enthält keine Logins, Kontakte, Browserprofile oder Datenbank.

- `bash WHATSAPP_VERBINDEN.sh`: QR-Code über „Verknüpfte Geräte“ scannen, nach Anmeldung Enter im Terminal.
- `bash WHATSAPP_APP.sh`: separates Webfenster mit eigenem Profil und **separatem QR-Login**, damit es die Automationsverbindung nicht sperrt.
- `bash OUTLOOK_VERBINDEN.sh`: im Browser anmelden.
- `bash GMAIL_VERBINDEN.sh`: eigene Google-OAuth-Desktop-Clientdatei nötig; Pfad lokal in `.env` eintragen.
- `bash SNAPCHAT_VERBINDEN.sh`: nur Anmeldung/Status; kein vollständiger Inhaltszugriff oder Versand.

Die Verbindungsschaltflächen öffnen in Mint den Anmeldeablauf im Terminal. Das Backend wird währenddessen angehalten, damit kein Browserprofil mehrfach geöffnet wird. Sprachverbindung danach neu verbinden. Deine echten Konten wurden hier nicht getestet.

## Alltag und Diagnose

| Befehl | Wirkung |
| --- | --- |
| `bash STARTEN.sh` | Backend und Appfenster |
| `bash STARTEN.sh --browser` | Backend und Standardbrowser |
| `bash STARTEN.sh --no-browser` | Nur Backend |
| `bash STOPPEN.sh` | Backend sauber beenden |
| `bash STATUS.sh` | Laufenden Server prüfen |
| `bash DIAGNOSE.sh --browser` | Systemadapter und sandboxierten Browser prüfen |
| `bash UPDATE_JARVIS.sh` | Linux-Updatekanal prüfen |
| `bash AUTOSTART_AN.sh` / `bash AUTOSTART_AUS.sh` | Backend-Autostart nach Desktop-Anmeldung |

Oberfläche: http://127.0.0.1:8017. Es gibt einen Mint-Startmenüeintrag. Nach Verschieben des Ordners Installer erneut ausführen. Für Mikrofon, Sprache und gesprochene Antworten muss das Appfenster oder die Browseransicht verbunden bleiben. Das Backend allein ist kein ständig lauschender Sprachdienst. Nach hartem Ausschalten kann Safe Mode greifen.

Eigene Daten: `.env`, `data/` und standardmäßig `~/.jarvis/`; vor einem Umzug privat sichern. Updates schützen außerdem `.venv` und `.jarvis_envs`. Diagnose: `data/linux-diagnose.json`; Startfehler: `data/linux-server.log`. Protokolle vor Teilen auf persönliche Inhalte prüfen. Windows-Dateien sind historische Quellen für Regressionstests; unter Linux die `.sh`-Dateien verwenden.

## Nachweise

Siehe TESTBERICHT_LINUX.md. Automatisierte Tests bestätigen keine vollständige Funktion fremder Desktop-Apps.

Technische Quellen: [AT-SPI](https://lazka.github.io/pgi-docs/Atspi-2.0/classes/Action.html), [Gio](https://docs.gtk.org/gio/method.AppInfo.launch.html), [RemoteDesktop-Portal](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.RemoteDesktop.html), [Screenshot-Portal](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.Screenshot.html), [pactl](https://manpages.debian.org/bookworm/pulseaudio-utils/pactl.1.en.html), [BlueZ](https://bluez.readthedocs.io/en/latest/device-api/), [WhatsApp-Desktop](https://www.whatsapp.com/download/desktop), [Microsoft UFO³](https://github.com/microsoft/UFO/blob/main/galaxy/README.md).

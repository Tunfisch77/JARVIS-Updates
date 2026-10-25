# Prüfnachweis – JARVIS Linux v15.3.0

Stand: 25.10.2026. Prüfplattform: Linux-Container, Python 3.12.14. Keine Mint-/VMware-Desktop-Sitzung verfügbar.

## Automatisiert geprüft

- Vollständige Python-Suite: **304 Tests und 16 Untertests bestanden**. Fünf bestehende FastAPI/Starlette-Deprecation-Warnungen; kein Testfehler.
- Python-, Shell- und Browser-JavaScript-Syntax geprüft. Kernabhängigkeiten frisch aus requirements-linux-lock.txt installiert; pip check erfolgreich.
- Eindeutige App-Auswahl, Namenskonflikte, Terminal-Sperre; AT-SPI-Klick, Mehrdeutigkeiten, unvollständige Suche; Wayland-Ablehnung vor Planbeginn, Monitorgrenzen, Unicode/Hotkeys, Tastenfreigabe, Sitzungsende und Not-Aus.
- Ehrliche Metadaten für Portal-Bilddateien, Audioziel-/Lautstärkeprüfung und Bestätigung, getrennte Appfensterprofile mit aktiver Sandbox.
- Fehlgeschlagene Modulinstallation erhält die alte Umgebung. Fehlgeschlagener Umgebungswechsel stellt das ursprüngliche Verzeichnis wieder her.
- Fremde und wiederverwendete Prozess-IDs werden anhand kontrollierter Metadaten abgelehnt. Die Testumgebung besitzt keine normale /proc-Prozesssicht; dieser Teil wurde daher simuliert.

Gio, AT-SPI, Portale, Audio und Bluetooth wurden mit kontrollierten Test-Doubles geprüft. Das bestätigt Ablauf und Fehlerbehandlung, nicht die tatsächliche Reaktion von Cinnamon, GNOME, KDE oder einem Gerät.

## Backend und Upgrade

- Der originale v15.2-Updater installierte das v15.3-Paket: 22 geänderte Programmdateien, Backup bytegenau geprüft.
- Test-.env, Datenbank, persönliche Testdatei und Marker in .venv/.jarvis_envs blieben beim Update bytegenau erhalten.
- Alle installierten Paketdateien wurden gegen die ZIP verglichen; erneuter Updateaufruf erkennt „bereits aktuell“.
- Das aktualisierte Backend wurde über Uvicorn zweimal tatsächlich auf localhost gestartet, über HTTP geprüft und sauber beendet. Version/Oberfläche, PC-Status, Modulübersicht, Diagnose und Stabilitätsstatus erreichbar. Nach sauberem Neustart kein Safe Mode; Datenbanksentinel erhalten.
- Der gesamte grafische Launcher wurde in diesem Container nicht ausgeführt: dessen Prozessbesitzprüfung benötigt eine normale /proc-Sicht. Die Produktionsprüfung bleibt bestehen; fremde und wiederverwendete PIDs sind separat getestet.
- ZIP-Integrität und enthaltene Pfade geprüft: keine .env, Datenbank, Browserprofile, Laufzeitumgebungen oder persönlichen Daten im Auslieferungspaket.

Ausschließlich isolierte Testdaten; keine echten Nachrichten oder Konten.

## In deiner VMware-VM offen

1. Installation und Startmenü/Appfenster; Mikrofon erlauben und Sprachverbindung prüfen.
2. Zusätzlich installierte Startmenü-App öffnen; bei Namenskonflikten App-ID verwenden.
3. In einem Testdokument eine eindeutig benannte Schaltfläche anklicken. Ziel-App nach Aktivieren der Barrierefreiheit neu starten.
4. Mehrschritt-Plan stoppen. Bereits abgeschlossene Schritte bleiben bestehen.
5. System- und App-Lautstärke/Stummschaltung prüfen; Bluetooth nur mit durchgereichtem und gekoppeltem Adapter/Gerät.
6. WhatsApp verbinden und Posteingang/Entwurf prüfen. Testversand nur bewusst an gewählten Empfänger.
7. Wayland: Portal in Diagnose prüfen, Freigabe verweigern/erteilen, einen Monitor auswählen, Aufnahme/Stoppen testen. Ohne Portalbackend Cinnamon-X11 verwenden.
8. Mehrere Tage normal starten, stoppen und VM neu starten; Audioaussetzer, Zielerkennung und Safe-Mode-Auslöser beobachten.

Nicht nachgewiesen: komplette Mint-/VMware-Desktopsteuerung, alle optionalen Paketgruppen, persönliche Konten, Mikrofon/Grafik/Bluetooth-Hardware, identische Windows-WhatsApp-Funktionen oder vollständiger UFO²/PyWinRT-Ersatz. „Alles funktioniert 1:1“ wäre durch diese Prüfungen nicht gedeckt.

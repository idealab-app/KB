# Kassenbuch

Ein gemeinsames Haushaltsbuch nach der **Umschlagmethode** – als schlanke Web-App, die auf dem Handy wie eine eigene App vom Startbildschirm aus geöffnet wird.

Jeder Euro bekommt eine Aufgabe: Einnahmen minus Fixkosten, Sparraten und persönliches Budget ergeben das Haushaltsbudget, das auf Umschläge (Kategorien) verteilt wird. Ausgaben werden in Umschläge gebucht, und man sieht jederzeit, wie viel in jedem Umschlag noch übrig ist.

---

## Funktionen

### Übersicht
- **Vier Ansichten:** *Gemeinsam* (Haushalt), je eine Ansicht pro Person, *N* und *M* (persönliche Budgets) sowie *🏆 Sparen* (Spartopf & Sparduell)
- **Monatswechsel** über ‹ › – frühere Monate bleiben einsehbar
- **Kennzahlen:** noch in den Umschlägen, übrig nach Fixkosten, ausgegeben im Monat
- **Hinweis „ohne Aufgabe“:** zeigt, wie viel Budget noch keinem Umschlag oder Sparziel zugeordnet ist (oder ob zu viel verplant ist)

### Umschläge (Kategorien)
- Fortschrittsbalken pro Umschlag: ausgegeben / Budget / Rest, Überzug wird rot markiert
- **Antippen** filtert die Buchungen auf diesen Umschlag
- **✎ Anpassen:** Name, Budget und Zuordnung (gemeinsam oder eine der beiden Personen) ändern oder Umschlag löschen
- **Budget nur für einen Monat ändern:** Abweichung gilt nur im gewählten Monat (Kennzeichnung „nur Nov“), danach wieder das Standardbudget
- **+ Umschlag** legt neue Kategorien an
- **Monatsanfang:** Am 1. jedes Monats starten alle Umschläge bei 0 € ausgegeben – kein Übertrag aus dem Vormonat

### Buchungen
- **+ Ausgabe eintragen:** Betrag, Umschlag, Beschreibung, Datum, bezahlt von (eine der beiden Personen oder **Gemeinsam**, z. B. vom Gemeinschaftskonto)
- **🎤 Diktieren:** Ausgabe einsprechen, z. B. „23,40 Euro bei Lidl“ oder „gestern zwölf Euro fünfzig Bäckerei“. Die App teilt automatisch auf in **Betrag**, **Händler/Beschreibung**, **Datum** (heute, gestern, vorgestern) und **bezahlt von** („ich“ → die auf diesem Gerät eingestellte Person, „vom gemeinsamen Konto“ → Gemeinsam) und schlägt einen **Umschlag** vor: zuerst anhand früherer Buchungen beim selben Händler, sonst über Stichworte (z. B. Aral → Tanken, dm → Drogerie). Mit „für mich“ oder „privat“ wird ein persönlicher Umschlag gewählt. Gespeichert wird erst nach Prüfen und Tippen auf *Speichern*.
- Funktioniert mit der Spracherkennung des Browsers (Chrome, Safari). Wo sie fehlt, kann man das Mikrofon der Handytastatur ins Schnelleingabefeld sprechen – die Aufteilung klappt genauso.
- Buchungen antippen zum Bearbeiten oder Löschen
- Änderungen erscheinen sofort auf beiden Handys (Echtzeit-Synchronisation)
- Funktioniert dank Offline-Cache auch bei schlechtem Empfang; Daten werden nachgeladen, sobald wieder Verbindung besteht

### 🏆 Sparen: Spartopf & Sparduell
- **Spartopf:** Was am Monatsende in den gemeinsamen Umschlägen übrig bleibt, wird über alle abgeschlossenen Monate gesammelt. Überzogene Monate ziehen davon ab. So sieht man, wie viel langfristig zusätzlich gespart wird.
- **Sparduell zwischen beiden Personen:** Jeder Monat ist eine Runde. Es gewinnt, wer weniger aus den eigenen persönlichen Umschlägen ausgegeben hat. Der Gewinner bekommt den Rest **beider** persönlicher Budgets; bei Gleichstand wird geteilt.
- **Punkte:** +1 je € übrig im persönlichen Budget · +5 je Tag ohne persönliche Ausgabe · +2 je eingetragener Buchung · −25 je überzogenem persönlichen Umschlag
- **Gesamtwertung:** Siege, Punkte und gewonnene Beträge aller abgeschlossenen Monate
- **Vergangene Runden:** Gewinner und Betrag je Monat, mit Häkchen „ausgezahlt“, sobald das Geld überwiesen ist

### Einnahmen & Monatsplan
- **Einnahmen** pro Monat überschreibbar (z. B. schwankendes Gehalt), ohne den Standardwert zu ändern
- **Monatsplan:** Einnahmen → Fixkosten → Sparen & Rücklagen → persönliche Budgets → Haushaltsbudget für Umschläge

### Einstellungen (⚙︎)
- Einnahmen, Fixkosten, Sparraten, persönliche Budgets und Umschläge als Liste bearbeiten
- **Datenquelle:** Link zur Excel-Planung (z. B. OneDrive/SharePoint) hinterlegen – erscheint auch im Monatsplan
- **„Ich bin“:** legt pro Gerät fest, wer standardmäßig als „bezahlt von“ eingetragen wird
- Abmelden

### Anmeldung & Zugriff
- Anmeldung mit **E-Mail + Passwort** (inkl. E-Mail-Bestätigung und „Passwort vergessen“) oder **mit Google**
- Zugriff nur für freigeschaltete, bestätigte E-Mail-Adressen (Firestore-Sicherheitsregeln)

---

## Aufbau

| Datei | Zweck |
|---|---|
| `index.html` | Die komplette App (HTML, CSS, JavaScript) inkl. Firebase-Anbindung |
| `manifest.json` | Name, Farben und Icon für die Kachel auf dem Startbildschirm |
| `icon-180.png` | App-Icon für iPhone/iPad |
| `icon-512.png` | App-Icon für Android |
| `README.md` | Diese Beschreibung |

**Technik:** reines HTML/JavaScript ohne Build-Schritt · Hosting über **GitHub Pages** · Daten und Login über **Firebase** (Cloud Firestore + Authentication) · Firebase SDK wird von `gstatic.com` geladen.

### Datenmodell (Firestore)

| Pfad | Inhalt |
|---|---|
| `budget/config` | Einnahmen, Fixkosten, Sparraten, persönliche Budgets, Umschläge, Link zur Datenquelle |
| `months/{JJJJ-MM}` | Abweichungen für einen Monat (Einnahmen, Umschlag-Budgets) und Auszahlungsstatus des Sparduells |
| `tx/{id}` | Einzelne Buchung: Betrag, Umschlag, Beschreibung, Datum, Monat, bezahlt von |

---

## Datenschutz & Sicherheit

- **In diesem Repository liegen keine Finanzdaten und keine E-Mail-Adressen.** Repository und Seite sind öffentlich, die Daten liegen ausschließlich in Firebase.
- Der Firebase-`apiKey` in `index.html` ist kein Geheimnis – er identifiziert nur das Projekt. Geschützt werden die Daten durch die **Firestore-Regeln**:
  - Lesen/Schreiben nur mit **bestätigter, freigeschalteter E-Mail-Adresse**
  - nur die drei Bereiche `budget/config`, `months/*`, `tx/*` – alles andere ist gesperrt
  - Prüfung der Felder (erlaubte Felder, Betrag als Zahl, Längen, Link nur `https://`)
- Die Regeln liegen **nicht** hier, sondern in der Firebase-Konsole unter *Firestore Database → Regeln*.
- **Abmelden** löscht auch die lokal zwischengespeicherte Kopie der Daten auf dem Gerät.
- Alle Inhalte werden als Text dargestellt (kein HTML aus der Datenbank), Links nur mit `https://`.
- Bitte **keine** Finanzdaten, Kontonummern, E-Mail-Adressen oder die Startdaten-Datei in dieses Repository hochladen.

### Empfohlene Einstellungen in Firebase / Google Cloud
- *Authentication → Settings → User actions:* nach dem Anlegen beider Konten **„Enable create (sign-up)“ ausschalten**
- *Authentication → Settings:* **E-Mail-Enumeration-Schutz** aktiv lassen, **Passwortrichtlinie** (z. B. min. 12 Zeichen) setzen
- *Google Cloud Console → APIs & Dienste → Anmeldedaten:* API-Key auf **Websites** beschränken (`https://DEINNAME.github.io/*` und `https://PROJEKT.firebaseapp.com/*`)
- Firestore-Standort in der EU wählen

---

## Einrichtung

1. **Firebase-Projekt** anlegen, Web-App registrieren, Konfiguration in `index.html` (`firebaseConfig`) eintragen
2. **Firestore** aktivieren (EU-Region, z. B. `europe-west3`) und die Sicherheitsregeln mit den erlaubten E-Mail-Adressen veröffentlichen
3. **Authentication** aktivieren: *E-Mail-Adresse/Passwort* (und optional *Google*)
4. Dateien in dieses Repository hochladen, unter *Settings → Pages* den Branch `main` / Ordner `/(root)` veröffentlichen
5. In Firebase unter *Authentication → Settings → Autorisierte Domains* die eigene `github.io`-Domain eintragen
6. App öffnen, Konto erstellen, E-Mail bestätigen, einmalig **„Startdaten aus der Excel-Liste anlegen“** tippen

### Als Kachel auf den Startbildschirm
- **iPhone:** Seite in Safari öffnen → Teilen → *Zum Home-Bildschirm*
- **Android:** Seite in Chrome öffnen → Menü ⋮ → *Zum Startbildschirm hinzufügen*

Nach einem Icon-Wechsel die Kachel einmal löschen und neu anlegen.

---

## Häufige Fragen

**„Keine Berechtigung“ oder „hat keinen Zugriff“** – Die E-Mail-Adresse steht nicht (oder nicht kleingeschrieben) in den Firestore-Regeln, oder sie ist noch nicht bestätigt.

**Google-Login klappt nicht** – Die `github.io`-Domain fehlt unter *Autorisierte Domains*, oder Pop-ups sind blockiert. Alternativ mit E-Mail + Passwort anmelden.

**Änderungen an `index.html` sind nicht sichtbar** – GitHub Pages braucht nach dem Hochladen 1–2 Minuten; danach die Seite neu laden.

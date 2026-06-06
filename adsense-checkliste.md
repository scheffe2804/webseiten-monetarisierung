# AdSense-Checkliste

Stand: 2026-06-06

Ziel: Die drei Webseiten auf Google-AdSense-Tauglichkeit prüfen und eine sinnvolle Reihenfolge für Freigabe, Einbau und Tests festlegen.

Hinweis: Das ist eine technische und organisatorische Checkliste, keine Rechtsberatung.

## Kurzantwort zum Google-Zugriff

Der vorhandene dokumentierte Google-Zugriff reicht nach aktuellem Stand **für Analytics- und Search-Console-Auswertungen**, aber **nicht nachweislich für AdSense**.

Bekannt/dokumentiert ist:

- Google Analytics 4 ist eingerichtet und eingebunden.
- Google Search Console hat Owner-Zugriff für die drei Zielseiten.
- Frühere Sitzungen konnten Analytics Admin/Data API und Search Console auswerten.

Nicht bekannt/dokumentiert ist:

- ob ein Google-AdSense-Konto existiert: inzwischen durch Nutzerstart im Browser praktisch bestätigt
- ob die drei Seiten in AdSense angelegt sind
- ob eine AdSense-Publisher-ID existiert (`pub-...`): inzwischen bekannt, siehe unten
- ob eine AdSense-Freigabe, Ablehnung oder Policy-Meldung existiert
- ob AdSense-API-Scopes vorhanden sind

In der aktuellen Sitzung ist außerdem kein direkter Google-MCP/API-Server sichtbar. Deshalb kann hier aktuell nicht live in AdSense nachgesehen werden.

Für den nächsten praktischen AdSense-Schritt braucht es also entweder:

1. einen manuellen Blick in die AdSense-Oberfläche durch den Nutzer, oder
2. erneut verfügbaren Google-/AdSense-API-Zugriff in der Arbeitsumgebung, oder
3. Export/Screenshot/Angaben aus AdSense: Publisher-ID, Site-Status, Policy Center, Ablehnungsgründe.

## Wichtig: Was die AdSense-API leisten kann und was nicht

Auch mit API-Zugriff ist AdSense nicht vollständig automatisierbar.

Möglich per AdSense Management API, falls passend berechtigt:

- AdSense-Konten listen
- Ad Clients / Publisher-Informationen auslesen
- Berichte abrufen
- ggf. vorhandene Sites/Status auslesen, je nach API-Version und Berechtigung

Nicht zuverlässig per API ersetzbar:

- erstmalige Kontoerstellung / ToS-Flows
- finale Website-Freigabe durch Google
- manche Policy-Center-Details
- manuelle Identitäts-, Zahlungs- oder Steuerangaben

Darum kommt der echte AdSense-Konto-/Freigabecheck wahrscheinlich als separater Schritt.

## Gesamtbefund

## AdSense-Bestand nach Nutzerangabe vom 2026-06-06

Der Nutzer hat in der AdSense-Oberfläche den Website-Verknüpfungscode für `sauerlandaktuell.de` erhalten:

```html
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-6440027702756215"
     crossorigin="anonymous"></script>
```

Daraus abgeleitet:

- Publisher-ID: `pub-6440027702756215`
- AdSense Client-ID: `ca-pub-6440027702756215`
- `ads.txt`-Zeile: `google.com, pub-6440027702756215, DIRECT, f08c47fec0942fa0`

Technischer Umsetzungsstand:

- Auf `sauerlandaktuell.de` wurde ein kleines MU-Plugin `adsense-setup.php` vorbereitet/ausgerollt.
- Es gibt den AdSense-Verknüpfungscode im HTML-Head aus.
- Es liefert `/ads.txt` mit der korrekten Google-Zeile aus.
- Öffentliche Prüfung am 2026-06-06:
  - `https://sauerlandaktuell.de/?adsense_check=...` enthielt `ca-pub-6440027702756215` und `pagead2.googlesyndication.com/pagead/js/adsbygoogle.js`.
  - `https://sauerlandaktuell.de/ads.txt` lieferte HTTP 200 und `google.com, pub-6440027702756215, DIRECT, f08c47fec0942fa0`.

Wichtig: Dies ist zunächst der AdSense-Verknüpfungs-/Prüfcode. Datenschutz-/Cookie-Texte und spätere Anzeigenkonfiguration müssen noch sauber nachgezogen werden.

### Rechtstexte nachgezogen am 2026-06-06

Die Live-WordPress-Seiten von `sauerlandaktuell.de` wurden per WP-CLI aktualisiert:

- Datenschutzseite: `https://sauerlandaktuell.de/datenschutzerklaerung/`
- Cookie-Richtlinie: `https://sauerlandaktuell.de/cookie-richtlinie/`

Vorherige Inhalte wurden auf dem Live-Host gesichert unter:

- `/home/chris/web/sauerlandaktuell.de/backups/adsense-legal-20260606/datenschutzerklaerung-before.html`
- `/home/chris/web/sauerlandaktuell.de/backups/adsense-legal-20260606/cookie-richtlinie-before.html`

Ergänzt/korrigiert wurde:

- Google AdSense / Google Werbung inklusive Publisher-ID `pub-6440027702756215`
- Google Analytics 4 mit `G-CR8ETZEYRB`
- Matomo unter `stats.sauerlandaktuell.de`
- Cookie-/Consent-Hinweis auf `cmplz_` statt falsch erwähntem `afd_cookie_consent`
- Google-CMP-/Consent-Hinweis
- Stand der Rechtstexte auf `6. Juni 2026`

Öffentliche Prüfung am 2026-06-06:

- Datenschutzseite lieferte HTTP 200 und enthielt `Google AdSense`, `pub-6440027702756215`, `G-CR8ETZEYRB` und `Stand: 6. Juni 2026`.
- Cookie-Richtlinie lieferte HTTP 200 und enthielt `Google AdSense`, `pub-6440027702756215`, `cmplz_` und `Stand: 6. Juni 2026`.

### Reihenfolge

1. `sauerlandaktuell.de` – zuerst testen
2. `darkandalternativeworld.de` – danach, aber vorher Datenschutz/Cookie-Texte aktualisieren
3. `afd-im-netz.de` – nur später und vorsichtig wegen politisch sensibler Inhalte

### Gemeinsame technische Befunde

- `ads.txt` fehlt aktuell auf allen drei Domains (`/ads.txt` liefert 404).
- Impressum, Datenschutz und Cookie-Seiten sind grundsätzlich vorhanden.
- GA4 und Matomo sind laut Projektstand consent-gated eingebunden.
- Search Console ist dokumentiert vorhanden.
- Die Datenschutz-/Cookie-Texte sind teilweise nicht synchron mit dem tatsächlichen aktuellen Trackingstand.
- AdSense/Google-Werbung wird in den Rechtstexten noch nicht konkret genug erwähnt.
- Publisher-ID ist nicht bekannt.

## Vor jeder AdSense-Anmeldung erledigen

### 1. AdSense-Konto klären

Manuell oder per API prüfen:

- Gibt es ein aktives AdSense-Konto?
- Ist der Account vollständig eingerichtet?
- Gibt es Zahlungsprofil, Identität, Steuerangaben?
- Gibt es eine Publisher-ID (`pub-...`)?
- Sind die Zielseiten schon hinzugefügt?
- Gibt es Ablehnungen oder Policy-Hinweise?

Ohne Publisher-ID kann `ads.txt` nicht korrekt vorbereitet werden.

### 2. Datenschutz und Cookies aktualisieren

Für AdSense müssen die Rechtstexte ergänzt werden um:

- Google AdSense / Google Werbung
- Anzeigenpersonalisierung bzw. nicht personalisierte Anzeigen
- mögliche Cookies/Identifier von Google und Werbepartnern
- Rechtsgrundlage Einwilligung für Marketing/Advertising
- Drittlandtransfer-Hinweis
- Widerruf über Cookie-Einstellungen
- ggf. Google Consent Mode / CMP-Hinweis

### 3. Consent-Setup prüfen

Vor dem Einbau von AdSense muss klar sein:

- Werden AdSense-Scripts erst nach Marketing-/Werbeeinwilligung geladen?
- Wird bei Ablehnung wirklich nichts von AdSense geladen?
- Funktionieren „Ablehnen“, „Akzeptieren“ und späterer Widerruf sauber?
- Falls Auto Ads genutzt wird: wird das Script ebenfalls consent-gated eingebunden?

### 4. `ads.txt` vorbereiten

Sobald die Publisher-ID bekannt ist, muss pro Domain eine Datei erreichbar sein:

```txt
google.com, pub-XXXXXXXXXXXXXXXX, DIRECT, f08c47fec0942fa0
```

Aktuell fehlt `/ads.txt` auf allen drei Seiten.

### 5. Content-Qualität prüfen

Google prüft nicht nur Technik, sondern auch Inhalt:

- ausreichend eigener Inhalt
- klare Navigation
- keine leeren/dünnen Seiten im Vordergrund
- keine reinen Kopien ohne Mehrwert
- Bildrechte klären
- Quellen sauber nennen
- keine irreführenden Überschriften
- keine Policy-riskanten Inhalte prominent monetarisieren

## Seite 1: `sauerlandaktuell.de`

### Einschätzung

Beste Startseite für AdSense.

Gründe:

- regionales Nachrichten-/Service-Thema
- breite Werbetauglichkeit
- viele lokale Inhalte und Tags
- Veranstaltungen, Ärzte-/Apothekennotdienste und lokale Suchintentionen
- politisch weniger riskant als `afd-im-netz.de`

### Öffentliche Checks

- `/ads.txt`: fehlt aktuell (`404`).
- Datenschutzseite vorhanden: `https://sauerlandaktuell.de/datenschutzerklaerung/`
- Cookie-Richtlinie vorhanden: `https://sauerlandaktuell.de/cookie-richtlinie/`
- Impressum ist in der Navigation erreichbar.
- Consent-Banner mit Kategorien einschließlich Marketing ist vorhanden.

### Auffälligkeiten in Rechtstexten

- Datenschutzerklärung erwähnt „Google Site Kit / Google-Tags“, aber nicht konkret Google AdSense.
- Cookie-Richtlinie erwähnt Analyse/Marketing und Google-Tags, aber nicht konkret AdSense/Werbung.
- Cookie-Richtlinie nennt `afd_cookie_consent`, obwohl Sauerland laut Tracking-Doku mit Complianz und `cmplz_statistics=allow` arbeitet. Das sollte geprüft/korrigiert werden.
- GA4-Details sollten mit dem tatsächlichen Setup abgeglichen werden: dokumentiert ist `G-CR8ETZEYRB`.

### Vor AdSense-Freigabe erledigen

- [ ] AdSense-Konto/Publisher-ID klären.
- [ ] `ads.txt` mit Publisher-ID einrichten.
- [ ] Datenschutzerklärung um Google AdSense/Google Werbung ergänzen.
- [ ] Cookie-Richtlinie um Werbe-Cookies/AdSense ergänzen.
- [ ] Cookie-Richtlinie auf korrektes Sauerland-Consent-System prüfen (`cmplz_...` statt `afd_cookie_consent`, falls zutreffend).
- [ ] Prüfen, ob Marketing-Kategorie im Consent-Banner für AdSense geeignet ist.
- [ ] Startseite und Top-Artikel auf Eigenanteil, Quellen und Bildrechte prüfen.
- [ ] AdSense zuerst nur auf dieser Domain anmelden/testen.

### Empfohlener Einbau

Erst nach Freigabe:

- Auto Ads nur moderat testen, oder
- 1–2 manuelle Anzeigenplätze:
  - unter erstem Absatz in Artikeln
  - unter Artikeln / zwischen Beitragslisten
- kein aggressives Vollstopfen der Seite.

## Seite 2: `darkandalternativeworld.de`

### Einschätzung

Gute Nische, aber vor AdSense erst Rechtstexte und Content-/Bildrechte prüfen.

Gründe für Potenzial:

- klare Szene-/Musik-Nische
- Bands A-Z, Festivals, Konzerte, Community-Ansatz
- für Anzeigen grundsätzlich geeignet, aber vermutlich niedrigere Reichweite

Risiken:

- viele importierte/kuratiert wirkende Inhalte
- mögliche fremde Bilder/Pressebilder
- Datenschutz-/Cookie-Texte wirken nicht mehr synchron mit GA4-Einbindung

### Öffentliche Checks

- `/ads.txt`: fehlt aktuell (`404`).
- Datenschutzseite vorhanden: `https://darkandalternativeworld.de/rechtliches/datenschutzerklaerung/`
- Cookie-Richtlinie vorhanden: `https://darkandalternativeworld.de/rechtliches/cookie-richtlinie/`
- Impressum ist in der Navigation erreichbar.
- Cookie-Banner vorhanden mit „Nur notwendige Cookies“ und „Alle Cookies akzeptieren“.

### Auffälligkeiten in Rechtstexten

- Datenschutzerklärung sagt: „Derzeit ist keine externe Webanalyse wie Google Analytics aktiv eingebunden.“
- Cookie-Richtlinie sagt ebenfalls: „Derzeit sind keine Google-Analytics- oder vergleichbaren Statistik-Cookies aktiv eingebunden.“
- Laut Projektstand ist GA4 aber eingebunden: `G-END9E0M1V7`, consent-gated.
- AdSense wird noch nicht erwähnt.

### Vor AdSense-Freigabe erledigen

- [ ] Datenschutzseite auf tatsächliche GA4-/Matomo-Einbindung aktualisieren.
- [ ] Cookie-Richtlinie auf tatsächliche GA4-/Matomo-Einbindung aktualisieren.
- [ ] AdSense/Google Werbung ergänzen.
- [ ] Publisher-ID klären.
- [ ] `ads.txt` einrichten.
- [ ] Bildrechte/Pressebilder prüfen.
- [ ] Eigene Bandporträts/Festivalübersichten stärker hervorheben.
- [ ] Erst nach Sauerland-Erfahrung anmelden/testen.

### Empfohlener Einbau

- zurückhaltend, szenetauglich
- eher auf Bandporträts, Festival-/Eventseiten und unter Artikeln
- keine störenden Anzeigen im Hero-/Stream-Bereich

## Seite 3: `afd-im-netz.de`

### Einschätzung

AdSense ist hier am riskantesten.

Nicht zwingend unmöglich, aber wegen politisch sensibler Inhalte und Begriffsfelder ist mit Ablehnung, eingeschränkter Auslieferung oder niedrigem RPM zu rechnen.

Risiken:

- AfD, Rechtsextremismus, Verfassungsschutz, Remigration, Antifa, Russland/Trump/Ukraine
- Brand-Safety-Einschränkungen
- Anzeigenkunden meiden politisch kontroverse Kontexte
- einzelne Artikel können nicht monetarisierbar sein

### Öffentliche Checks

- `/ads.txt`: fehlt aktuell (`404`).
- Datenschutzseite vorhanden: `https://afd-im-netz.de/rechtliches/datenschutzerklaerung/`
- Cookie-Richtlinie vorhanden: `https://afd-im-netz.de/rechtliches/cookie-richtlinie/`
- Impressum ist in der Navigation erreichbar.
- Cookie-Banner vorhanden.

### Auffälligkeiten in Rechtstexten

- Datenschutzerklärung nennt noch Measurement-ID `G-Q479JDDKHN`.
- Cookie-Richtlinie nennt `_ga_Q479JDDKHN`.
- Laut aktueller Projekt-Doku ist die neue Live-GA4-ID aber `G-8QNYTLS937`.
- AdSense/Google Werbung wird noch nicht erwähnt.

### Vor AdSense-Freigabe erledigen

- [ ] GA4-ID in Datenschutz/Cookie-Richtlinie aktualisieren.
- [ ] AdSense/Google Werbung ergänzen, falls die Seite überhaupt monetarisiert werden soll.
- [ ] Publisher-ID klären.
- [ ] `ads.txt` einrichten.
- [ ] Redaktionelle Linie prominent erklären: Dokumentation, Analyse, Faktencheck.
- [ ] Anzeigen nicht auf besonders sensiblen Artikeln starten.
- [ ] Erst nach Erfahrungen mit den anderen Seiten testen.

### Empfohlener Einbau

Wenn überhaupt:

- sehr zurückhaltend
- eher auf neutraleren Übersichts- und Faktencheckseiten
- nicht direkt neben Gewalt-/Extremismus-/Skandalbildern oder stark polarisierenden Begriffen
- keine aggressive Auto-Ads-Konfiguration

## Konkreter nächster Arbeitsplan

### Phase 1: AdSense-Konto klären

Nutzer prüft oder liefert:

- AdSense-Konto vorhanden: ja/nein
- Publisher-ID: `pub-...`
- Site-Status für `sauerlandaktuell.de`: nicht hinzugefügt / in Prüfung / freigegeben / abgelehnt
- ggf. Ablehnungsgrund oder Policy-Hinweis

### Phase 2: Sauerland vorbereiten

- Datenschutz und Cookie-Richtlinie aktualisieren.
- `ads.txt` vorbereiten, sobald Publisher-ID bekannt ist.
- Consent-Logik für AdSense planen.
- Erst `sauerlandaktuell.de` bei AdSense einreichen bzw. prüfen lassen.

### Phase 3: Einbau nach Freigabe

- AdSense-Code consent-gated einbauen.
- `ads.txt` live prüfen.
- Test ohne Consent / nach Ablehnen / nach Akzeptieren.
- 7–14 Tage messen.

### Phase 4: Auswertung

- RPM, Seitenaufrufe und Einnahmen prüfen.
- Erst dann `darkandalternativeworld.de` nachziehen.
- `afd-im-netz.de` separat bewerten.

## Offene Fragen

1. Existiert bereits ein AdSense-Konto?
2. Wie lautet die Publisher-ID?
3. Soll zuerst nur `sauerlandaktuell.de` eingereicht werden?
4. Soll AdSense strikt nur nach Marketing-Einwilligung geladen werden?
5. Sollen Datenschutz-/Cookie-Texte direkt in den WordPress-Seiten angepasst werden?

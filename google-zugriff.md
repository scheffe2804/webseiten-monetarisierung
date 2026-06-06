# Google-Zugriff und Tracking-Bestand

Quelle dieser Zusammenfassung:

- `/home/chris/web/diverses/webseiteenstatistiken.md`
- `/home/chris/web/diverses/google-analytics-fresh-setup-20260524.json`
- `/home/chris/web/diverses/google-inventory-20260524.json`

Stand der übernommenen Informationen: `2026-05-24`.

## Wichtig für diese Monetarisierungsplanung

Für die drei Zielseiten existiert bereits eine sauber dokumentierte Google-/Tracking-Vorarbeit. Die Informationen hier sollen im Projekt dauerhaft verfügbar sein, damit AdSense-, Analytics- und Search-Console-Fragen nicht jedes Mal neu aus `/home/chris/web/diverses` zusammengesucht werden müssen.

## Google Analytics 4

Es wurde ein gemeinsames Google-Analytics-Konto für die drei Hauptseiten neu angelegt.

- Konto: `accounts/395569161`
- Anzeigename: `Webseiten Hauptseiten`
- Region: `DE`

### sauerlandaktuell.de

- GA4 Property: `properties/538764066`
- Webstream: `properties/538764066/dataStreams/14935975825`
- Measurement ID: `G-CR8ETZEYRB`
- Default URI: `https://sauerlandaktuell.de`
- Zeitzone/Währung: `Europe/Berlin` / `EUR`

### darkandalternativeworld.de

- GA4 Property: `properties/538777884`
- Webstream: `properties/538777884/dataStreams/14935969451`
- Measurement ID: `G-END9E0M1V7`
- Default URI: `https://darkandalternativeworld.de`
- Zeitzone/Währung: `Europe/Berlin` / `EUR`

### afd-im-netz.de

- GA4 Property: `properties/538679471`
- Webstream: `properties/538679471/dataStreams/14935925373`
- Measurement ID: `G-8QNYTLS937`
- Default URI: `https://afd-im-netz.de`
- Zeitzone/Währung: `Europe/Berlin` / `EUR`

## GA4-Einbindung auf den Live-Seiten

GA4 ist laut Protokoll auf allen drei Live-Seiten consent-konform eingebunden.

### sauerlandaktuell.de

- Datei: `/home/chris/web/sauerlandaktuell.de/wp-content/mu-plugins/matomo-tracking.php`
- GA4 Measurement ID: `G-CR8ETZEYRB`
- Matomo Site-ID: `1`
- Tracking wird nur ausgeliefert bei Host `sauerlandaktuell.de` / `www.sauerlandaktuell.de` und Cookie `cmplz_statistics=allow`.

### darkandalternativeworld.de

- Datei: `/home/chris/web/darkandalternativeworld.de/wp-content/mu-plugins/matomo-tracking.php`
- GA4 Measurement ID: `G-END9E0M1V7`
- Matomo Site-ID: `4`
- Tracking wird nur ausgeliefert bei Host `darkandalternativeworld.de` / `www.darkandalternativeworld.de` und Cookie `afd_cookie_consent=accept`.

### afd-im-netz.de

- Datei: `/home/chris/web/afd-im-netz.de/wp-content/mu-plugins/afd-site-optimizer.php`
- GA4 Measurement ID: `G-8QNYTLS937`
- Matomo Site-ID: `2`
- Tracking wird nur ausgeliefert bei Host `afd-im-netz.de` / `www.afd-im-netz.de` und Cookie `afd_cookie_consent=accept`.

## Consent-Fix

Am `2026-05-24` wurde ein Consent-Bug korrigiert:

- Dark und AFD hatten vorher einen Event-Bypass, bei dem `reject`/„Nur notwendige Cookies“ theoretisch trotzdem Analytics laden konnte.
- Das wurde behoben: `afd_cookie_consent_updated` lädt Analytics nur noch bei `event.detail.value === 'accept'`.
- Sauerland wurde ebenfalls robuster gemacht: kein riskantes Laden bei leerem/unklarem Complianz-Event-Detail.
- Danach wurden Cloudflare-Caches für alle drei Seiten gepurgt.

Finale öffentliche Live-Prüfung laut Protokoll:

- Ohne Consent: keine statischen externen `gtag.js`- oder `matomo.js`-Script-Tags.
- Nach Accept: Matomo und GA4 können geladen werden.
- `loadAnalytics(true)` wurde bei Dark/AFD entfernt.

## Google Search Console

Search Console ist für die drei Zielseiten vorhanden und liefert Suchdaten.

### sauerlandaktuell.de

- Property: `sc-domain:sauerlandaktuell.de`
- Berechtigung: `siteOwner`
- Sitemap: `https://sauerlandaktuell.de/sitemap_index.xml`
- Sitemap-Status laut Inventar: `1` Warnung, `0` Fehler
- Letzter Download laut Inventar: `2026-03-29T12:21:00.566Z`
- Letzte 28 Tage laut Inventar vom 2026-05-24:
  - `81` Klicks
  - `2397` Impressionen

### darkandalternativeworld.de

- Property: `sc-domain:darkandalternativeworld.de`
- Berechtigung: `siteOwner`
- Sitemap: `https://darkandalternativeworld.de/sitemap_index.xml`
- Sitemap-Status laut Inventar: `1` Warnung, `0` Fehler
- Letzter Download laut Inventar: `2026-05-21T22:37:14.708Z`
- Letzte 28 Tage laut Inventar vom 2026-05-24:
  - `88` Klicks
  - `3121` Impressionen

### afd-im-netz.de

- Property: `https://afd-im-netz.de/`
- Berechtigung: `siteOwner`
- Sitemap: `https://afd-im-netz.de/sitemap_index.xml`
- Sitemap-Status laut Inventar: `0` Warnungen, `0` Fehler
- Letzter Download laut Inventar: `2026-05-24T03:35:09.266Z`
- Letzte 28 Tage laut Inventar vom 2026-05-24:
  - `2` Klicks
  - `313` Impressionen

## Matomo

Matomo läuft unter:

- `https://stats.sauerlandaktuell.de/`

Sites:

- Site `1`: `Sauerlandaktuell`
- Site `2`: `Afd im Netz`
- Site `3`: `Suche`
- Site `4`: `darkandalternativeworld.de`

Wichtige Befunde:

- Matomo ist erreichbar und nimmt Tracking-Requests an.
- Tracking ist auf allen drei Seiten consent-gated.
- Ohne Consent wird bewusst kein Matomo-Code geladen.
- Ein Matomo-Archivierungs-Cron wurde am `2026-05-24` eingerichtet.
- Cron-Marker: `# matomo-core-archive`
- Intervall: alle 15 Minuten.
- Lock: `/tmp/matomo-core-archive.lock`
- Logdatei: `/home/chris/web/stats.sauerlandaktuell.de/logs/core-archive-cron.log`

## Relevanz für AdSense

Für AdSense sind diese Punkte wichtig:

1. Die Seiten haben bereits Impressum/Datenschutz/Cookie-Strukturen.
2. Tracking ist consent-gated und damit grundsätzlich passend für EU-/DE-Anforderungen.
3. Search Console liefert organische Suchdaten für alle drei Zielseiten.
4. GA4 ist bereits eingerichtet, aber wegen Consent und wenig echtem Traffic können die Zahlen niedrig sein.
5. Für AdSense muss zusätzlich geprüft/eingerichtet werden:
   - AdSense-Konto und Site-Freigabe
   - Publisher-ID
   - `ads.txt`
   - AdSense/Google-Werbung in Datenschutz und Cookie-Richtlinie
   - Consent Mode / CMP-Kompatibilität für Werbe-Cookies
   - keine Anzeigenauslieferung ohne passende Einwilligung, soweit erforderlich

## Aktueller Tool-Hinweis

In der aktuellen Sitzung ist kein direkter Google-MCP/API-Server sichtbar. Die vorhandenen Dateien zeigen aber, dass frühere Sitzungen Google Analytics Admin/Data API und Search Console auswerten konnten. Falls erneut direkte Google-Abfragen gewünscht sind, muss der entsprechende API-/MCP-Zugriff in der aktuellen Umgebung verfügbar sein oder über bestehende lokale Skripte/Credentials geprüft werden.

Keine Secrets, Tokens oder OAuth-Dateien in dieses Projekt übernehmen.

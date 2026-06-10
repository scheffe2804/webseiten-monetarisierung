# Monetarisierungs-Protokoll

## 2026-06-06 – Start
Projekt neu angelegt.
Ausgangsfrage: Mit drei Webseiten Geld verdienen; erste Idee: Werbung. Gewünscht sind Vorschläge.

Konkret genannte Webseiten/Projektordner unter `/home/chris/web`:
- `sauerlandaktuell.de`
- `darkandalternativeworld.de`
- `afd-im-netz.de`

## Erste Vorschläge
- Die drei Webseiten zuerst nach Thema, Besucherzahlen, Zielgruppe und rechtlicher Eignung bewerten.
- Klassische Werbung kann sinnvoll sein, lohnt sich aber meistens erst ab relevanter Reichweite.
- Zusätzlich oder alternativ sollten Affiliate-Links, eigene Dienstleistungen, Lead-Generierung, Sponsoring und digitale Produkte geprüft werden.

## Erste Einschätzung zu den drei Seiten

### sauerlandaktuell.de
Vermutlich am besten geeignet für regionale Monetarisierung:
- lokale Anzeigenkunden
- Sponsoring durch lokale Firmen
- Branchen-/Veranstaltungs-/Vereins-Einträge
- Affiliate für Freizeit, Tourismus, regionale Produkte, Tickets, Hotels, Ausflüge
- später Newsletter-Sponsoring

### darkandalternativeworld.de
Geeignet für Nischen-/Community-Monetarisierung:
- Affiliate für Musik, Kleidung, Schmuck, Gothic-/Alternative-Shops, Konzerttickets
- gesponserte Beiträge von Shops, Bands, Labels, Events
- direkte Banner statt nur generische Werbung
- Newsletter oder Eventkalender als Community-Kanal

### afd-im-netz.de
Politisch sensibles Umfeld. Klassische Werbenetzwerke können hier ablehnen, einschränken oder sehr schlecht vergüten.

Sinnvoller wären eher:
- keine aggressive klassische Werbung als Hauptmodell
- rechtlich saubere Spenden-/Unterstützer-Hinweise, falls inhaltlich und rechtlich passend
- Newsletter/Recherche-Unterstützung
- ggf. sachbezogene Buch-/Dokument-/Quellen-Affiliates, wenn neutral und transparent
- besondere Vorsicht bei Datenschutz, Parteienrecht, Impressum, Kennzeichnung und Plattformregeln

Wichtig: Keine verdeckte politische Einflussnahme, keine irreführende Werbung, keine nicht gekennzeichneten Sponsoren.

## 2026-06-08 – AdSense-ads.txt fuer sauerlandaktuell.de
AdSense meldete fuer `sauerlandaktuell.de`:
- `Mögliche Einnahmeverluste`
- `Ads.txt wurde nicht gefunden`
- zuletzt gecrawlt: `Nicht zutreffend`
- verlangte Zeile: `google.com, pub-6440027702756215, DIRECT, f08c47fec0942fa0`

Pruefung/Ergebnis:
- Host-Kontext bestaetigt: OpenCode-Sitzung auf `m11h`; Runtime fuer `sauerlandaktuell.de` laut Projekt-AGENTS auf `m00h`.
- Oeffentlicher Check von `https://sauerlandaktuell.de/ads.txt` lieferte bereits HTTP 200 mit der korrekten Google-Zeile ueber das MU-Plugin.
- Zur robusteren Auslieferung wurde auf `m00h` im laufenden Live-WordPress-Container zusaetzlich eine echte Datei `/var/www/html/ads.txt` angelegt.
- Ein erster Shell-Escaping-Fehler erzeugte kurzfristig ein falsches trailing `n`; wurde sofort korrigiert.
- Verifikation danach:
  - Datei im Container: `59` Byte, exakt eine Zeile mit abschliessendem Newline.
  - Oeffentlich: `https://sauerlandaktuell.de/ads.txt?opencode_check=202606080802` lieferte HTTP 200, `content-type: text/plain`, `content-length: 59` und exakt:
    `google.com, pub-6440027702756215, DIRECT, f08c47fec0942fa0`

Hinweis: AdSense kann einige Tage brauchen, bis die Datei erneut gecrawlt und die Warnung entfernt wird.

## 2026-06-10 – AdSense-Status sauerlandaktuell.de
AdSense-Warnung für `sauerlandaktuell.de` wurde als erledigt/akzeptiert gemeldet. Monetarisierung für diese Seite ist damit im ersten Schritt aktiv.

## 2026-06-10 – AdSense-Staging-Umbau sauerlandaktuell.de

Repo-Trennung geklärt:

- `/home/chris/web/webseiten-monetarisierung` bleibt Planungs-/Doku-Repo.
- `/home/chris/web/staging.sauerlandaktuell.de` ist die aktive Entwicklungsumgebung für technische Änderungen an `sauerlandaktuell.de`.
- `/home/chris/web/sauerlandaktuell.de` ist Live und soll laut Projektregeln nicht direkt geändert werden.

Technischer Stand im Staging-Repo:

- Vorhandenes MU-Plugin `wp-content/mu-plugins/adsense-setup.php` wurde nicht dupliziert, sondern angepasst.
- `/ads.txt`-Auslieferung bleibt unverändert mit `google.com, pub-6440027702756215, DIRECT, f08c47fec0942fa0`.
- Das AdSense-Script wird nicht mehr direkt statisch im Head geladen.
- Stattdessen lädt ein Consent-Gated-Loader `pagead2.googlesyndication.com/pagead/js/adsbygoogle.js` erst nach Complianz-Marketing-Consent (`cmplz_marketing=allow` bzw. Complianz-Marketing-Events).
- PHP-Syntaxprüfung im Staging-Repo erfolgreich: `php -l wp-content/mu-plugins/adsense-setup.php`.
- Runtime-HTTP-Check über `m00h` zeigte den neuen Loader noch nicht; wahrscheinlich wegen Host-Begrenzung auf Live-Domain und/oder noch fehlender Synchronisation des m11h-Staging-Checkouts zur laufenden m00h-Runtime.

Noch offen vor Live-Rollout:

- Staging-/Runtime-Abgleich herstellen und Frontend lokal über Runtime-Host `m00h` prüfen.
- Test ohne Marketing-Consent: kein AdSense-Netzwerkrequest.
- Test nach Marketing-Consent: AdSense-Script wird geladen.
- Danach Entscheidung über Commit/Push und späteren Release nach Live.

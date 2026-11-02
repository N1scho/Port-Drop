# PORT DROP

**Browserbasiertes Lernspiel für Netzwerkports zur Vorbereitung auf die schriftliche IHK-Abschlussprüfung Teil 2 (Fachinformatiker Systemintegration).**

PORT DROP verbindet die Zuordnung von Portnummern und Netzwerkdiensten mit einem Reaktionsspiel. Portnummern oder Dienstnamen fallen von oben durch das Spielfeld. Die passende Eingabe entfernt das Ziel, bevor es die untere Begrenzung erreicht.

## Spiel starten

1. `PORT_DROP_v5_kompakte_legende.html` herunterladen.
2. Datei mit einem aktuellen Browser öffnen.
3. Lernkategorie, Fragetyp und Fallgeschwindigkeit einstellen.
4. Auf **Spiel starten** klicken.
5. Passende Portnummer oder passenden Dienst eingeben und mit **Enter** oder **FEUER** bestätigen.

Das Spiel ist eine eigenständige HTML-Datei. Es benötigt keine Installation, keinen Webserver, kein Benutzerkonto und keine Internetverbindung.

## Funktionsumfang

| Funktion | Beschreibung |
| --- | --- |
| Lernkategorien | Basics (12 Ports), Erweitert (22 Ports), Alle (30 Ports) |
| Fragen | Gemischt, Portnummern fallen, Dienste fallen |
| Geschwindigkeit | Stufenloser Schieberegler von 0,3× bis 2,0× in 0,1er-Schritten; Startwert 0,7× |
| Treffer | Ziel verschwindet mit Strahl-, Partikel- und Schockwellen-Effekt |
| Spielstand | Punkte, Leben, Level und Trefferkombination (Combo) |
| Legende | Ein- und ausblendbar, zweispaltig, nur Ports der gewählten Lernkategorie |
| Farbcodierung | Gemeinsame Dienstkategorien im Spielfeld und in der Legende |
| Pause | Per Schaltfläche oder **Esc** im Eingabefeld |

Die Geschwindigkeit lässt sich während des Spiels ändern und wirkt auch auf bereits fallende Ziele. Die Auswahl der Lernkategorie ist ebenfalls während des Spiels möglich. Ziele, welche nicht zur neuen Kategorie gehören, werden entfernt.

## Spielregeln

- Zeigt ein Ziel eine **Portnummer**, muss der zugehörige **Dienstname** eingegeben werden.
- Zeigt ein Ziel einen **Dienstnamen**, muss die entsprechende **Portnummer** eingegeben werden.
- Ein richtiger Treffer entfernt das passende Ziel und erhöht die Punktzahl.
- Mehrere Treffer hintereinander erhöhen den Combo-Bonus.
- Erreicht ein Ziel den unteren Spielfeldrand, geht ein Leben verloren.
- Nach fünf verpassten Zielen endet die Runde.
- Nicht passende Eingaben setzen die Combo zurück, kosten jedoch kein Leben.

**Beispiele**

| Fallendes Ziel | Eingabe |
| --- | --- |
| `443` | `HTTPS` |
| `SSH` | `22` |
| `995` | `POP3S` |
| `3389` | `RDP` |

Dienstnamen werden ohne Beachtung der Groß- und Kleinschreibung geprüft. Für bestimmte Dienste sind zusätzliche Schreibweisen hinterlegt.

## Lernkategorien

Die drei Auswahlstufen sind aufeinander aufgebaut.

### Basics – 12 Ports

| Port | Dienst |
| --- | --- |
| 21 | FTP-Steuerung |
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 67 | DHCP-Server |
| 68 | DHCP-Client |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 445 | SMB |
| 3389 | RDP |

### Erweitert – 22 Ports

Enthält alle zwölf Basics-Ports sowie folgende elf Ports:

| Port | Dienst |
| --- | --- |
| 20 | FTP-Daten |
| 23 | Telnet |
| 69 | TFTP |
| 123 | NTP |
| 161 | SNMP |
| 162 | SNMP-Trap |
| 389 | LDAP |
| 587 | SMTP Submission |
| 636 | LDAPS |
| 993 | IMAPS |
| 995 | POP3S |

**Hinweis:** Die aktuelle Implementierung enthält in der erweiterten Auswahl tatsächlich **23 Ports** (12 Basics + 11 zusätzliche). Die Bezeichnung „Erweitert (22)“ im Auswahlmenü ist daher ein offener Korrekturpunkt. Der tatsächliche Umfang wird im Spiel über den aktiven Zähler angezeigt.

### Alle – 30 Ports

Enthält alle Ports der erweiterten Auswahl und die übrigen sieben:

| Port | Dienst |
| --- | --- |
| 465 | SMTPS |
| 514 | Syslog |
| 1433 | MS SQL Server |
| 3306 | MySQL |
| 5432 | PostgreSQL |
| 5900 | VNC |
| 8080 | HTTP-Alternative |

**Hinweis:** `3389` (RDP) gehört bereits zu Basics und wird nicht erneut aufgeführt. Die vollständige Menge aller 30 Ports ist im JavaScript-Array `entries` definiert.

## Farbcodierung

| Kategorie | Farbe | Beispiele |
| --- | --- | --- |
| Web | Blau | HTTP, HTTPS |
| Mail | Violett | SMTP, POP3, IMAP |
| Remote | Grün | SSH, RDP, VNC |
| Netzwerk | Türkis | DNS, DHCP, NTP |
| Verzeichnis | Gelb | LDAP, LDAPS |
| Datei | Koralle | FTP, TFTP, SMB |
| Datenbank | Pink | MySQL, PostgreSQL, MS SQL Server |
| Sonstiges | Grau | Reservekategorie |

Die Farben dienen der visuellen Zuordnung. Sie entsprechen keiner offiziellen IHK-Klassifikation.

## Technischer Aufbau

Das Projekt ist derzeit eine einzelne HTML-Datei mit eingebettetem CSS und JavaScript.

```text
port-drop/
├── README.md
└── index.html
```

Für diese Repository-Struktur wird die aktuelle Spieldatei in `index.html` umbenannt. Eine Aufteilung in separate CSS- und JavaScript-Dateien ist für den Betrieb nicht erforderlich.

Wichtige Bereiche im Quellcode:

| Bestandteil | Funktion |
| --- | --- |
| `entries` | Portnummern, Dienstnamen, Eingabevarianten und Farbkategorien |
| `sets` | Zuordnung der Ports zu Basics, Erweitert und Alle |
| `renderLegend()` | Baut die kompakte zweispaltige Legende auf |
| `spawnItem()` | Erzeugt fallende Ziele |
| `tick()` | Aktualisiert die Bewegung und erkennt verpasste Ziele |
| `fire()` | Prüft die Eingabe und verarbeitet Treffer |
| `createExplosion()` / `createBeam()` | Visuelle Treffer-Effekte |
| `updatePortSet()` | Aktualisiert aktive Ports und Legende |

### Ports hinzufügen oder ändern

Neue Ports werden im Array `entries` eingetragen. Ein Datensatz enthält Portnummer, Anzeigenamen, erlaubte Eingaben, CSS-Farbklasse und Kategoriebezeichnung.

```javascript
['443', 'HTTPS', ['https'], 'web', 'Web']
```

Anschließend müssen die Mengen in `sets` angepasst werden. Die Anzahl der Einträge und die Auswahlbeschriftungen sollten übereinstimmen.

### Geschwindigkeit anpassen

Der Regler `#fallSpeed` nutzt die HTML-Attribute `min`, `max`, `step` und `value`. Die tatsächliche Bewegung wird in `tick()` mit dem eingestellten Faktor multipliziert.

```javascript
it.y += it.speed * Number($('fallSpeed').value) * dt;
```

## Veröffentlichung über GitLab Pages

Eine einfache Veröffentlichung ist über GitLab Pages möglich. Dafür sollte die Spieldatei `index.html` heißen.

**Repository:**

```text
README.md
index.html
.gitlab-ci.yml
```

**Beispiel für `.gitlab-ci.yml`:**

```yaml
create-pages:
  stage: deploy
  script:
    - mkdir -p public
    - cp index.html public/index.html
  pages: true
  artifacts:
    paths:
      - public
  rules:
    - if: '$CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH'
```

Nach dem Commit auf dem Standardbranch und erfolgreicher Pipeline kann die veröffentlichte Adresse unter **Deploy > Pages** im GitLab-Projekt aufgerufen werden. Die Verfügbarkeit und Konfiguration von GitLab Pages hängen von der jeweiligen GitLab-Instanz ab.

## Prüfen vor einem Release

- [ ] HTML-Datei öffnet sich lokal ohne Netzwerkverbindung.
- [ ] Alle drei Lernkategorien enthalten die vorgesehenen Ports.
- [ ] Die im Menü angegebene Anzahl entspricht den tatsächlich aktiven Ports.
- [ ] Die Legende zeigt nur die zur Kategorie gehörenden Einträge, ohne Lücken.
- [ ] Der Regler verändert die Fallgeschwindigkeit zwischen 0,3× und 2,0×.
- [ ] Portnummer und Dienstname funktionieren in beiden Fragerichtungen.
- [ ] Treffer- und Fehlschlaglogik funktionieren.
- [ ] Pause, Neustart und Game Over funktionieren.
- [ ] Die Oberfläche bleibt bei schmalen Browserfenstern bedienbar.
- [ ] GitLab-Pages-Pipeline läuft erfolgreich, sofern die Bereitstellung genutzt wird.

## Bekannte Einschränkungen

- Spielstände und Highscores werden nicht dauerhaft gespeichert.
- Es gibt keine Benutzerkonten und keine Online-Rangliste.
- Die Trefferbewertung basiert auf hinterlegten Antwortvarianten, nicht auf einer freien semantischen Auswertung.
- Das Spiel bildet nicht alle Kombinationen aus TCP/UDP, Protokollvarianten oder dynamischen Ports ab.
- Die Kennzeichnung „Erweitert (22)“ ist aktuell inkonsistent mit den 23 hinterlegten Ports dieser Lernstufe.

## Projektstatus

**Version:** v5 – kompakte Legende und drei Lernkategorien  
**Stand:** 02.11.2026  
**Technologie:** HTML5, CSS3, JavaScript ohne externe Bibliotheken  
**Zielgruppe:** Auszubildende und Umschüler im Bereich Fachinformatik Systemintegration, insbesondere zur AP2-Vorbereitung

Eine Lizenz wurde bislang nicht festgelegt. Ohne gesonderte Lizenzdatei wird keine allgemeine Nutzungserlaubnis für Dritte zugesichert.

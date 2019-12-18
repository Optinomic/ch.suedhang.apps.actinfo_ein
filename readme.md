## act-info :: Eintrittsbefragung

### Zusammenfassung
act-info (addiction, care and therapy information) ist ein einheitliches, gesamtschweizerisches Klientenmonitoringsystem für den Bereich der Suchthilfe.

Erfassung der vier Bereiche Behandlungsgrundlagen, soziodemografische Angaben, Substanzkonsum und Gesundheit (ausserhalb Substanzproblematik)

### Hintergrund
Das nationale Dokumentationssystem umfasst Angebote der ambulanten und stationären Behandlung von Problemen mit legalen und illegalen Substanzen sowie von nicht-substanzgebundener Abhängigkeit. act-info ist das Resultat der Harmonisierung der bestehenden Suchthilfestatistiken.

Die beteiligten Forschungsinstitute (Sucht Schweiz Lausanne, ISGF Zürich) sind für die Datenerhebung und die Auswertungen in den einzelnen Behandlungssektoren verantwortlich.

act-info wird durch das Bundesamt für Gesundheit BAG finanziert und koordiniert. Die Verantwortung für das Gesamtprojekt act-info liegt beim BAG.

#### Auswertung / Ergebnisse
- Substanzkonsum vor Eintritt (Substanz + Häufigkeit)
- Zusatzangaben vor Eintritt (Hauptproblemsubstanz)
- AUDIT-Summenwert (Verdacht auf Alkoholabhängigkeit)
- Fagerström-Summenwert (Ausprägung der Nikotinabhängigkeit)


## Admin

### SQL

Direktes Auslesen von `calculation_values` via SQL:

```SQL
SELECT 
 
patient_id as optinomic_pid,
time,
cast(calc.value as json)->0 as full_calc,
cast(calc.value as json)->0->'hash' as hash,
cast(calc.value as json)->0->'problemsubstanzen'->'substanzen'->0->'drinks'->'gramm_total' as gramm,
*

FROM calculation_values AS calc 
WHERE calc.module = 'ch.suedhang.apps.actinfo_ein.production' 
```

oder in Kombination mit `survey_response` kann wie folgt abgefragt werden.

```SQL
SELECT 
 
calc.value
 
FROM survey_response_view AS sr 
LEFT JOIN calculation_values AS calc
ON calc.patient_id = sr.patient_id AND calc.module = sr.module
WHERE calc.module = 'ch.suedhang.apps.isk' AND calculation = 'scores_calculation'
LIMIT 5
```

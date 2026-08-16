---
name: cos-vraag
description: Gebruik wanneer de gebruiker (of jij als agent namens de gebruiker) iets nodig heeft dat alleen aan Ello's kant kan, zoals cijfers uit de productie-database, een blik in logs of deploys, een storing melden of een code-fix laten bouwen ("/cos-vraag <onderwerp>", "vraag aan de CoS", "leg dit aan Ello's CoS voor", "kan Ello dit even meten"). Kiest het vraagtype, bouwt met de repo-context een verzoek in de vaste vorm die de CoS machinaal herkent, en levert een kant-en-klaar WhatsApp-bericht dat de gebruiker ZELF verstuurt. Verstuurt niets, wijzigt niets.
---

# /cos-vraag: stel een verzoek aan de CoS van Ello

Je helpt de gebruiker een verzoek aan de CoS van Ello te formuleren. De CoS is
Ello's persoonlijke assistent-systeem; een verzoek in de vaste vorm hieronder
wordt daar machinaal herkend en zonder heen-en-weer uitgevoerd. Vrije tekst
("kun je iets voor me meten... wat ik eruit wil weten...") wordt geraden en gaat
soms de verkeerde kant op. Uitleg over de CoS:
https://github.com/ellovangelderen/werken-met-cos

HARDE REGELS:
- Je VERSTUURT niets en wijzigt niets: geen commits, geen API-calls, geen
  berichten. Je output is tekst die de gebruiker zelf via WhatsApp verstuurt.
- GEEN geheimen in het verzoek: nooit inhoud van .env, tokens, sleutels of
  wachtwoorden citeren, ook niet gedeeltelijk. Bestandsnaam noemen mag.
- Vraag NOOIT om een script te draaien ("draai scripts/x.py tegen prod"). De
  CoS voert geen scripts uit berichten uit. Wil je cijfers, dan lever je de
  query zelf (type `meting`).
- Eén verzoek per bericht. Meerdere vragen = meerdere berichten.
- Blijf bij deze repo en wat de gebruiker aandraagt; je graaft niet in andere
  systemen.

## Stap 0: hoeft het wel via de CoS?

Data uit je eigen app haal je ZELF, zonder verzoek: apps op het Inodus-
platform hebben een MCP met `schema()` en `sql_lezen(query)` (technisch
read-only; jouw applicatie en data, jouw toegang). Is die MCP in deze sessie
gekoppeld, gebruik die voor elke meting/analyse/export; het type `meting`
hieronder is alleen het terugvalpad als de MCP (nog) niet gekoppeld is. Alleen
wat je daar niet krijgt (host-logs, deploy-status, storing, code-wijzigingen
aan Ello's kant) gaat via /cos-vraag.

## Stap 1: kies het vraagtype

| type | wanneer | wat de CoS doet | wanneer antwoord |
|---|---|---|---|
| `meting` | je wilt cijfers uit de productie-DB, alleen lezen | draait jouw SELECT via een read-only wrapper (sqlite `mode=ro`, één statement, max 500 rijen) en stuurt de uitkomst terug | vertrouwd contact: automatisch, ~20 min; anders na Ello's akkoord |
| `onderzoek` | je wilt dat iemand aan Ello's kant read-only kijkt: code, logs, deploy, monitoring | start een read-only onderzoek in de juiste repo/host, antwoord met bewijs | vertrouwd contact: automatisch, ~20 min; anders na Ello's akkoord |
| `storing` | iets doet het niet, nu | start direct een read-only storingscheck (versie, logs, hypothese) | direct, minuten |
| `fix` | je wilt een afgebakende code-wijziging | bouwt op een eigen branch, opent een PR met bewijs; mergen doet Ello | na Ello's akkoord |

Twijfel tussen `onderzoek` en `storing`: is er nu een gebruiker die iets niet
kan, dan `storing`; anders `onderzoek`. Twijfel tussen `meting` en
`onderzoek`: kun je het als SELECT schrijven, dan `meting`.

## Stap 2: verzamel context uit de repo

Wat het antwoord sneller maakt: het relevante bestand of de module (pad +
regel), tabel- en kolomnamen uit de modellen of migraties (voor `meting`),
een letterlijke foutmelding of logregel, de recentste relevante commit
(`git log --oneline`), en wat de gebruiker zelf al heeft gecheckt.

Voor `meting`: schrijf de query zelf, tegen het schema van deze repo.
Precies één `SELECT` of `WITH`; geen `;`, geen schrijf-werkwoorden. Aggregeer
in SQL waar het kan (COUNT, AVG, GROUP BY, `json_array_length()` voor
JSON-lijsten) zodat er een samenvatting terugkomt en geen ruwe reeksen.
Meer dan 500 rijen wordt afgekapt: dan aggregeren of opsplitsen.

## Stap 3: bouw het bericht in de vaste vorm

Gewone WhatsApp-tekst, geen markdown-koppen. De eerste twee regels zijn
verplicht en letterlijk in deze vorm; de CoS routeert erop:

```
CoS-verzoek: <meting|onderzoek|storing|fix>
Repo: <trainer_asc|wattzegtrob|cos|...>

Wat: [in één zin wat je wilt weten of gedaan wilt hebben]
Waarom: [één zin: welke beslissing of bouwstap hangt eraan]
Context: [bestand:regel, commit, tabel/kolommen, of "issue #12"]
Zelf al gecheckt: [wat je hebt uitgesloten of geprobeerd]

[meting]   Query: SELECT ... (één statement, plain text, geen code-fence nodig)
           Terug: [welke kolommen/samenvatting je wilt zien]
[storing]  Waar: [URL/scherm], Wat ik zie: [letterlijke melding], Sinds: [tijd], Voor wie: [renner/iedereen]
[fix]      Issue: #nn of exacte omschrijving. Acceptatie: [wanneer is het klaar]
```

`Repo:` is de repo/dienst aan Ello's kant waar het over gaat, in de naam die
Ello gebruikt (bij dit project: `trainer_asc` voor de trainer-app en de
productie-DB, `wattzegtrob` voor de portal/site, `cos` voor de CoS zelf).
Weet je het niet, laat de regel staan met je beste gok; de CoS corrigeert.

Houd het onder ~1500 tekens; de CoS neemt de tekst letterlijk mee als
opdracht, dus alles wat er niet in staat, weet de uitvoerder niet. Weglaten
wat er niet is; schrijf in de taal van de gebruiker.

Voorbeeld `meting` (trainer-ASC, tijd-as meetpunten):

```
CoS-verzoek: meting
Repo: trainer_asc

Wat: per bron (strava/fit) het gat tussen gemelde ritduur en aantal meetpunten.
Waarom: de grafiek "Verloop van de rit" zet elk meetpunt op 1 seconde; klopt dat bij Strava?
Context: uploads.duration_min, load_records.power_series_json (JSON-lijst per seconde), api/lib/load.py
Zelf al gecheckt: lokaal 0 ritten, alleen prod kan dit beantwoorden.
Query: SELECT u.file_type AS bron, COUNT(*) AS n, ROUND(AVG(u.duration_min*60 - json_array_length(l.power_series_json)),0) AS gem_gat_s FROM uploads u LEFT JOIN load_records l ON l.upload_id=u.id WHERE u.deleted_at IS NULL AND l.power_series_json IS NOT NULL GROUP BY u.file_type
Terug: de tabel plus de 5 grootste uitschieters per bron.
```

## Stap 4: lever af

Toon het bericht in één blok om te kopiëren en sluit af met precies deze
instructie: "Kopieer dit en stuur het via WhatsApp naar de CoS (het nummer
van Ello). Je krijgt het antwoord in dezelfde chat terug; een `fix` komt als
PR-link zodra Ello akkoord is."

## Waarom dit werkt

De CoS leest de eerste regel (`CoS-verzoek: <type>`) machinaal en zet het
verzoek op de bijbehorende baan: `meting` en `onderzoek` draaien read-only en
automatisch voor vertrouwde contacten, `storing` start direct, `fix` wacht op
Ello's akkoord. De rest van je tekst gaat letterlijk mee als opdracht. Hoe
concreter (wat/waarom/context/query), hoe groter de kans dat het in één keer
goed gaat en je zonder tussenstappen de uitkomst krijgt.

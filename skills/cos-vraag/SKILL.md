---
name: cos-vraag
description: Gebruik wanneer de gebruiker een vraag of verzoek heeft voor de CoS van Ello ("/cos-vraag <onderwerp>", "vraag aan de CoS", "leg dit aan Ello's CoS voor"). Bouwt met de context van deze repo een beslisklare vraag volgens het vaste sjabloon en levert een kant-en-klaar WhatsApp-bericht dat de gebruiker ZELF verstuurt. Verstuurt niets, wijzigt niets.
---

# /cos-vraag — stel een goede vraag aan de CoS van Ello

Je helpt de gebruiker een vraag aan de CoS van Ello te formuleren. De CoS is
Ello's persoonlijke AI-assistent; een concrete, complete vraag wordt daar
vrijwel altijd zonder heen-en-weer opgepakt, een vage vraag kost dagen.
Uitleg over de CoS: https://github.com/ellovangelderen/werken-met-cos

HARDE REGELS:
- Je VERSTUURT niets en wijzigt niets: geen commits, geen API-calls, geen
  berichten. Je output is tekst die de gebruiker zelf via WhatsApp verstuurt.
- GEEN geheimen in de vraag: nooit inhoud van .env, tokens, sleutels of
  wachtwoorden citeren — ook niet gedeeltelijk. Bestandsnaam noemen mag.
- Blijf bij deze repo en wat de gebruiker aandraagt; je gaat niet op andere
  systemen graven.

## Stappen

1. **Begrijp het onderwerp.** De gebruiker geeft het aan (`/cos-vraag de
   kilometers kloppen niet in week 28`). Onduidelijk? Stel maximaal twee
   korte vragen, niet meer.

2. **Verzamel context uit de repo** die het antwoord sneller maakt: het
   relevante bestand of de module (pad + regel waar mogelijk), een letterlijke
   foutmelding of logregel, de recentste relevante commit (`git log --oneline`),
   en wat er feitelijk te zien is (welke pagina, welk gedrag, sinds wanneer).

3. **Check wat er al bekend is.** Heeft de gebruiker zelf al iets geprobeerd
   of gezien? Staat er al een issue of recente fix over dit onderwerp? Eén
   regel is genoeg.

4. **Bouw het bericht** volgens dit sjabloon, als gewone WhatsApp-tekst
   (geen markdown-koppen, geen opsommingstekens-jungle, maximaal ~15 regels):

   ```
   Hi CoS, [wat je wilt weten of gedaan wilt hebben, in één zin].

   Waar: [app/dienst + plek, bv. "ledenportal, weekoverzicht week 28"]
   Wat ik zie: [feitelijke waarneming, letterlijke foutmelding als die er is]
   Context uit de repo: [bestand:regel, commit, of "script X bestaat al"]
   Zelf al gecheckt: [wat de gebruiker al heeft uitgesloten of geprobeerd]

   [Alleen bij een verzoek om iets te draaien of te wijzigen: waarom het
   veilig is, als dat uit de repo blijkt — idempotent, alleen lege velden,
   geen migratie.]
   ```

   Weglaten wat er niet is; een korte complete vraag verslaat een lange met
   lege regels. Schrijf in de taal van de gebruiker.

5. **Lever af**: toon het bericht in één blok om te kopiëren en sluit af met
   precies deze instructie: "Kopieer dit en stuur het via WhatsApp naar de
   CoS (het nummer van Ello). Je krijgt vanzelf antwoord; afgerond werk komt
   automatisch bij je terug."

## Waarom dit werkt

De CoS herkent werkverzoeken machinaal en zet ze om in taken die het systeem
zelf uitvoert. Hoe concreter je vraag (wat/waar/wat zag je/wat is al
gecheckt), hoe groter de kans dat dat in één keer goed gaat en je zonder
tussenstappen de uitkomst krijgt.

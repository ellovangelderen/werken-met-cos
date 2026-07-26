# LLM Wiki

**Van wie:** Andrej Karpathy beschreef het patroon in
[deze gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## Wat het is

Een manier om met een LLM een **blijvende, groeiende kennisbank** te bouwen in
plaats van elke sessie opnieuw documenten te doorzoeken. Drie lagen:

1. **Bronnen** — onveranderbaar ruw materiaal (artikelen, gesprekken, logs).
2. **De wiki** — markdown-pagina's die de LLM onderhoudt: samenvattingen,
   inzichten en kruisverwijzingen, cumulatief bijgewerkt per nieuwe bron.
3. **Een schema** — de vaste werkstromen: *ingest* (nieuwe bron verwerken),
   *query* (vragen beantwoorden tegen de wiki), *lint* (gezondheidscheck op
   verouderde claims en drift).

De kern: "the wiki is a persistent, compounding artifact." De mens cureert en
stelt vragen; de LLM doet het administratieve onderhoud waar mensen zich bij
vervelen, zoals kruisverwijzingen actueel houden.

## Wanneer je het gebruikt

Zodra je merkt dat je een LLM voor de derde keer hetzelfde laat uitzoeken, of
dat inzichten uit eerdere sessies verdampen. Onderzoeksprojecten,
klantdossiers, een leertraject: alles waar kennis hoort te stapelen.

## Hoe ik het gebruik

Mijn Chief of Staff is in de kern een LLM Wiki over mijn werkende leven:

- **Bronnen:** de berichten-, mail- en agenda-feeds die het systeem
  read-only meeleest.
- **Wiki:** dossiers per contact en per onderwerp die de assistent zelf
  bijhoudt — wie iemand is, wat er loopt, wat er is toegezegd. Elke
  interactie maakt het dossier rijker; niets hoeft twee keer uitgelegd.
- **Lint:** nachtelijke en wekelijkse opruim- en leerrondes die verouderde
  taken sluiten, dubbelingen samenvoegen en lessen terugschrijven in de
  werkinstructies (zie [de CoS-opzet](cos-opzet.md)).

Het verschil met een chatbot-met-geheugen: de wiki is een leesbaar artefact
dat ik zelf kan openslaan, corrigeren en vertrouwen, geen zwarte doos.

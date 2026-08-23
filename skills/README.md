# Skills om te kopiëren

Hulpmiddelen voor wie zelf met Claude Code werkt in een project dat met Ello
gedeeld is.

## /cos-vraag

Een skill die met de context van je eigen project een verzoek voor de CoS
bouwt in de vaste vorm die de CoS machinaal herkent: eerste regel
`CoS-verzoek: meting | onderzoek | storing | fix`, dan `Repo:`, dan
wat/waarom/context/wat je al checkte. `meting` = cijfers uit de
productie-DB (je levert zelf één SELECT, de CoS draait die read-only),
`onderzoek` = read-only kijken in code/logs/deploy, `storing` = iets doet
het nu niet, `fix` = een afgebakende code-wijziging als PR. Het resultaat
stuur je zelf via WhatsApp; het antwoord komt in dezelfde chat terug.

`meting` is bewust het terugvalpad: data uit je eigen app haal je sneller zelf
via de app-MCP (`schema()` en `sql_lezen()`), zoals stap 0 van de skill
uitlegt. Alleen wat daar niet in zit gaat via een verzoek.

**Installeren:** kopieer de map [`cos-vraag/`](cos-vraag/) naar
`.claude/skills/cos-vraag/` in je eigen repo. Daarna werkt
`/cos-vraag <onderwerp>` in Claude Code.

De skill verstuurt zelf niets en wijzigt niets; hij formuleert alleen.

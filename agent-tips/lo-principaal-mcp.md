# Het principaal-profiel als MCP-server

**Van wie:** het onderliggende protocol (MCP, Model Context Protocol) is in
november 2024 door Anthropic geïntroduceerd en open-sourced. De toepassing
hier — je persoonlijke "zo werk je met mij"-handleiding als MCP-server — is
mijn eigen bouwsel.

## Het probleem

Wie met meerdere AI-oppervlakken werkt (een CLI, een chat-app, geplande
achtergrond-agents) legt overal opnieuw uit wie hij is, hoe hij communiceert
en wat de regels zijn. Die uitleg loopt uit elkaar: het ene oppervlak kent de
afspraak van vorige week, het andere niet. Context-drift is de stille
kwaliteitskiller van persoonlijke AI.

## De oplossing

Eén canoniek **principaal-profiel**: identiteit, communicatiestijl, harde
regels, routines. Dat profiel leeft op één plek en wordt via een kleine
MCP-server (`get_profile`, `get_section`) aan élk oppervlak geserveerd. Elke
sessie — interactief of headless — begint met het laden van het profiel en
werkt daarna volgens dezelfde handleiding.

Wat het oplevert:

- **Eén bron van waarheid.** Een regel wijzigen is één wijziging, geen
  zoektocht langs zeven systeemprompts.
- **Consistentie over oppervlakken.** De assistent in de terminal en de
  geplande nachtrun kennen dezelfde afspraken.
- **Drift wordt meetbaar.** Een periodieke check vergelijkt alle plekken waar
  de context leeft met de canon en meldt afwijkingen, in plaats van dat je ze
  per ongeluk ontdekt.

## Hoe ik het gebruik

Mijn profiel (het "LO-profiel") wordt geladen bij de start van elke sessie op
elke machine en elk oppervlak, ook door geplande runs zonder mens erbij. Een
tweewekelijkse drift-check loopt alle oppervlakken na. Verandert er een
afspraak op principaal-niveau, dan is dat één edit in de canon.

## Zelf bouwen

Een uitgeklede, geanonimiseerde template-versie van mijn server komt als
publieke repo beschikbaar; de link verschijnt hier zodra hij er is. Tot die
tijd: het patroon is klein genoeg om na te bouwen — een handvol
markdown-secties en een MCP-server met twee read-only tools erop.

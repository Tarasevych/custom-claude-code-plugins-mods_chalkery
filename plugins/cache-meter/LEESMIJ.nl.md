# cache-meter

[Tweetalig overzicht / Двомовний огляд](README.md) | [Українська](README.uk.md) | **Nederlands**

Een afsplitsing van het tussengeheugengedeelte van `cache-keeper` uit [nateherkai/claude-code-mods](https://github.com/nateherkai/claude-code-mods) (`33a936f`, door Nate Herk), zonder het overzichtsbord, de overdrachtsfunctie en de opnamestand.

Elk verzoek leest de volledige context opnieuw in. Vanuit het tussengeheugen kost dat ongeveer een tiende van de normale invoerprijs. Zodra het tussengeheugen verloopt (na 1 uur rust op een abonnement of 5 minuten volgens de standaardlevensduur van de programmagegevenskoppeling), schrijft het volgende bericht de gehele context opnieuw weg tegen 1,25 tot 2 maal de invoerprijs. Deze uitbreidingsmodule toont de actuele staat van het tussengeheugen en wat het opnieuw vullen ervan zou kosten.

## Wat u ziet

- Een rij boven het invoerveld: of het tussengeheugen warm is, afkoelt of koud is geworden, en wat het opnieuw vullen zou kosten. Via de programmagegevenskoppeling wordt dit in dollars getoond. Op een abonnement wordt dit in rekeneenheden weergegeven, aangezien een abonnement afrekent via bundelgrenzen in plaats van dollars; hetzelfde geldt voor modellen waarvan de module geen prijzen kent. De opdracht `/cache` voegt het dollarbedrag toe als vergelijkingswaarde. Bundelgrenzen verschijnen zodra een grens de 80% overschrijdt.
- **Warmhouden**: een knop (`k` in het tekstvenster) of `/keepwarm [uren|off]`; zie [Warmhouden](#warmhouden) voor de werking en het verbruik.
- Een bevestigingsvraag voordat u een bericht stuurt naar een afgekoeld groot tussengeheugen (standaard vanaf 150 duizend rekeneenheden): «Toch verzenden», «Samenvatten en verzenden», «Voer `/handoff` uit» (indien beschikbaar in de werksessie) of «Annuleren».
- Een korte melding wanneer een groot tussengeheugen op het punt staat af te koelen.

Hoe lang het tussengeheugen meegaat: 60 minuten op een abonnement en 5 minuten via de programmagegevenskoppeling. De module onderscheidt deze doordat `Claude Code` na het eerste antwoord al dan niet bundelgrenzen doorgeeft. Latere metingen en modelwisselingen verfijnen deze waarde. Met `/cache` ziet u waar het getal vandaan komt.

## Warmhouden

De functie voor warmhouden voorkomt dat het tussengeheugen afkoelt terwijl u weg bent. Vóór elke vervaltijd stuurt de module een klein onderhoudsverzoek dat het tussengeheugen herleest en zo de klok opnieuw start. Standaard draait dit 4 uur en hoogstens 24 uur; met `/keepwarm off` of de knop om te stoppen beëindigt u het eerder.

Dit is niet kosteloos. Elk onderhoudsverzoek leest de gehele context opnieuw: via de programmagegevenskoppeling kost dat een fractie van de invoerprijs (ongeveer \$0.04 voor 200 duizend rekeneenheden van Opus 5.5); op een abonnement tellen dezelfde rekeneenheden mee voor uw bundelgrenzen. Een tussengeheugen van een uur op een abonnement vraagt elke 52 minuten een onderhoudsverzoek (circa 4 in 4 uur); een tussengeheugen van 5 minuten vraagt er elke 3,5 minuut een (circa 70). Bij de start schat een melding hoeveel onderhoudsverzoeken er zullen uitgaan en hoeveel elk verzoek leest.

Het warmhouden stopt vanzelf wanneer de ingestelde tijd verstrijkt, wanneer een onderhoudsverzoek het tussengeheugen opnieuw schrijft in plaats van leest (het tussengeheugen was dan toch al afgekoeld), wanneer een onderhoudsverzoek geen antwoord krijgt, en zodra een bundelgrens 90% bereikt: onderhoudsverzoeken mogen niet de ruimte verbruiken die uw volgende echte bericht nodig heeft. Bij 90% of hoger start het warmhouden niet.

## Opdrachten

| Opdracht | Werking |
| --- | --- |
| `/cache` | Toont de status van het tussengeheugen en de instellingen |
| `/cache ttl 5\|60\|auto` | Stelt de levensduur van het tussengeheugen enkel voor deze werksessie in; `auto` keert terug naar de gedetecteerde waarde |
| `/cache guard on\|off` | Schakelt de bevestigingsvraag vóór verzending naar een koud tussengeheugen in of uit |
| `/cache big 150k` | Bepaalt vanaf hoeveel rekeneenheden een context als groot telt |
| `/cache alerts on\|off` | Schakelt de melding vóór het afkoelen van het tussengeheugen in of uit |
| `/keepwarm [uren\|off]` | Houdt het tussengeheugen warm of stopt daarmee |

Als een andere uitbreidingsmodule `/cache` of `/keepwarm` al gebruikt, heten de opdrachten van deze module `/cm-cache` en `/cm-keepwarm`.

## Voor andere uitbreidingsmodules

De module publiceert de staat van het tussengeheugen in `$.state` onder `cache-meter.cache` (zie `types/index.d.ts`). [handoff-relay](../handoff-relay/LEESMIJ.nl.md) en [next-steps](../next-steps/LEESMIJ.nl.md) lezen deze gegevens om te tonen wat een druk op hun knop kost bij een koud tussengeheugen. `cache-meter` zelf is niet afhankelijk van andere modules.

## Instellingen

| Instelling | Standaardwaarde | Werking |
| --- | --- | --- |
| `language` | `auto` | `auto` volgt de antwoordtaal van Claude uit `/config`; `en` of `uk` stelt deze vast in |

## Controles

```bash
claude plugin validate .
claude plugin test .
```

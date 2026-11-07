<!-- SPDX-License-Identifier: Apache-2.0. Modified by Yehor Hunia, 2026, from anthropics/claude-plugins-community@87c843d (next-steps). -->
# next-steps (op verzoek)

[Tweetalig overzicht / Двомовний огляд](README.md) | [Українська](ОГЛЯД.uk.md) | **Nederlands**

Een afsplitsing van [next-steps](https://github.com/anthropics/claude-plugins-community/tree/main/next-steps) uit `anthropics/claude-plugins-community@87c843d` (door Thariq Shihipar). In het oorspronkelijke ontwerp werden na elke beurt automatisch vervolgopdrachten voorgesteld, waardoor elke beurt een extra modelaanroep kostte. Hier toont de strook boven het invoerveld enkel een knop «Wat nu?», en wordt het model pas bevraagd wanneer u op die knop drukt.

Door in een leeg invoerveld op `1`, `2` of `3` te drukken (of door te klikken) wordt een voorstel als concepttekst in het invoerveld geplaatst: u kunt de tekst naar wens aanpassen en drukt zelf op de bevestigingstoets. Met `0` verbergt u de voorstellen en keert de knop terug. Het eerste voorstel verschijnt bovendien als grijze voorsteltekst in het invoerveld en kan met de tabtoets worden overgenomen. De module verstuurt nooit zelf een bericht en vangt de cijfertoetsen enkel af zolang de voorstellen op het scherm staan.

## Hoe het werkt

Bestand `hooks/register.tsx`, functie-onderscheppers:

- `turn.complete`: na een antwoord van de hoofdlus (niet van een hulpassistent) dat langer is dan `minAnswerChars`, verschijnt de knop. Het model wordt nog niet bevraagd.
- Druk op de knop: `$.model.fork` met dezelfde context, zodat het voorvoegsel uit het tussengeheugen wordt gelezen. De afsplitsing ontvangt de vaardigheden en schuine-streepopdrachten van de werksessie (`$.command.list`); een voorstel dat een niet-bestaande opdracht noemt, wordt weggelaten.
- Als [cache-meter](../cache-meter/LEESMIJ.nl.md) is geplaatst en een groot tussengeheugen koud is geworden, toont de knop wat het opnieuw vullen van het tussengeheugen zou kosten.
- `turn.start` verbergt alles.
- De toestand van de strook wordt bewaard in `$.state` (`next-steps.view`), zodat deze bij direct herladen behouden blijft.

Voorstellen zijn afkomstig van een model dat mogelijk onvertrouwde tekst heeft gelezen; daarom worden ze vóór weergave ontdaan van stuurcodes en onzichtbare tekens (de oorspronkelijke functie `clean`, ongewijzigd).

## Instellingen

| Instelling | Standaardwaarde | Werking |
| --- | --- | --- |
| `language` | `auto` | `auto` volgt de antwoordtaal van Claude uit `/config`; `en` of `uk` stelt deze vast in |
| `minAnswerChars` | `80` | Geen knop na kortere antwoorden |
| `suggestSkills` | `true` | Geeft de vaardigheden en schuine-streepopdrachten van de sessie door aan de voorsteller |

## Controles

```bash
claude plugin validate .
claude plugin test .
```

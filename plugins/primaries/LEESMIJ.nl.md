# primaries

[Tweetalig overzicht / Двомовний огляд](README.md) | [Українська](README.uk.md) | **Nederlands**

Het verzamelpakket: alle drie de uitbreidingsmodules van `chalkery` in één plaatsing, zoals de drie primaire kleuren. Het pakket bevat zelf geen programmacode, maar uitsluitend afhankelijkheden van [handoff-relay](../handoff-relay/LEESMIJ.nl.md), [cache-meter](../cache-meter/LEESMIJ.nl.md) en [next-steps](../next-steps/LEESMIJ.nl.md).

```bash
claude plugin install primaries@chalkery
```

Om het pakket te verwijderen voert u `claude plugin uninstall primaries@chalkery` uit. De drie afzonderlijke modules blijven daarbij behouden; met `claude plugin prune` verwijdert u ook die.

Als bij de plaatsing wordt gemeld dat een afhankelijkheid niet in een ingestelde catalogus is gevonden, raadpleeg dan het onderdeel «Probleemoplossing» in het [hoofdbestand `LEESMIJ.nl.md`](../../LEESMIJ.nl.md#probleemoplossing).

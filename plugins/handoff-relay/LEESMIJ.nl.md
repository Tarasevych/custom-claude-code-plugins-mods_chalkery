# handoff-relay

[Tweetalig overzicht / Двомовний огляд](README.md) | [Українська](ОГЛЯД.uk.md) | **Nederlands**

Een knop voor contextoverdracht boven het invoerveld. Wanneer u erop drukt, wordt `/handoff` uitgevoerd: de huidige werksessie schrijft een overdrachtsdocument voor de volgende werksessie. Vanaf 180 duizend rekeneenheden aan context (de instelling `threshold`) licht de knop op, omdat een verse werksessie vanaf dat punt doorgaans voordeliger en scherper werkt dan de huidige.

Als u een eigen `/handoff`-opdracht hebt, voert de knop die uit. Anders gebruikt zij de vaardigheid [`handoff`](skills/handoff/SKILL.md) die bij deze module hoort.

## Hoe het werkt

Bestand `hooks/register.tsx`, functie-onderscheppers:

- `session.start` en `session.measure`: houden bij hoeveel rekeneenheden de context bevat.
- De knop roept `$.command.run` aan met `/handoff`. Zolang de overdrachtsbeurt loopt, meldt de strook dat het document wordt geschreven.
- De overdracht is voltooid zodra tijdens de overdrachtsbeurt een `.md`-bestand is weggeschreven en de beurt eindigt met een antwoord (niet met een vraag, onderbreking of fout). Daarna maakt de knop plaats voor de melding dat het overdrachtsdocument is aangemaakt, zodat een tweede druk op de knop het zojuist geschreven document niet overschrijft. Dit werkt met elke `/handoff`, ongeacht waar het document wordt opgeslagen, ook in het tekstvenster.
- Een overdrachtsbeurt is een beurt die is gestart via de knop, via een handmatig ingetypte `/handoff`, of doordat het model zelf de vaardigheid `handoff` aanroept.
- Wanneer de overdracht ook een kaart voor de volgende werksessie aanmaakt (`spawn_task` in de bureaubladtoepassing), vermeldt de strook dat direct samen met de naam van de kaart.
- Als [cache-meter](../cache-meter/LEESMIJ.nl.md) is geplaatst, licht de knop ook op wanneer een groot tussengeheugen bijna afkoelt of al koud is, met een toelichting over wat dat betekent voor de overdracht.

## De overdrachtsvaardigheid (`handoff`)

`skills/handoff/` schrijft het document naar `.claude/handoffs/` onder de hoofdmap van het project, of naar de map die in `CLAUDE.md` van het project wordt genoemd. Overdrachten vormen ketens: de werksessie die een overdracht oppakt krijgt de titel `<basisnaam> H<N>`, en elke fase krijgt een eigen bestand. Het script `scripts/phase.py` (`python3`, enkel standaardbibliotheek) berekent de bestands- en kaartnamen; zonder `python3` past de vaardigheid dezelfde regels zelf toe. In de bureaubladtoepassing maakt de vaardigheid een kaart aan die de volgende werksessie start; in het tekstvenster drukt zij de startopdracht af om te plakken.

## Instellingen

| Instelling | Standaardwaarde | Werking |
| --- | --- | --- |
| `language` | `auto` | `auto` volgt de antwoordtaal van Claude uit `/config`; `en` of `uk` stelt deze vast in |
| `threshold` | `180000` | Aantal rekeneenheden aan context waarna de knop oplicht |

## Controles

```bash
claude plugin validate .
claude plugin test .
python3 -m unittest discover -s skills/handoff/scripts
```

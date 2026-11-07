# chalkery

[![Doorlopende controle](https://github.com/drdickgraysonjr/chalkery/actions/workflows/ci.yml/badge.svg)](https://github.com/drdickgraysonjr/chalkery/actions/workflows/ci.yml)

[Tweetalig overzicht / Двомовний огляд](README.md) | [Українська](ОПИС.uk.md) | **Nederlands**

De naam `chalkery` verwijst naar de plek waar het krijt wordt bewaard: een onderkomen voor uitbreidingsmodules van `Claude Code` die een informatiebalk direct boven het invoerveld tekenen.

Deze verzameling omvat drie uitbreidingsmodules voor `Claude Code` die samen één overzichtelijke strook boven het invoerveld vormen, zowel in het tekstvenster als in het codetabblad van de bureaubladtoepassing.

![De informatiebalk in het codetabblad van de bureaubladtoepassing: contextoverdracht met contextomvang, status van het tussengeheugen met de knop om warm te houden, en de knop voor vervolgstappen](docs/band-desktop-uk.png)

![Dezelfde informatiebalk in het tekstvenster](docs/band-terminal-uk.png)

*De schermopnamen zijn gemaakt met een verlaagde drempelwaarde (`threshold`), zodat de overdrachtsknop al onder 180 duizend rekeneenheden oplicht.*

| Uitbreidingsmodule | Werking |
| --- | --- |
| [`handoff-relay`](plugins/handoff-relay/LEESMIJ.nl.md) | Een knop voor contextoverdracht die de opdracht `/handoff` uitvoert. Vanaf 180 duizend rekeneenheden aan context (de instelling `threshold`) licht de knop op. Zodra het overdrachtsdocument is weggeschreven, maakt de knop plaats voor een melding dat de overdracht is aangemaakt. De module bevat een eigen vaardigheid `handoff`, zodat de knop direct na plaatsing werkt. |
| [`cache-meter`](plugins/cache-meter/LEESMIJ.nl.md) | Toont hoe lang het tussengeheugen van de opdracht nog warm blijft en wat het opnieuw vullen ervan zou kosten. Met de knop voor warmhouden voorkomt u dat het tussengeheugen afkoelt. Voordat u een bericht verstuurt naar een afgekoeld groot tussengeheugen, vraagt de module eerst om bevestiging. |
| [`next-steps`](plugins/next-steps/LEESMIJ.nl.md) | Een knop «Wat nu?» die tot drie volgende verzoeken voorstelt. Het gekozen voorstel verschijnt als concepttekst in het invoerveld; u drukt zelf op de bevestigingstoets. |

U kunt de modules afzonderlijk of allemaal tegelijk plaatsen. In elke combinatie vormen ze één gezamenlijke balk met altijd dezelfde volgorde: contextoverdracht, tussengeheugen, vervolgstappen.

## Vereisten

`Claude Code` versie 2.1.289 of nieuwer. De uitbreidingsmodules zijn gebouwd op functie-onderscheppers, een nieuw onderdeel van `Claude Code`, en zijn getoetst op versie 2.1.289. Oudere versies kunnen de beschrijvingsbestanden weigeren.

Controleer uw versie met `claude --version` en werk bij met `claude update`. De bureaubladtoepassing werkt zichzelf automatisch bij.

## Plaatsing en beheer

Voeg eerst de catalogus toe:

```bash
claude plugin marketplace add drdickgraysonjr/chalkery
```

Plaats alle drie de modules met één opdracht via het verzamelpakket [`primaries`](plugins/primaries/LEESMIJ.nl.md) (drie modules, zoals de drie primaire kleuren):

```bash
claude plugin install primaries@chalkery
```

Of plaats enkel de gewenste module:

```bash
claude plugin install cache-meter@chalkery
```

In de bureaubladtoepassing vindt u dezelfde mogelijkheid onder het menu voor uitbreidingsmodules en catalogi. De modules worden actief vanaf de volgende werksessie.

Wanneer er een nieuwe uitgave beschikbaar is, ververst u eerst de catalogus en werkt u daarna elke geplaatste module bij:

```bash
claude plugin marketplace update chalkery
```

```bash
claude plugin update cache-meter@chalkery
```

Om het verzamelpakket te verwijderen voert u `claude plugin uninstall primaries@chalkery` uit. De drie afzonderlijke modules blijven daarbij behouden; met `claude plugin prune` verwijdert u ook die.

## Taalinstelling

De uitbreidingsmodules ondersteunen Oekraïens en Engels in de gebruikersomgeving. Elke module heeft een instelling `language`: `auto` (standaardwaarde), `en` of `uk`. Met `auto` volgen de modules de antwoordtaal van Claude uit `/config`, zodat één instelling alle drie bedient. Om de taal uitdrukkelijk vast te leggen wijzigt u de instelling in `/config` of geeft u deze mee tijdens de plaatsing:

```bash
claude plugin install cache-meter@chalkery --config language=uk
```

## De overdrachtsvaardigheid (`handoff`)

De module `handoff-relay` levert een ingebouwde vaardigheid [`handoff`](plugins/handoff-relay/skills/handoff/SKILL.md) mee. Deze schrijft een overdrachtsdocument voor de volgende werksessie naar de map `.claude/handoffs/` in de hoofdmap van het project, of naar een andere map indien `CLAUDE.md` van het project dat aangeeft. Overdrachten vormen ketens: de werksessie die een document overneemt krijgt de titel `<basisnaam> H<N>`, en elke fase krijgt een eigen bestand. In de bureaubladtoepassing maakt de vaardigheid bovendien een kaart aan waarmee de volgende werksessie direct wordt gestart; in het tekstvenster wordt de startopdracht afgedrukt om te kopiëren. Als u al een eigen `/handoff` gebruikt, voert de knop uw eigen versie uit.

## Instellingen van `next-steps`

Na plaatsing kan de opdrachtregel melden dat enkele instellingen niet zijn opgegeven. Alle instellingen hebben echter standaardwaarden, zodat handmatig instellen niet verplicht is.

| Instelling | Standaardwaarde | Werking |
| --- | --- | --- |
| `language` | `auto` | `auto`, `en` of `uk`, zoals hierboven beschreven |
| `minAnswerChars` | `80` | Geen knop na kortere antwoorden |
| `suggestSkills` | `true` | Een voorstel kan ook een vaardigheid of schuine-streepopdracht van de sessie zijn |

## Probleemoplossing

**Melding over een onbekende sleutel `options` of een ongeldig beschrijvingsbestand.** Uw versie van `Claude Code` is ouder dan vereist. Voer `claude update` uit en probeer het opnieuw.

**Het pakket `primaries` meldt dat een afhankelijkheid niet is gevonden in een ingestelde catalogus.** De catalogus zelf is doorgaans in orde: een van de drie modules kon niet worden geplaatst en het verzamelpakket geeft dat op deze manier weer. Plaats de betreffende module afzonderlijk om de werkelijke foutmelding te zien:

```bash
claude plugin install handoff-relay@chalkery
```

Meestal betreft het de hierboven genoemde fout vanwege een verouderde programmaversie.

## Voor ontwikkelaars

```
.claude-plugin/marketplace.json   de catalogus: het pakket en de drie modules, elk gekoppeld aan een uitgavemarkering
plugins/<module>/                 elke module staat op zichzelf en kan afzonderlijk worden geplaatst
plugins/<module>/hooks/locales/   taalbestanden (en.mjs en uk.mjs) met alle zichtbare teksten
plugins/handoff-relay/skills/     de overdrachtsvaardigheid en het fasescript (python3, inclusief toetsen)
plugins/primaries/                het verzamelpakket: bevat uitsluitend afhankelijkheden van de drie modules
shared/band.mjs                   de gedeelde afspraak voor de gezamenlijke informatiebalk
shared/i18n.mjs                   taalkeuze op basis van de instelling language
scripts/sync-shared.sh            kopieert shared/*.mjs naar de map hooks/ van elke module
scripts/pin-releases.sh           verbindt de catalogus met de nieuwste uitgavemarkering van elke module
scripts/check.sh                  voert alle controles uit die ook bij doorlopende controle draaien
docs/                             schermopnamen voor de documentatie
.github/workflows/ci.yml          automatische controle bij elke wijziging en wekelijks op maandag
```

**De gedeelde informatiebalk.** De volgorde waarin de verwerkingskern de `ui.render`-onderscheppers van verschillende uitbreidingsmodules aan elkaar koppelt, ligt niet vast en hangt af van de manier van plaatsing. Daarom plaatst een module haar rij niet simpelweg boven of onder het resultaat van `next(e)`. In plaats daarvan voegt zij haar rij in de gezamenlijke kolom `prompt-band` op een vaste positie in: contextoverdracht op `10`, tussengeheugen op `20`, en vervolgstappen op `30`. Vreemde rijen (van de verwerkingskern zelf of van externe modules) komen daaronder terecht. Omdat een module ook zonder haar buren geplaatst kan worden, worden `band.mjs` en `i18n.mjs` naar elke module gekopieerd. Bewerk altijd `shared/` en voer daarna `scripts/sync-shared.sh` uit; `scripts/sync-shared.sh --check` controleert of alle kopieën overeenkomen.

**Talen.** De programmacode van een module bevat geen enkele tekst die rechtstreeks aan de gebruiker wordt getoond; alle teksten komen uit `locales/<taal>.mjs`. Een nieuwe taal toevoegen betekent één extra bestand per module, een vermelding in `LOCALES` en een herkenningspatroon in `shared/i18n.mjs`. De toetsen van elke module controleren of alle taalbestanden exact dezelfde sleutels bevatten.

**Controles.** Het script `scripts/check.sh` voert alle controles uit: het valideert de beschrijvingsbestanden, voert de toetsen van elke module en van het fasescript uit, vergelijkt de gedeelde bestanden, controleert versienummers ten opzichte van uitgavemarkeringen, verifieert dat de catalogus naar de nieuwste uitgavemarkeringen verwijst en plaatst `primaries` vanuit de catalogus in een tijdelijke thuismap. Om één enkele module te controleren:

```bash
claude plugin validate plugins/cache-meter && claude plugin test plugins/cache-meter
```

**Rechtstreekse ontwikkeling.** Om vanuit uw lokale werkkopie te werken in plaats van de geplaatste versie, voegt u de modulemappen toe aan de omgevingsvariabele `CLAUDE_CODE_PLUGIN_DIRS` (gescheiden door dubbele punten). Een module uit die map krijgt voorrang boven een geplaatste module met dezelfde naam.

**Uitgave van een nieuwe versie.** Verhoog `version` in `plugins/<module>/.claude-plugin/plugin.json`. Zonder verhoging merkt `claude plugin update` de wijziging niet op en faalt de controle zodra een module sinds haar laatste uitgavemarkering is gewijzigd. Maak na samenvoeging vanuit de modulemap de uitgavemarkering aan met `claude plugin tag --push` (dit vormt `<module>--v<versie>`). Voer vervolgens `scripts/pin-releases.sh` uit en voeg het bijgewerkte `marketplace.json` samen.

**Wat gebruikers ontvangen.** Elke vermelding in de catalogus verwijst naar een vaste uitgavemarkering en de bijbehorende vastlegging, zodat `claude plugin install` en `claude plugin update` altijd de vrijgegeven code leveren en nooit de tussenliggende staat op de hoofdtak `main`.

**Controlelijst vóór een uitgave.** De automatische controle ziet de visuele strook zelf niet; controleer daarom handmatig vóór het plaatsen van een uitgavemarkering:

1. `scripts/check.sh` slaagt volledig.
2. Met de gewijzigde modules in `CLAUDE_CODE_PLUGIN_DIRS` toont een nieuwe werksessie in het tekstvenster de strook in de volgorde: contextoverdracht, tussengeheugen, vervolgstappen.
3. Hetzelfde geldt voor het codetabblad in de bureaubladtoepassing.
4. De overdrachtsknop start `/handoff` en verandert na het opslaan van het document in de melding dat de overdracht is aangemaakt; «Wat nu?» biedt vervolgstappen en `/cache` toont de status van het tussengeheugen.
5. Bij het wisselen van `language` naar `uk` en `en` veranderen alle opschriften van taal.
6. Elke gewijzigde module heeft een verhoogd versienummer in `version`.

## Licenties

`handoff-relay`, `primaries` en de gedeelde code vallen onder de MIT-licentie, Yehor Hunia. `cache-meter` is een afsplitsing van het tussengeheugengedeelte van `cache-keeper` uit [nateherkai/claude-code-mods](https://github.com/nateherkai/claude-code-mods), MIT-licentie, Nate Herk. `next-steps` is een afsplitsing van [anthropics/claude-plugins-community/next-steps](https://github.com/anthropics/claude-plugins-community/tree/main/next-steps), Apache 2.0-licentie. De licentietekst van elke module bevindt zich in de bijbehorende map.

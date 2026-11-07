# chalkery

[![Безперервна перевірка / Doorlopende controle](https://github.com/drdickgraysonjr/chalkery/actions/workflows/ci.yml/badge.svg)](https://github.com/drdickgraysonjr/chalkery/actions/workflows/ci.yml)

**Двомовний огляд / Tweetalig overzicht** | [Українська (`README.uk.md`)](ОПИС.uk.md) | [Українська (`ОПИС.uk.md`)](ОПИС.uk.md) | [Nederlands (`LEESMIJ.nl.md`)](LEESMIJ.nl.md)

---

## 🇺🇦 Українська

Назва `chalkery` означає місце, де зберігають крейду: оселя для модифікацій середовища `Claude Code`, які виводять інформаційну смугу безпосередньо над полем введення запиту.

Три модифікації для `Claude Code` формують єдину інформаційну смугу над полем введення — як у терміналі, так і у вкладці коду настільного застосунку.

![Інформаційна смуга у вкладці коду настільного застосунку](docs/band-desktop-uk.png)

![Та сама інформаційна смуга в терміналі](docs/band-terminal-uk.png)

| Модифікація | Призначення | Документація |
| --- | --- | --- |
| [`handoff-relay`](plugins/handoff-relay/README.md) | Кнопка передачі контексту запускає команду `/handoff` і підсвічується від 180 тисяч облікових одиниць контексту. Містить вбудовану навичку [`handoff`](plugins/handoff-relay/skills/handoff/SKILL.md). | [УКР](plugins/handoff-relay/ОГЛЯД.uk.md) · [НЛД](plugins/handoff-relay/LEESMIJ.nl.md) |
| [`cache-meter`](plugins/cache-meter/README.md) | Показує час збереження проміжної пам'яті запиту, вартість повторного запису та кнопку «Тримати теплим». Попереджає перед надсиланням у велику охололу проміжну пам'ять. | [УКР](plugins/cache-meter/ОГЛЯД.uk.md) · [НЛД](plugins/cache-meter/LEESMIJ.nl.md) |
| [`next-steps`](plugins/next-steps/README.md) | Кнопка «Що далі?» на вимогу пропонує до трьох наступних запитів і вставляє обраний варіант як чернетку в поле введення. | [УКР](plugins/next-steps/ОГЛЯД.uk.md) · [НЛД](plugins/next-steps/LEESMIJ.nl.md) |
| [`primaries`](plugins/primaries/README.md) | Зведений набір, який встановлює всі три модифікації однією командою, немов три основні кольори. | [УКР](plugins/primaries/ОГЛЯД.uk.md) · [НЛД](plugins/primaries/LEESMIJ.nl.md) |

### Швидке встановлення

```bash
claude plugin marketplace add drdickgraysonjr/chalkery
claude plugin install primaries@chalkery
```

Повний посібник із вимогами, налаштуванням мови, описом навички передачі контексту та відомостями для розробників читайте в [повній українській документації (`README.uk.md`)](ОПИС.uk.md) (також доступній як [`ОПИС.uk.md`](ОПИС.uk.md)). Правила супроводу сховища викладено у [`CLAUDE.md`](CLAUDE.md).

---

## 🇳🇱 Nederlands

De naam `chalkery` verwijst naar de plek waar het krijt wordt bewaard: een onderkomen voor uitbreidingsmodules van `Claude Code` die een informatiebalk direct boven het invoerveld tekenen.

Drie uitbreidingsmodules voor `Claude Code` vormen samen één overzichtelijke strook boven het invoerveld, zowel in het tekstvenster als in het codetabblad van de bureaubladtoepassing.

| Uitbreidingsmodule | Werking | Documentatie |
| --- | --- | --- |
| [`handoff-relay`](plugins/handoff-relay/README.md) | Een knop voor contextoverdracht die `/handoff` uitvoert en oplicht vanaf 180 duizend rekeneenheden aan context. Bevat de ingebouwde vaardigheid [`handoff`](plugins/handoff-relay/skills/handoff/SKILL.md). | [NL](plugins/handoff-relay/LEESMIJ.nl.md) · [UK](plugins/handoff-relay/ОГЛЯД.uk.md) |
| [`cache-meter`](plugins/cache-meter/README.md) | Toont hoe lang het tussengeheugen warm blijft, wat opnieuw vullen kost en biedt een knop om het tussengeheugen warm te houden. Vraagt bevestiging vóór verzending naar een afgekoeld groot tussengeheugen. | [NL](plugins/cache-meter/LEESMIJ.nl.md) · [UK](plugins/cache-meter/ОГЛЯД.uk.md) |
| [`next-steps`](plugins/next-steps/README.md) | Een knop «Wat nu?» die op verzoek tot drie volgende opdrachten voorstelt en de gekozen optie als concept in het invoerveld plaatst. | [NL](plugins/next-steps/LEESMIJ.nl.md) · [UK](plugins/next-steps/ОГЛЯД.uk.md) |
| [`primaries`](plugins/primaries/README.md) | Het verzamelpakket waarmee alle drie de uitbreidingsmodules in één keer worden geplaatst, zoals drie primaire kleuren. | [NL](plugins/primaries/LEESMIJ.nl.md) · [UK](plugins/primaries/ОГЛЯД.uk.md) |

### Snelle plaatsing

```bash
claude plugin marketplace add drdickgraysonjr/chalkery
claude plugin install primaries@chalkery
```

Raadpleeg de [volledige Nederlandse documentatie (`LEESMIJ.nl.md`)](LEESMIJ.nl.md) voor alle vereisten, taalinstellingen, de werking van de overdrachtsvaardigheid en richtlijnen voor ontwikkelaars. De afspraken voor het onderhouden van deze opslagplaats staan in [`CLAUDE.md`](CLAUDE.md).

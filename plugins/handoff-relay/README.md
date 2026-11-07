# handoff-relay

**Двомовний огляд / Tweetalig overzicht** | [Українська (`README.uk.md`)](ОГЛЯД.uk.md) | [Nederlands (`LEESMIJ.nl.md`)](LEESMIJ.nl.md)

---

## 🇺🇦 Українська

Модифікація `handoff-relay` додає кнопку передачі контексту над полем введення `Claude Code`. Вона запускає команду `/handoff`, підсвічується при досягненні порогу обсягу контексту (типово 180 тисяч облікових одиниць) або перед охолодженням великої проміжної пам'яті та містить вбудовану навичку [`handoff`](skills/handoff/SKILL.md) для ведення ланцюжків послідовних сеансів.

- Повний посібник українською мовою: [`README.uk.md`](ОГЛЯД.uk.md)
- Опис вбудованої навички передачі контексту: [`skills/handoff/SKILL.md`](skills/handoff/SKILL.md)
- Сусідні модифікації: [`cache-meter`](../cache-meter/README.uk.md), [`next-steps`](../next-steps/README.uk.md), набір [`primaries`](../primaries/README.uk.md)
- Загальний опис сховища: [головний `README.uk.md`](../../README.uk.md)

---

## 🇳🇱 Nederlands

De uitbreidingsmodule `handoff-relay` plaatst een knop voor contextoverdracht boven het invoerveld van `Claude Code`. De knop voert `/handoff` uit, licht op zodra de contextdrempel (standaard 180 duizend rekeneenheden) wordt bereikt of wanneer een groot tussengeheugen dreigt af te koelen, en bevat de ingebouwde vaardigheid [`handoff`](skills/handoff/SKILL.md) voor geketende werksessies.

- Volledige handleiding in het Nederlands: [`LEESMIJ.nl.md`](LEESMIJ.nl.md)
- Beschrijving van de ingebouwde overdrachtsvaardigheid: [`skills/handoff/SKILL.md`](skills/handoff/SKILL.md)
- Verwante uitbreidingsmodules: [`cache-meter`](../cache-meter/LEESMIJ.nl.md), [`next-steps`](../next-steps/LEESMIJ.nl.md), verzamelpakket [`primaries`](../primaries/LEESMIJ.nl.md)
- Algemeen overzicht van de opslagplaats: [hoofdbestand `LEESMIJ.nl.md`](../../LEESMIJ.nl.md)

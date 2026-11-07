# chalkery — Пам'ятка для розробників / Richtlijnen voor ontwikkelaars

[Двомовний огляд / Tweetalig overzicht](README.md) | [Українська (`README.uk.md`)](ОПИС.uk.md) | [Nederlands (`LEESMIJ.nl.md`)](LEESMIJ.nl.md)

---

## 🇺🇦 Українська

Модифікації середовища `Claude Code`, побудовані на функційних перехоплювачах, що малюють інформаційну смугу над полем введення запиту, а також зведений набір `primaries`, який встановлює всі три модифікації разом. У файлі [`README.uk.md`](ОПИС.uk.md) (розділ «Відомості для розробників») описано структуру сховища коду та прийняті домовленості.

### Мовні правила

Уся документація сховища ведеться чистою українською (`README.uk.md`, `ОПИС.uk.md`) та нідерландською (`LEESMIJ.nl.md`) мовами, а оглядові файли `README.md` є двомовними (українсько-нідерландськими). Написи, які бачить користувач під час роботи модифікацій, містяться у файлах `hooks/locales/en.mjs` та `hooks/locales/uk.mjs` і оновлюються узгоджено.

### Ліцензії

Кожен файл вихідного коду починається з рядка `SPDX-License-Identifier` (одразу після заголовка інтерпретатора або директив розмітки). Файли модифікації `next-steps` поширюються за ліцензією Apache-2.0 і містять обов'язкову примітку про змінення згідно з пунктом 4(b) ліцензії Apache-2.0; копії спільних файлів у ній залишаються під ліцензією MIT. Усі інші складові поширюються за ліцензією MIT.

### Перед створенням запиту на злиття

- Запустіть `scripts/check.sh`. Система безперервної перевірки виконує ті самі перевірки під час кожного надсилання змін і кожного запиту на злиття на найстарішій підтримуваній версії `Claude Code`, а також щотижня на найновішій версії.
- Спільний код редагуйте виключно в теці `shared/`, після чого запускайте `scripts/sync-shared.sh`; ніколи не редагуйте скопійовані файли безпосередньо в `plugins/<модифікація>/hooks/`.
- Кожна модифікація, яка зазнала змін після своєї останньої мітки випуску (`<модифікація>--v<версія>`), повинна мати вище значення `version` у своєму файлі `plugin.json`; інакше перевірка завершиться помилкою.
- Каталог розширень встановлює кожну модифікацію з її мітки випуску, а не з гілки `main`. Зміни в поведінці модифікації перед злиттям перевіряються наживо через змінну `CLAUDE_CODE_PLUGIN_DIRS`.
- Порядок випуску: підвищити `version`, злити зміни, виконати `claude plugin tag --push` у теці модифікації, потім запустити `scripts/pin-releases.sh` і внести оновлений `marketplace.json`.

---

## 🇳🇱 Nederlands

Uitbreidingsmodules voor `Claude Code`, gebouwd op functie-onderscheppers die een informatiebalk boven het invoerveld tekenen, plus het verzamelpakket `primaries` dat alle drie de modules tegelijk plaatst. In [`LEESMIJ.nl.md`](LEESMIJ.nl.md) (onderdeel «Voor ontwikkelaars») vindt u de indeling en de geldende afspraken.

### Taalafspraken

Alle documentatie in deze opslagplaats is opgesteld in zuiver Oekraïens (`README.uk.md`, `ОПИС.uk.md`) en Nederlands (`LEESMIJ.nl.md`), waarbij de `README.md`-bestanden een tweetalig overzicht bieden. Teksten die een gebruiker in de toepassing ziet, staan in `hooks/locales/en.mjs` en `hooks/locales/uk.mjs` en worden altijd samen bijgewerkt.

### Licenties

Elk broncodebestand begint met een `SPDX-License-Identifier`-opmerking. De bestanden van `next-steps` vallen onder Apache-2.0 en bevatten de verplichte wijzigingsvermelding volgens artikel 4(b) van Apache-2.0; de kopieën van de gedeelde bestanden blijven onder MIT. Alle overige bestanden vallen onder de MIT-licentie.

### Vóór het indienen van een samenvoegingsverzoek

- Voer `scripts/check.sh` uit. De doorlopende controle voert dezelfde toetsen uit bij elke wijziging en elk samenvoegingsverzoek tegen de oudste ondersteunde versie van `Claude Code`, en wekelijks tegen de nieuwste versie.
- Bewerk gedeelde code uitsluitend in `shared/` en voer daarna `scripts/sync-shared.sh` uit; wijzig nooit rechtstreeks de kopieën in `plugins/<module>/hooks/`.
- Een module die sinds haar laatste uitgavemarkering (`<module>--v<versie>`) is gewijzigd, moet een hoger versienummer (`version`) in haar `plugin.json` krijgen; anders faalt de controle.
- De catalogus plaatst elke module vanaf haar uitgavemarkering, niet vanaf `main`. Wijzigingen in het gedrag van een module worden vóór samenvoeging rechtstreeks getest via `CLAUDE_CODE_PLUGIN_DIRS`.
- Een nieuwe uitgave verloopt als volgt: verhoog `version`, voeg samen, voer `claude plugin tag --push` uit in de map van de module, draai daarna `scripts/pin-releases.sh` en dien het vernieuwde `marketplace.json` in.

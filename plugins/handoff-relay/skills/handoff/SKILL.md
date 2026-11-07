---
name: handoff
description: Ущільнює поточну розмову в документ передачі контексту для наступного виконавця; використовується, коли контекст стає завеликим, завершується етап послідовного ланцюжка або користувач натискає кнопку передачі контексту. / Vat het huidige gesprek samen in een overdrachtsdocument voor een volgende werksessie wanneer de context groot wordt of een fase wordt afgerond.
argument-hint: "Для чого призначено наступний сеанс? / Waarvoor dient de volgende werksessie?"
---

# Навичка передачі контексту / Vaardigheid voor contextoverdracht (`handoff`)

---

## 🇺🇦 Українська

Складіть документ передачі контексту, який підсумовує поточну розмову так, щоб новий виконавець у свіжому сеансі зміг безперервно продовжити роботу. Пишіть документ мовою поточної розмови.

### Куди зберігати документ

Збережіть файл у теці передач контексту проєкту: `.claude/handoffs/` у корені проєкту (корінь сховища `git` або поточна робоча тека поза сховищем). Створіть цю теку, якщо її ще немає. Якщо у файлі `CLAUDE.md` проєкту або у вказівці користувача названо іншу теку для передач контексту, використовуйте саме її. Збережіть там один файл під назвою, визначеною правилами ланцюжка етапів нижче; не створюйте зайвих копій у `/tmp` чи інших місцях.

### Зміст документа

- У чому полягає завдання, у якому воно стані зараз і що залишається відкритим. Чітко зазначте, що вже перевірено і яким способом, а що ще не перевірялося.
- Розділ рекомендованих навичок: які саме навички має викликати наступний виконавець і на якому кроці.
- Посилання замість дублювання: не переписуйте вміст технічних вимог, планів, архітектурних рішень, задач, фіксацій змін або порівнянь коду; вказуйте на них за шляхом до файлу або вебпосиланням.
- Жодних таємних даних: приховуйте ключі програмного інтерфейсу, паролі, ключі доступу й особисті відомості.

Якщо користувач передав додаткові аргументи, вони описують мету наступного сеансу — пристосуйте зміст документа саме до цієї мети.

### Ланцюжок етапів

Кожна передача контексту є одним етапом ланцюжка: сеанс, який її приймає, отримує назву `<основа> H<N>`, і кожен етап має власний окремий файл; наявний файл ніколи не перезаписується для наступного етапу.

1. **Визначте поточний етап.** Якщо перше повідомлення розмови містить рядок ланцюжка, він має перевагу над усіма іншими джерелами (користувач міг перейменувати сеанс). Цей рядок має вигляд `Ланцюжок: <основа>, фаза H<N>, попередній: <шлях>` (або відповідник мовою документа). Інакше візьміть назву сеансу: у настільному застосунку її повертає виклик `mcp__ccd_session_mgmt__get_session` із параметром `session_id: "self"`. Якщо немає ні рядка ланцюжка, ні назви (сеанс у терміналі без початкового рядка), розпочніть новий ланцюжок: основою назви стає короткий опис роботи (від трьох до шести слів), а поточним етапом вважається `H1`.
2. **Обчисліть назви.** Виконайте команду:
   `python3 "${CLAUDE_SKILL_DIR}/scripts/phase.py" --handoffs-dir "<тека передач>"` із параметром `--title "<назва>"`, або з `--chain "<основа>" --phase <N>` із рядка ланцюжка, або з `--chain "<основа>" --phase 1` для нового ланцюжка.
   Сценарій виводить поля `base`, `phase`, `next_phase`, `card_title`, `file`, `previous`, `previous_exists`. Використовуйте ці значення без змін.
   Якщо `python3` недоступний, застосуйте ті самі правила самостійно: назва, що закінчується на ` H<N>` (пробіл, літера `H` і додатне число), означає етап `N` ланцюжка з назвою решти рядка; будь-яка інша назва означає етап `1`; `next_phase` = `N + 1`; `file` = `<основа> — H<next_phase>.md`, де символи `* " \ / < > : | ? # ^ [ ]` в основі замінено на `-`; `previous` = `<основа> — H<N>.md` за `N ≥ 2`, інакше порожньо; `card_title` = `<основа> H<next_phase>`, де основу за потреби скорочено з додаванням `…`, щоб загальна довжина не перевищувала 60 символів.
3. **Збережіть** документ як `<тека передач>/<file>`. Якщо такий файл уже існує і поточний сеанс його не створював, запитайте дозволу в користувача перед перезаписом.
4. **Вступні метадані.** Почніть файл блоком:
   ```yaml
   ---
   chain: <основа>
   phase: <next_phase>
   previous: "[[<previous без .md>]]"   # коли previous_exists дорівнює true; інакше: previous: null
   ---
   ```
5. **Коли `phase` ≥ 2,** поточний сеанс сам є продовженням ланцюжка. Спершу прочитайте попередній документ передачі контексту. Угорі зазначте, що це етап `H<phase>` ланцюжка, перенесіть лише ті пункти, які досі лишаються відкритими, а для всього вже виконаного вкажіть посилання на попередній файл замість повторення.
6. **Завершіть документ** розділом із заголовком `Запит для старту H<next_phase>` (у нідерландському документі: `Startopdracht voor H<next_phase>`). Першим рядком цього розділу має бути рядок ланцюжка для наступного сеансу мовою документа: `Ланцюжок: <основа>, фаза H<next_phase>, попередній: <тека передач>/<file>`. Далі коротко вкажіть, що наступний сеанс має прочитати насамперед і з чого почати.
7. **Передайте виконання.** Після збереження документа, якщо доступний інструмент `spawn_task` (у настільному застосунку), викличте його з параметрами `title` = `card_title`, `prompt` = точний текст фінального розділу та однореченням у `tldr`. Інакше виведіть цей початковий запит в окремому блоці коду, щоб користувач міг вставити його в новий сеанс. Не архівуйте, не перейменовуйте і не очищуйте поточний сеанс.

---

## 🇳🇱 Nederlands

Schrijf een overdrachtsdocument dat het huidige gesprek samenvat, zodat een nieuwe uitvoerder in een verse werksessie het werk naadloos kan voortzetten. Schrijf het document in de taal van het gesprek.

### Waar het document wordt opgeslagen

Sla het bestand op in de overdrachtsmap van het project: `.claude/handoffs/` onder de hoofdmap van het project (de `git`-hoofdmap of de werkmap buiten een opslagplaats). Maak de map aan als deze nog niet bestaat. Als `CLAUDE.md` van het project of de gebruiker een andere overdrachtsmap noemt, gebruik dan die map. Sla daar één bestand op onder de naam die de onderstaande faseketen oplevert; verspreid geen losse kopieën in `/tmp` of elders.

### Wat erin komt

- Wat het werk inhoudt, in welke staat het verkeert en welke punten nog openstaan. Vermeld wat is gecontroleerd en hoe, en wat nog niet is gecontroleerd.
- Een onderdeel met aanbevolen vaardigheden: welke vaardigheden de volgende uitvoerder moet aanroepen en wanneer.
- Verwijzingen in plaats van herhalingen: herhaal niet wat al in ontwerpspecificaties, plannen, architectuurbesluiten, meldingen, vastleggingen of codeverschillen staat, maar verwijs ernaar via bestandspad of webadres.
- Geen geheimen: verwijder toegangssleutels, wachtwoorden, machtigingsbewijzen en persoonsgegevens.

Als de gebruiker argumenten heeft meegegeven, beschrijven die waarvoor de volgende werksessie dient; stem het document daarop af.

### Faseketen

Elke overdracht vormt één fase van een keten: de werksessie die de overdracht ontvangt krijgt de titel `<basisnaam> H<N>`, en elke fase krijgt een eigen bestand; een bestaand bestand wordt nooit overschreven voor een volgende fase.

1. **Bepaal de huidige fase.** Als het eerste bericht van het gesprek een ketenregel bevat, gaat die vóór alles (de gebruiker kan de werksessie hebben hernoemd). De regel luidt `Ланцюжок: <basisnaam>, фаза H<N>, попередній: <pad>` (of het equivalent in de taal van het document). Neem anders de sessietitel: in de bureaubladtoepassing geeft `mcp__ccd_session_mgmt__get_session` met `session_id: "self"` deze terug. Zonder beide (een sessie in het tekstvenster zonder ketenregel) start u een nieuwe keten: de basisnaam is een korte omschrijving van het werk in drie tot zes woorden, en de huidige fase is `H1`.
2. **Bereken de namen.** Voer uit:
   `python3 "${CLAUDE_SKILL_DIR}/scripts/phase.py" --handoffs-dir "<overdrachtsmap>"` met `--title "<titel>"`, of met `--chain "<basisnaam>" --phase <N>` uit de ketenregel, of met `--chain "<basisnaam>" --phase 1` voor een nieuwe keten.
   Het script drukt `base`, `phase`, `next_phase`, `card_title`, `file`, `previous` en `previous_exists` af. Gebruik deze waarden ongewijzigd.
   Als `python3` niet beschikbaar is, past u dezelfde regels zelf toe: een titel die eindigt op ` H<N>` (een spatie, gevolgd door `H` en een getal) is fase `N` van de keten met de overige tekst als basisnaam; elke andere titel is fase `1`; `next_phase` = `N + 1`; `file` = `<basisnaam> — H<next_phase>.md`, waarbij `* " \ / < > : | ? # ^ [ ]` in de basisnaam worden vervangen door `-`; `previous` = `<basisnaam> — H<N>.md` wanneer `N ≥ 2`, anders leeg; `card_title` = `<basisnaam> H<next_phase>`, waarbij de basisnaam indien nodig wordt ingekort met `…` zodat de gehele titel binnen 60 tekens blijft.
3. **Sla op** als `<overdrachtsmap>/<file>`. Als dat bestand al bestaat en niet door deze werksessie is geschreven, vraag dan eerst toestemming aan de gebruiker.
4. **Kopgegevens.** Begin het bestand met:
   ```yaml
   ---
   chain: <basisnaam>
   phase: <next_phase>
   previous: "[[<previous zonder .md>]]"   # wanneer previous_exists waar is; anders: previous: null
   ---
   ```
5. **Wanneer `phase` ≥ 2,** maakte deze werksessie zelf deel uit van een bestaande keten. Lees eerst het vorige overdrachtsdocument. Vermeld bovenaan dat dit fase `H<phase>` van de keten is, neem enkel de nog openstaande punten over en verwijs voor alles wat al is afgerond naar het vorige bestand.
6. **Sluit het document af** met een sectie getiteld `Startopdracht voor H<next_phase>` (in een Oekraïens document: `Запит для старту H<next_phase>`). De eerste regel daarvan is de ketenregel voor de volgende werksessie, gevolgd door wat de volgende werksessie als eerste moet lezen en uitvoeren.
7. **Draag het werk over.** Nadat het document is opgeslagen, roept u — indien `spawn_task` beschikbaar is (in de bureaubladtoepassing) — deze functie aan met `title` = `card_title`, `prompt` = de letterlijke tekst van de startsectie en een samenvatting van één zin in `tldr`. Druk anders de startopdracht af in een codeblok zodat de gebruiker deze in een nieuwe werksessie kan plakken. Archiveer, hernoem of wis de huidige werksessie niet.

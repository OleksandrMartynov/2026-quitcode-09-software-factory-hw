# Трек розробника WS9: покроково

Ви продовжуєте **своє репо** `<я>/<team-slug>` з WS6–WS8: той самий Supabase, той самий Vercel, ruleset `solo-main`,
ворота й UAT з WS8. Нового репо для основного треку немає. Цього тижня ви **реалізуєте весь зріз 3 звичайним
способом** і доводите **кожен його AC**:

- **A** — вимоги й спека зрізу 3: `/requirements-sync`, `add-<зріз3>` з AC ID PM, план задач, питання PM;
- **B** — реалізація задачами: міграції з RLS і GRANT, перевірки на сервері, тести на кожен AC;
- **C** — UAT-запис на проді й proof pack на **кожен** AC; приймання PM;
- **D** — прод, `docs/conformance.md`, акаунти для PM, коротке ретро «що взяти з фабрики».

Бонус (необов'язково) — той самий зріз 3 реалізує фабрика в двійнику проєкту: [factory-guide](factory-guide.md) і
[twin-deploy](twin-deploy.md).

Корисні посилання:

- Рубрика й Definition of Done — у [README](../README.md#трек-розробника--100--10-балів).
- **Промпти агенту для кожного кроку** — [бібліотека промптів WS9](prompts.md) (F01–F18). Промпти WS8 (ворота,
  мутації, AI-рев'ю, відхилення) — у [бібліотеці WS8](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/prompts.md).
- Процес попередніх тижнів (типи PR, `/requirements-sync`, спека, план, задачі, `conformance`) —
  [walkthrough розробника WS7](https://github.com/koldovsky/2026-quitcode-07-fullstack-hw/blob/main/docs/walkthrough-dev.md),
  кроки 3–11. Тут — порядок і що нового.
- Гайди WS8, які діють без змін: [D0](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/deploy-d0.md) ·
  [ворота](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/gates-guide.md) ·
  [якість тестів](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/testing-guide.md) ·
  [UAT](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/uat-guide.md).

Позначення: `<я>` — ваш GitHub-логін, `<pm>` — логін PM, `<team-slug>` — слаг команди, `<проєкт>` — ім'я вашого
проєкту у Vercel, `<зріз3>` — коротка латинська назва зрізу 3 (наприклад, `vendor-access`), `AC-<AREA>-<NN>` — ID
критерію PM.

**Команди — для Windows PowerShell.** `curl.exe` замість `curl`, без `&&` (команди по черзі), `Copy-Item` замість
`cp`. У Git Bash і на macOS — звичні відповідники. Матеріали завантажуємо з цього репо; адреса — у змінній `$B`:

```powershell
$B = "https://raw.githubusercontent.com/koldovsky/2026-quitcode-09-software-factory-hw/main/materials"
```

**Бюджет основного треку: 14–18 годин** (кроки 0–7, без бонусу). Бонус фабрики — ще ~6–10 год плюс ліміти моделі.

| Крок | Коли | Бюджет | Трек |
|---|---|---|---|
| 0. Підготовка й D0 | 03.11 | 30–45 хв | D0 |
| 1. Вимоги й спека зрізу 3 | 03–06.11, після уточнень — 07.11 | 2–3 год | A |
| 2. Реалізація зрізу 3 | 05–10.11 | 6,5–8 год | B |
| 3. UAT на кожен AC | 09–10.11 (звіт бажано до кінця Вт 10.11) | 3–4 год | C |
| 4. Пакет доказів на кожен AC | 10–13.11 | 45 хв – 1 год | C |
| 5. Прод, conformance, ретро | 10–13.11 | 1 год | D |
| 6. Відхилення від PM | 11–14.11 («Виправлено» до кінця Сб 14.11) | за потреби | C (приймання PM) |
| 7. Здача | до кінця Нд 15.11 | 15 хв | — |
| Бонус. Фабрика в двійнику | будь-коли до здачі | 6–10 год + ліміти | +10 |

**Хвіст WS8 іде паралельно:** «Виправлено» в issues «Відхилення від вимог» WS8 — до кінця Сб 07.11. Це окремі
бали WS8, тож не відкладайте їх заради зрізу 3.

**Кожна задача — окремою гілкою й PR, як у WS7–WS8:** зелений CI і `quality` → `@coderabbitai review` окремим
коментарем → **відповідь на кожну знахідку рецензента** (виправлення або аргумент у треді; таблицю відповідей складає
промпт [WS8 P15](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/prompts.md#p15)) → Squash
and merge → зелений `db-deploy`. Цього тижня PR задач — тип «Реалізація», тож рядок «Поза межами типу» не потрібен.

## 0. Підготовка й D0 (30–45 хв)

1. **Здача WS8** — до кінця Вт 03.11 у репо домашки WS8.
2. **Свіжий `main` і чисте дерево:**

   ```powershell
   git switch main
   git pull
   git status --short     # порожньо
   ```

3. **Supabase не спить:** Dashboard → проєкт активний (якщо ні — **Resume project**).
4. **D0 досі зелений** (таблиця D0 — у [README](../README.md#d0--розгортання-умова-прийняття-домашки-розробника)):

   ```powershell
   npm run smoke -- https://<проєкт>.vercel.app <SHA main>
   npm ls next
   ```

   Код 0 і рядок «OK — прод живий…»; `next` ≥ 16.4.1. Якщо D0 у WS8 ще не закрито — [гайд D0 з WS8](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/deploy-d0.md),
   промпт самоперевірки — [WS8 P13](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/prompts.md#p13).
5. **Бонус — тег `ws9-base` зараз, до першого коміту зрізу 3.** З нього ви візьмете двійник:

   ```powershell
   git tag ws9-base
   git push origin ws9-base
   ```

   Не плануєте бонус — тег усе одно не завадить.

**Доказ:** зелений `smoke` на `main`; `/api/health` з `"db":"ok"` і SHA `main`.

## 1. Вимоги й спека зрізу 3 (2–3 год) — трек A

Як у WS7, кроки 3–6, лише для зрізу 3. Промпт — [F01](prompts.md#f01).

1. **`/requirements-sync`** у чистому дереві → звіт агента → PR «Оновлення вимог» → `@coderabbitai review` → зелений
   CI → Squash and merge. Прочитайте `requirements/` уважно: вимоги зрізу 3, матриця прав, roadmap.
2. **Питання PM — бажано до кінця Чт 05.11**, бо PM відповідає й змінює вимоги до кінця Пт 06.11. Форма «Питання до
   вимог» у репо PM; спершу перегляньте наявні: `gh issue list --repo <pm>/<team-slug>-spec --label question --state all`.
   Найцінніші питання:
   - «Тоді», якого не видно на екрані (UAT-запис його не доведе);
   - дія зрізу 3, якої немає в матриці прав, або відмова без «що не змінилося»;
   - порожній стан чи помилка без дослівного тексту;
   - суперечність між двома AC.

   F01 складає питання чернетками; публікуєте їх ви.
3. **Спека `add-<зріз3>`** — гілка `spec/add-<зріз3>`: кожен AC PM — `#### Scenario` з тим самим ID і дослівним
   «Тоді»; `npx openspec validate add-<зріз3> --strict`; скіл `spec-review`; archive у гілці; рядки
   `docs/conformance.md` для кожного AC зрізу 3 → PR «Специфікація». Промпт для `/opsx:propose` — як у
   [WS7, крок 5](https://github.com/koldovsky/2026-quitcode-07-fullstack-hw/blob/main/docs/walkthrough-dev.md#5-специфікація-add-зріз2-45-хв--1-год--трек-a),
   з «зрізом 3» замість «зрізу 2».
4. **План `impl-<зріз3>`** — `design.md` з трьома розділами (UI і компоненти · Дані й RLS · Auth) і `tasks.md` з
   вертикальними задачами «Done when: AC-…», як у
   [WS7, крок 6](https://github.com/koldovsky/2026-quitcode-07-fullstack-hw/blob/main/docs/walkthrough-dev.md#6-план-impl-зріз2-45-хв--трек-c).
   Окремим пунктом кожної задачі — тести на її AC і доказ, що тест відмови здатен упасти.
5. **Після уточнень PM (Сб 07.11 або пізніше):** `/requirements-sync` ще раз → PR «Оновлення вимог». Якщо AC змінено
   — зміна специфікації `clarify-<зріз3>` (MODIFIED / ADDED / REMOVED), як у
   [WS7, крок 10](https://github.com/koldovsky/2026-quitcode-07-fullstack-hw/blob/main/docs/walkthrough-dev.md#10-після-уточнень-pm-3045-хв--трек-a).
   Якщо агент відповів «Вимоги актуальні» — рядок `/requirements-sync <YYYY-MM-DD>: Вимоги актуальні` у файлі здачі.

**Доказ:** PR «Оновлення вимог»; PR «Специфікація» з `add-<зріз3>` і планом; питання в репо PM (або «питань не
було»); `npm run ac:coverage` без попереджень «не перекладено» для AC зрізу 3.

## 2. Реалізація зрізу 3 (6,5–8 год) — трек B

Одна задача з `tasks.md` — одна гілка `task/<коротко>` — один PR «Реалізація». Рецепти міграцій, політик і перевірок
на сервері — у [`auth-guide.md` WS7](https://github.com/koldovsky/2026-quitcode-07-fullstack-hw/blob/main/docs/auth-guide.md).

1. **Тест спершу, в окремій сесії.** На кожен AC задачі — тест з тексту AC на правильному рівні (промпт
   [F02](prompts.md#f02) у **новій** сесії агента, до коду):
   - розрахунки й межі — unit;
   - «хто не може» на рівні даних — тест бази на PGlite (`asUser` / `asAnon`), а не мок `rpc`;
   - те, що видно на екрані, — UAT-специфікація (крок 3).

   Таблиця рівнів — у [гайді з тестів WS8](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/testing-guide.md#який-рівень-для-якого-ac).
   Тест червоний — добре: реалізації ще немає.
2. **Реалізація задачі** у вашій звичайній сесії: міграція (`revoke` + явні GRANT, політика RLS на кожну операцію,
   column grants), Server Action чи Route Handler з `getClaims()` і роллю з `profiles` до запису, сторінка з текстами
   з AC. Тест стає зеленим.
3. **Доказ, що тест відмови здатен упасти** — для кожного відмовного AC одне з трьох, як у WS7–WS8: червоний прогін
   без політики чи перевірки; убитий мутант Stryker (`--mutate <файл>`); SQL-мутант (`npm run mutation:sql` →
   `KILLED`). Промпти — [WS8 P04, P03, P05](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/prompts.md#p04).
4. **Перевірки перед PR:** `npm run lint`, `npm run typecheck`, `npm test`, `npm run build`, `npm run ac:coverage`,
   `npm run spec:check`. Рядки `docs/conformance.md` для AC задачі — у тому самому PR.
5. **PR «Реалізація»:** в описі — пункт `tasks.md`, AC, таблиця «AC → тест → рівень → доказ, що падає», джерело
   запозичених файлів. `@coderabbitai review` → відповідь на кожну знахідку → зелені обов'язкові перевірки (`CI`,
   «Ворота: fallow audit», «Покриття: доменна підлога») → Squash and merge → **зелений `db-deploy`**.
6. Нова роль у зрізі 3? Додайте її в `supabase/demo-users.json` (крок 5).

**Доказ:** усі задачі зрізу 3 злито; тест з AC ID на кожен AC; доказ, що тести відмов здатні впасти; зелені `quality`
і `db-deploy`.

## 3. UAT на кожен AC (3–4 год) — трек C

Конвеєр UAT у вас уже є з WS8 (`npm run uat`, `scripts/uat/`, `tests/e2e/support/`). Нового встановлювати нічого.
Якщо UAT у WS8 не ставили — [WS8 walkthrough-dev, крок 5.1](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/walkthrough-dev.md#51-playwright-і-матеріали)
з `$B` цього репо: `materials/uat/` тут ті самі. Правила вердикту, вартість і приватність —
[UAT-гайд WS8](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/uat-guide.md). Гілка `task/uat-<зріз3>`.

1. **Ролі зрізу 3:** у `tests/e2e/auth.setup.ts` додайте в `ROLES` нові ролі й адреси акаунтів на проді (крок 5).
2. **Специфікація на кожен AC** — один AC, один `test` у `tests/e2e/<область>.spec.ts`. Промпт — [F03](prompts.md#f03).
   Що нового порівняно з WS8:
   - **кожен** AC зрізу 3, а не 3 на вибір;
   - AC-відмова «напряму, оминаючи сторінки» — тест PGlite з кроку 2 плюс крок UAT, що показує на екрані «що не
     змінилося»;
   - AC, що змінює дані, — `@writes`, `uatEmail()` і `deleteUatUsers(…)`; таблиці з FK на користувача без `on
     delete cascade` — в `OWNED_ROWS` (`tests/e2e/support/admin.ts`). Безпечно прибрати не виходить — AC іде в «Не
     перевірено» proof pack з причиною.
3. **Прогін на проді — ви, у звичайному терміналі**, не агент (команда пише в спільний прод і читає секретний ключ з
   `.env.local`):

   ```powershell
   npm run uat -- --url https://<проєкт>.vercel.app
   ```

   Потім Claude Code пояснює звіт — промпт [F04](prompts.md#f04). Тест упав? Спершу з'ясуйте, чия вада: продукту (→
   виправлення окремим PR), тесту (локатор, другий `alert`) чи формулювання AC (→ питання PM). Твердження «під
   продукт» не підганяйте, вердикт руками не правте.
4. **Негативний контроль NC-1** — промпт [F05](prompts.md#f05); `npm run uat:nc -- --url https://<проєкт>.vercel.app`
   → FAIL. Якщо файл `tests/e2e/negative-controls.spec.ts` уже є з WS8 — досить нового прогону на AC зрізу 3.
5. **PR:** `docs/qa/uat/<дата>/` (`report.md`, `manifest.json`, `frames/`), специфікації; відео — вкладенням в опис
   PR (не в git). `docs/conformance.md`: кожен AC зрізу 3 — `реалізовано` зі шляхами коду, тесту й специфікації;
   `npm run ac:coverage` зелений. `@coderabbitai review` → злиття.
6. **У чат команди:** «UAT готовий: звіт <посилання>, PR <посилання>, AC зрізу 3: …». Бажано до кінця Вт 10.11: тоді
   PM прийме до кінця Чт 12.11, і у вас лишиться час виправити відхилення.

**Доказ:** специфікація на кожен AC; звіт на SHA з кожним AC зрізу 3; PR з відео й негативним контролем; рядки
`conformance.md`.

## 4. Пакет доказів на кожен AC (45 хв – 1 год) — трек C

`docs/qa/proof-pack.md` за [шаблоном WS9](../materials/templates/proof-pack.template.md): рядок на **кожен** AC зрізу 3
і розділ «Не перевірено». Якщо в репо є proof pack WS8 — збережіть його поруч:

```powershell
git switch -c docs/proof-pack-ws9
git mv docs/qa/proof-pack.md docs/qa/proof-pack-ws8.md
curl.exe -fsSL --create-dirs -o docs/qa/proof-pack.md "$B/templates/proof-pack.template.md"
```

Заповнює **нова** сесія агента — промпт [F06](prompts.md#f06): лише з того, що є в репо й у звітах. Перевірте самі:
кожен AC є в таблиці або в «Не перевірено»; кожне посилання веде на конкретний рядок чи SHA; FAIL і NEEDS-HUMAN не
сховано. PR → злиття.

**Доказ:** `docs/qa/proof-pack.md` на SHA з рядком на кожен AC зрізу 3.

## 5. Прод, conformance, ретро (1 год) — трек D

1. **Зріз 3 на проді:** усі PR злито, `db-deploy` зелений, `/api/health` показує SHA `main`. Пройдіть зріз на
   прод-URL під кожною роллю.
2. **Акаунти для PM:** по акаунту `*@example.test` на кожну нову роль зрізу 3 у `supabase/demo-users.json` →
   `npm run users:demo` → рядки в `accounts.txt` → новий `accounts.zip` (схема й команди —
   [гайд D0 WS8](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/deploy-d0.md#тестові-акаунти-accountszip)).
   У чат команди: «Зріз 3 на проді: <прод-URL>. Нові ролі: …; `accounts.zip` оновлено». Пароль — лише в архіві.
3. **`docs/conformance.md`** — рядок на кожен AC зрізу 3 з чесним статусом; `npm run ac:coverage` зелений.
4. **Ретро** до 10 рядків «що б ми взяли з фабрики у свій звичайний процес» — промпт [F14](prompts.md#f14).
   Розділ «Ретро» в proof pack або `docs/qa/retro-ws9.md`. Кожне «візьмемо» — з конкретним файлом чи налаштуванням.
   Ретро пишуть усі, навіть без бонусу: станції фабрики показано на занятті.

**Доказ:** `/api/health` з SHA `main`; `accounts.zip` на SHA; ретро на SHA.

## 6. Відхилення від PM — трек C

PM приймає **кожен** AC зрізу 3 за вашим звітом і в браузері й ставить оцінку 0–3 у таблиці приймання PM. Оцінка — 10
з 35 балів треку C, кожен AC — рівна частка: 2–3 — повна, 1 — половина, 0 — нуль. Рахуємо оцінку **на кінець Нд
15.11**, тож виправлене відхилення повертає бали:

1. На кожну оцінку 0–1 PM відкриває у вашому репо issue «Відхилення від вимог» з кроками, очікуваним і фактичним
   (форма `.github/ISSUE_TEMPLATE/deviation.yml` у вас уже є з WS8; якщо ні — `curl.exe -fsSL --create-dirs -o
   .github/ISSUE_TEMPLATE/deviation.yml "$B/issue-templates/deviation.yml"`).
2. **Ваша вада** — PR з тестом, що відтворює відхилення (AC ID у назві), і `Fixes #N`; новий прогін UAT для цього AC.
   Промпт — [WS8 P27](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/prompts.md#p27).
   Потім в issue: «Виправлено: <PR>, прогін UAT: <посилання>» — **до кінця Сб 14.11**.
3. **Різне читання вимоги** — коментар з поясненням і питання PM через форму, якщо AC неоднозначний. Спірне вирішує
   викладач за текстом AC.

PM перевіряє повторно до кінця Нд 15.11. Не встиг — перевіряє викладач, балів ви не втрачаєте. Зауваження PM поза
«Тоді» балів не знімають.

## 7. Здача (15 хв)

[`submissions/README.md`](../submissions/README.md): PR з `submissions/<team-slug>/<github-login>.md`, заголовок
`WS9: Ім'я Прізвище`, `gh pr create --web` (без `--fill`), окремим коментарем `@coderabbitai review`. SHA — коміт
`main` вашого репо (`git fetch origin` → `git rev-parse origin/main`).

## Бонус (+10) — фабрика в двійнику

Необов'язково. Основний зріз — пріоритет. Що здати — у [README](../README.md#бонус-10-фабрика-в-двійнику), покроково —
[twin-deploy](twin-deploy.md) (двійник з окремим розгортанням) і [factory-guide](factory-guide.md) (фабрика курсу на
ваших вимогах). Коротко:

| Крок | Бюджет | Промпт |
|---|---|---|
| Запустити першоджерело без ключа (`npm run demo`) | 15–30 хв | [F07](prompts.md#f07) |
| Двійник з тегу `ws9-base`: репо, Supabase, Vercel, ruleset, автозлиття | 1,5–2 год | [F08](prompts.md#f08) |
| Червоні тести AC на гілці `tests-first` — з тексту AC, не з основного репо | 45 хв – 1 год | [F18](prompts.md#f18) |
| Імпортер під формат вашого PM → задача фабрики | 1–1,5 год | [F09](prompts.md#f09) |
| Приховані сценарії з AC (окрема сесія) і печатка набору | 45 хв – 1 год | [F10](prompts.md#f10) |
| Перевірити задачу і схвалити намір (і план) | 20–30 хв | [F11](prompts.md#f11) |
| Прогін фабрики: ворота → пакет доказів → PR і автозлиття → деплой двійника | 1–2 год + час моделі | [F12](prompts.md#f12), [F13](prompts.md#f13) |
| UAT на кожен AC на проді двійника | 30–45 хв | [F03](prompts.md#f03), [F04](prompts.md#f04) |
| Звіт порівняння «звичайно проти фабрики» | 45 хв – 1 год | [F15](prompts.md#f15) |

Ліміти закінчилися на середині — зупиніться й напишіть чесний звіт: що фабрика встигла, де зупинилась і чому. Він
теж зараховується.

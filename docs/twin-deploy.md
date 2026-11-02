# Двійник проєкту: окреме розгортання для фабрики

Бонус WS9. Двійник — окрема копія вашого проєкту зі своїм репо, базою й хостингом, де фабрика зливає й розгортає
сама. Автозлиття й `db-deploy` фабрики ніколи не торкаються основного проду.

Промпт перевірки — [F08](prompts.md#f08); як потім запустити фабрику — [factory-guide](factory-guide.md). Бюджет —
**1,5–2 год**. Курс-редакція фабрики називає двійник «близнюком» (`docs/uk/twin.md`) — це те саме.

## Навіщо окреме розгортання

У двійнику фабрика **повністю автономна**: після воріт вона сама вмикає автозлиття PR, зокрема з міграціями, а злиття в
`main` запускає `db-deploy` у базу й деплой у Vercel. Якби це було ваше основне репо, неперевірена людиною міграція
пішла б у прод, яким користується PM. Тому:

| | Основний проєкт | Двійник |
|---|---|---|
| Репо | `<dev>/<team-slug>` | `<dev>/<team-slug>-factory` |
| База | ваш Supabase | **свій** Supabase-проєкт |
| Хостинг | ваш Vercel | **свій** Vercel-проєкт |
| Хто зливає | ви, як у WS6–WS8 | фабрика (автозлиття після воріт і required checks) |
| Зріз 3 | звичайним способом | фабрика, з тих самих AC |

Двійник — з коміту основного репо **до** початку зрізу 3 (тег `ws9-base`). Код і тести звичайної реалізації в двійник не
переносимо: інакше фабриці нічого будувати, а порівняння втрачає сенс.

**Не плутати** з тестовим двійником WS8 (підміна залежності в тестах): тут двійник — це цілий проєкт.

## Що потрібно

- Тег `ws9-base` в основному репо ([walkthrough-dev](walkthrough-dev.md#0-підготовка-й-d0-3045-хв), пункт 5). Якщо забули і зріз 3 уже почато — поставте тег на
  останній коміт `main` **до** першого коміту зрізу 3: `git tag ws9-base <sha>`, `git push origin ws9-base`.
- **Другий безкоштовний проєкт Supabase.** Безкоштовний план має обмеження на кількість активних проєктів; основний
  проєкт займає один слот, двійник — другий. Якщо слоту немає — бонус не робимо за рахунок основного проєкту
  (основний не видаляйте й не призупиняйте).
- Vercel (той самий акаунт, що й для основного), GitHub CLI `gh` з `gh auth login`, Supabase CLI (`npx supabase` з
  проєкту).
- Менеджер паролів для пароля бази й ключів двійника.

## Двійник крок за кроком

Створюєте хмарні проєкти й вставляєте ключі **ви** — у вебінтерфейсі, `.env.local` і секретах. Агенту (F08) — лише
перевірка й чек-лист, без значень.

### Репо з тегу `ws9-base`

```powershell
gh repo create <dev>/<team-slug>-factory --public --description "Двійник для фабрики (бонус WS9); прод — окремий"
git clone https://github.com/<dev>/<team-slug> C:\pf\twin
cd C:\pf\twin
git switch -C main ws9-base                 # main двійника = ws9-base, без коду зрізу 3
git remote set-url origin https://github.com/<dev>/<team-slug>-factory
git push -u origin main
git push origin ws9-base
```

- **Перевірка:** `git remote -v` — лише `<team-slug>-factory`; `git log -1` — коміт `ws9-base`; у `src/` і `tests/` немає
  нічого зі зрізу 3.
- У README двійника першими рядками: «Двійник для фабрики (бонус WS9); прод — окремий», URL проду двійника і посилання
  на основне репо. Зміни README — звичайним PR (ruleset ще не діє, але звичка та сама).
- Settings → General → Pull Requests: **Allow auto-merge** і **Automatically delete head branches**.

### Supabase двійника

1. Supabase Dashboard → New project, той самий регіон, що й основний. Назва — з `factory` у кінці, щоб не сплутати.
   Пароль бази — одразу в менеджер паролів.
2. Перевірте `supabase/config.toml`, розділ `[auth]`: `site_url` і `additional_redirect_urls`. У шаблоні курсу там
   `http://localhost:3000`. Якщо ви вписали туди прод **основного** проєкту — змініть на прод двійника (комітом у
   двійник), інакше `config push` запише основний домен у Auth двійника, і посилання з листів та запрошень двійника
   поведуть на основний прод.
3. Налаштування Auth — як в основному (реєстрацію вимкнено, підтвердження пошти вимкнено):

   ```powershell
   cd C:\pf\twin
   npx supabase link --project-ref <ref двійника>
   npx supabase config push
   ```

   `supabase link` пише ref у `supabase/.temp/` (у `.gitignore`). Перевірте, що це ref **двійника**: Dashboard двійника →
   Project Settings → General.
4. **Міграції руками не застосовуйте** (`supabase db push` — ні). Вони йдуть у базу лише через CI `db-deploy`. Перший раз
   — вручну з Actions, коли буде секрет (розділ GitHub нижче): Actions → CI → Run workflow, гілка `main`, `include_seed`
   увімкнено. У лозі `db-deploy` мають бути рядки «Applying migration …» і «Finished supabase db push».
5. Ключі двійника (Project Settings → API Keys) — у `C:\pf\twin\.env.local` за `.env.example`: `NEXT_PUBLIC_SUPABASE_URL`,
   `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`, `SUPABASE_SECRET_KEY`, `DEMO_USER_PASSWORD`.

### Vercel двійника

1. Vercel → Add New → Project → імпорт `<dev>/<team-slug>-factory`. **Framework Preset: Next.js.** Якщо створювати з CLI
   (`vercel project add`), Vercel ставить «Other», і перша збірка падає з «No Output Directory named public» — змініть
   на Next.js у Settings → Build and Deployment.
2. Settings → Environment Variables — значення **двійника**:
   - `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` — Production і Preview;
   - `SUPABASE_SECRET_KEY` — **лише Production**.
3. Settings → Deployment Protection: Vercel Authentication для проду вимкнено — інакше UAT і `/api/health` бачать
   сторінку входу Vercel.
4. Redeploy після змінних. Запишіть URL проду двійника: `https://<проєкт-двійника>.vercel.app`.

Якщо прив'язуєте локально `vercel link` — він дописує рядки в `.env.local` і може змінити `.gitignore`; перевірте
`git status` і не комітьте `.vercel/` (у шаблоні курсу він уже в `.gitignore`).

### GitHub: автозлиття, ruleset, environment `production`

1. **Environment `production`:** Settings → Environments → New environment `production`; Deployment branches — лише `main`;
   секрет **`SUPABASE_DB_URL`** — рядок підключення Session pooler двійника (Dashboard двійника → Connect → Session pooler,
   порт 5432; пароль у рядку — percent-encoded). Його читає лише job `db-deploy`.
2. **Змінна репо `PROD_URL`** = URL проду двійника: Settings → Secrets and variables → Actions → Variables (або
   `gh variable set PROD_URL --repo <dev>/<team-slug>-factory --body https://<проєкт-двійника>.vercel.app`). Без неї
   `smoke.yml` перевіряє адресу за замовчуванням з коду — тобто **ваш основний прод**, а не двійник.
3. Інші секрети CI з основного репо — лише ті, що не ведуть до основного проду (наприклад `SPEC_DENYLIST` для перевірки
   спеки). Жодних ключів основного Supabase.
4. **Ruleset** — Settings → Rules → Rulesets → New branch ruleset, ціль `main`:
   - **Require a pull request before merging**, 0 обов'язкових затверджень (затверджує фабрика воротами й рівнем
     автономії);
   - **Require status checks to pass:** `spec`, `app`, `conformance`, `db-check`;
   - **Block force pushes**, **Restrict deletions**.
5. **Allow auto-merge** (якщо не ввімкнули в кроці «Репо»).

Ruleset — головна сітка безпеки після автономного злиття: CI заново проганяє тести, лінтер, збірку й порядок міграцій з
чистого checkout, і GitHub не зіллє PR, доки required checks не зелені. **Без required checks автозлиття GitHub зливає
PR одразу**, ще до CI. Чекають лише обов'язкові перевірки: дорадчі (fallow, порада щодо залежностей) можуть бути
червоними — PR однаково зіллється (так було з fallow у PR фабрики на демо-двійнику).

Перевірка (лише читання): `gh api repos/<dev>/<team-slug>-factory --jq .allow_auto_merge` → `true`;
`gh api repos/<dev>/<team-slug>-factory/rulesets` — ruleset активний; `gh api repos/<dev>/<team-slug>-factory/environments/production/secrets
--jq '.secrets[].name'` → `SUPABASE_DB_URL`.

### Демо-користувачі й `/api/health`

1. Після першого `db-deploy` (міграції + seed):

   ```powershell
   cd C:\pf\twin; npm ci
   npm run users:demo
   ```

   Створіть ті самі акаунти **тим самим способом, що в основному проєкті**: рядки `supabase/demo-users.json` →
   `npm run users:demo` (пароль — `DEMO_USER_PASSWORD` з `.env.local` двійника) або запрошенням, якщо так заведено у
   вас. Щоб PM могли зайти на прод двійника тими самими даними, що в `accounts.zip` (бонус PM), візьміть той самий
   `DEMO_USER_PASSWORD`, що в основному. Пароль архіву курсу — **не** пароль акаунтів. Значення — лише в `.env.local`.
2. `https://<проєкт-двійника>.vercel.app/login` — 200; `…/api/health` → `"ok":true`, `"db":"ok"` і `sha` = останній коміт
   `main` двійника. Саме `sha` фабрика (`F uat`) порівнює з комітом злиття перед UAT.
3. UAT з WS8 працює на двійнику до фабрики: `npm run uat -- --url https://<проєкт-двійника>.vercel.app` проходить на
   наявних AC зрізів 1–2.

## Секрети: де живуть і де ніколи

| Де | Що | Хто бачить |
|---|---|---|
| `C:\pf\twin\.env.local` (у `.gitignore`) | ключі Supabase **двійника**, `DEMO_USER_PASSWORD` | ви; хук фабрики не дає будівельнику читати `**/.env*` |
| GitHub, environment `production` | `SUPABASE_DB_URL` двійника | лише job `db-deploy` на `main` |
| Vercel двійника | `NEXT_PUBLIC_*`, `SUPABASE_SECRET_KEY` (лише Production) | збірка й сервер |
| менеджер паролів | пароль бази двійника, `FACTORY_HOLDOUT_KEY` | ви |

**Ніколи:** у git, у задачах, прихованих сценаріях, пакетах доказів, описах PR, у чаті з агентом, у файлі здачі. Ключі
основного проєкту в двійник не копіюйте: у двійника — **свої**. `FACTORY_HOLDOUT_KEY` до двійника не стосується: він
лише у вашому терміналі під час прогону.

## Перевірка: двійник не бачить основного проду

Дайте агенту [F08](prompts.md#f08) — він порівнює **назви**, не значення. Самі перевірте:

- [ ] `git -C C:\pf\twin remote -v` — лише `<team-slug>-factory`;
- [ ] `supabase/.temp/project-ref` двійника = ref двійника (Dashboard), не основного;
- [ ] у Vercel двійника `NEXT_PUBLIC_SUPABASE_URL` містить ref двійника;
- [ ] змінна `PROD_URL` двійника = URL проду двійника;
- [ ] `.mcp.json` читає `SUPABASE_PROJECT_REF` зі змінної середовища: у терміналі двійника вона або не задана, або
      = ref двійника (інакше Supabase MCP у Claude Code працюватиме з основною базою);
- [ ] `/api/health` двійника → `sha` з історії двійника; прод-URL двійника ≠ прод-URL основного (CodeRabbit перевіряє це
      у файлі здачі);
- [ ] у фабриці `FACTORY_TARGET` = `C:\pf\twin`, `delivery.uatUrl` = прод двійника.

## Типові пастки

| Симптом | Причина | Що робити |
|---|---|---|
| PR фабрики злився за секунди, ще до CI | немає ruleset з required checks | ruleset (розділ GitHub); перевірте наступний PR |
| `gh pr merge --auto` відмовляє | Allow auto-merge вимкнено або немає required checks | Settings → General; ruleset |
| `db-deploy` зелений, а нової таблиці чи функції в базі немає | немає секрету `SUPABASE_DB_URL` в environment `production` — job **мовчки пропускає** міграції (notice «db push пропущено») | додайте секрет, перезапустіть CI на `main`; у лозі — «Applying migration …» |
| `smoke` на двійнику червоний чи перевіряє не той прод | змінну `PROD_URL` не задано — `smoke.yml` бере адресу з коду | `gh variable set PROD_URL …` (розділ GitHub) |
| перша збірка Vercel: «No Output Directory named public» | Framework Preset «Other» | Next.js у Settings → Build and Deployment |
| UAT чи `/api/health` показують сторінку входу Vercel | Deployment Protection увімкнено | вимкніть для проду двійника |
| `db-deploy` падає на першому запуску | seed не застосовано, або міграцію застосовано руками раніше | перший запуск — Run workflow з `include_seed`; руками `db push` не робимо |
| UAT: вхід не вдається | демо-користувачів не створено або інший пароль | `npm run users:demo` з `.env.local` двійника |
| посилання з листа чи запрошення двійника веде на основний прод | `site_url` / `additional_redirect_urls` у `supabase/config.toml` — основний домен, і `config push` записав його в Auth двійника | прод двійника в `config.toml` двійника, знову `npx supabase config push` |
| `Filename too long` у фабриці | шляхи > 260 символів | `git config core.longpaths true`, короткі `C:\pf\…` |
| fallow у PR фабрики: «unused file» `scripts/factory-tap.mjs` | скрипт запускають руками | `"scripts/factory-tap.mjs"` у `entry` `.fallowrc.jsonc` (factory-guide, червоні тести) |
| JSON задачі «не читається» | PowerShell 5.1 `>` пише UTF-16 | `--out <файл>` імпортера |
| `doctor`: `agent … shell shim` | `claude` встановлено через npm | нативний `claude.exe` |
| прод двійника не відповідає через кілька днів | безкоштовний проєкт Supabase призупинено через неактивність | Dashboard двійника → Restore; потім `/api/health` |

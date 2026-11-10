# Здача WS9

## Дані

- Роль: dev
- Команда: property-workflows
- GitHub-логін: OleksandrMartynov
- Ваше репо: https://github.com/OleksandrMartynov/property-workflows
- SHA: 51a0b42e3e423bccf6e90189d85b916cf9410d4a
- Репо вимог PM: https://github.com/solomchaak/property-workflows-spec
- Прод-URL: https://property-workflows-rho.vercel.app

## PM

—

## Розробник

### D0 — умова прийняття

- `/api/health`: https://property-workflows-rho.vercel.app/api/health — `"db":"ok"`, `"sha":"51a0b42e3e423bccf6e90189d85b916cf9410d4a"`
- Зелений прогін `smoke` на `main`: https://github.com/OleksandrMartynov/property-workflows/actions/runs/42632502465
- `accounts.zip` на SHA: https://github.com/OleksandrMartynov/property-workflows/blob/51a0b42e3e423bccf6e90189d85b916cf9410d4a/accounts.zip
- Ролі тестових акаунтів: менеджер — pm-manager@example.test; адміністратор — pm-admin@example.test; підрядник — pm-vendor@example.test (паролі — лише в accounts.zip)
- Версія `next`: next@16.4.3

### Зріз 3 звичайним способом

- AC зрізу 3: AC-VENDOR-01, AC-VENDOR-02, AC-VENDOR-03, AC-VENDOR-04, AC-VENDOR-05, AC-VENDOR-06, AC-VENDOR-07, AC-VENDOR-08, AC-VENDOR-09, AC-VENDOR-10
- `/requirements-sync` — PR «Оновлення вимог» (A): https://github.com/OleksandrMartynov/property-workflows/pull/43
- Спека `add-<зріз3>` і план — PR «Специфікація» (A): https://github.com/OleksandrMartynov/property-workflows/pull/46 (спека `add-vendor`), https://github.com/OleksandrMartynov/property-workflows/pull/47 (план `impl-vendor`); дослівне «Тоді» PM у сценаріях AC-VENDOR-01…10 — https://github.com/OleksandrMartynov/property-workflows/pull/83
- Після уточнень PM (A): /requirements-sync 2026-11-09: Вимоги актуальні
- Питання PM (A): https://github.com/solomchaak/property-workflows-spec/issues/45, https://github.com/solomchaak/property-workflows-spec/issues/46
- PR задач «Реалізація» (B): https://github.com/OleksandrMartynov/property-workflows/pull/49, https://github.com/OleksandrMartynov/property-workflows/pull/50, https://github.com/OleksandrMartynov/property-workflows/pull/59, https://github.com/OleksandrMartynov/property-workflows/pull/84, https://github.com/OleksandrMartynov/property-workflows/pull/85, https://github.com/OleksandrMartynov/property-workflows/pull/86, https://github.com/OleksandrMartynov/property-workflows/pull/87
- Тести відмов здатні впасти (B): AC-VENDOR-03 → V03 KILLED; AC-VENDOR-04 → V04 KILLED; AC-VENDOR-06 → V06 KILLED; AC-VENDOR-07 → V07 KILLED; AC-VENDOR-08 → V08 KILLED і V08t (TS) KILLED; AC-VENDOR-09 → V09 KILLED — SQL-мутанти `npm run mutation:sql`, таблиця «мутант → тест, що впав» у https://github.com/OleksandrMartynov/property-workflows/pull/84
- Ворота й `db-deploy` на `main` (B): зелений quality https://github.com/OleksandrMartynov/property-workflows/actions/runs/42632411941; зелений db-deploy (CI) https://github.com/OleksandrMartynov/property-workflows/actions/runs/42632411782
- UAT — специфікації (C): https://github.com/OleksandrMartynov/property-workflows/blob/51a0b42e3e423bccf6e90189d85b916cf9410d4a/tests/e2e/vendor.spec.ts (на кожен AC-VENDOR-01…10)
- UAT — звіт на проді (C): https://github.com/OleksandrMartynov/property-workflows/blob/51a0b42e3e423bccf6e90189d85b916cf9410d4a/docs/qa/uat/2026-11-09/report.md — 13/13 PASS (AC-VENDOR-01…10 — усі PASS)
- UAT — PR зі звітом і відео (C): https://github.com/OleksandrMartynov/property-workflows/pull/88 (відео на кожен AC — вкладенням в описі)
- UAT — негативний контроль (C): https://github.com/OleksandrMartynov/property-workflows/pull/88 — `npm run uat:nc`: NC-1 → FAIL (R1), NC-2 → NEEDS-HUMAN (R3)
- QA proof pack (C): https://github.com/OleksandrMartynov/property-workflows/blob/51a0b42e3e423bccf6e90189d85b916cf9410d4a/docs/qa/proof-pack.md
- Не перевірено (C): жоден AC не лишився без доказу; застереження — AC-VENDOR-01…10 перевірено на синтетичних запусках і підрядниках `uat-*@example.test` (UAT пише в спільну базу, тож демо-акаунти PM не змінюємо), а для AC-VENDOR-02 і 04 «жодних інших даних профілю» — автоматичні перевірки, бо на кадрі не видно того, чого немає
- Відхилення від PM (C): не надходило
- Рядки `docs/conformance.md` (D): https://github.com/OleksandrMartynov/property-workflows/blob/51a0b42e3e423bccf6e90189d85b916cf9410d4a/docs/conformance.md — AC-VENDOR-01…10 «реалізовано» з кодом, тестами й `tests/e2e/vendor.spec.ts (UAT)`
- Акаунти для нових ролей зрізу 3 (D): підрядник — pm-vendor@example.test, https://github.com/OleksandrMartynov/property-workflows/pull/59
- Ретро «що взяти з фабрики» (D): https://github.com/OleksandrMartynov/property-workflows/blob/51a0b42e3e423bccf6e90189d85b916cf9410d4a/docs/qa/retro-ws9.md

## Бонус

### Фабрика в двійнику (розробник)

- Репо двійника: https://github.com/OleksandrMartynov/property-workflows-factory
- SHA двійника: 02367c24ebd1eb92e94b5cf5312c4b5bbfed7ca9
- Прод двійника: https://2026-quitcode-09-software-factory-h.vercel.app — https://2026-quitcode-09-software-factory-h.vercel.app/api/health (`"sha":"02367c24ebd1eb92e94b5cf5312c4b5bbfed7ca9"`, `"db":"ok"`)
- Тег `ws9-base` в основному репо: https://github.com/OleksandrMartynov/property-workflows/tree/ws9-base
- Фабрика й імпортер: курс-редакція `koldovsky/2026-quitcode-09-factory` (`1280bc6`), fork https://github.com/OleksandrMartynov/2026-quitcode-09-factory, адаптер імпортера https://github.com/OleksandrMartynov/2026-quitcode-09-factory/commit/c35fc80076e3f2adff2edc6373adf602be877619 (гілка `team/property-workflows`: префікс заголовка «Requirement:» + тест на знімку вимоги PM)
- Формат вимог PM: issues «Вимога» в репо PM, після `/requirements-sync` — `requirements/39-assign-a-vendor-own-tasks.md`, рядки `- AC-VENDOR-NN: Given … When … Then …` (`--format lines`, парсер без змін)
- Червоні тести (`tests-first`): https://github.com/OleksandrMartynov/property-workflows-factory/tree/ws9-red (AC-VENDOR-01, 02, 09); далі по задачі: https://github.com/OleksandrMartynov/property-workflows-factory/tree/ws9-red-vendor-tasks і https://github.com/OleksandrMartynov/property-workflows-factory/tree/ws9-red-vendor-tasks-2 (AC-VENDOR-04, 05, 07), https://github.com/OleksandrMartynov/property-workflows-factory/tree/ws9-red-vendor-runs (AC-VENDOR-03, 06, 10), https://github.com/OleksandrMartynov/property-workflows-factory/tree/ws9-red-vendor-templates (AC-VENDOR-08), https://github.com/OleksandrMartynov/property-workflows-factory/tree/ws9-red-vendor-picker-email (AC-VENDOR-02 — вада з UAT), https://github.com/OleksandrMartynov/property-workflows-factory/tree/ws9-red-vendor-last-required (AC-VENDOR-05 — вада з e2e/UAT)
- Приховані сценарії: 6 наборів, 46 сценаріїв, усі запечатані (`*.json.enc` лише в теці контролю): vendor-db 11 (AC-VENDOR-01 — 3, 02 — 4, 09 — 4), vendor-tasks 12 (04, 05, 07 — по 4), vendor-runs 11 (03 — 4, 06 — 4, 10 — 3), vendor-templates 5 (08), vendor-picker-email 3 (02), vendor-last-required 4 (05); кожен набір додатково прогнано на реалізації основного репо — зелені
- AC, які доводить лише UAT: немає (кожен AC має Vitest/PGlite-тест у задачі фабрики; UAT — на кожен AC)
- Прогони фабрики: run-20261109170435-f64016 — needs-human (CLI без входу, $0); run-20261109171206-969078 — released; run-20261109173709-7b2fe3 — needs-human (mutation, 3/3); run-20261109175104-a3083c — released; run-20261109181518-708621 — released; run-20261109185757-05d653 — released; run-20261109191512-de86a1 — released; run-20261109193717-11437f — released
- Пакети доказів: `575a832a41d58d1e4a189983ef9e6a4ea9841efd5bcbf0bc714c0b48e0329ad1` (vendor-db), `fdbe029adac41120f397706d7de1876683a129e5cf470c78eb8ec176734b0eda` (vendor-tasks), `9de95fb4ec4e145c7317f6050c080837c44be3394b80f43f8b6c35ecd68097f2` (vendor-runs), `8ea42bfebc0555666e59da90010b3c227f8631efe1bc50ba7e09aa06f4ee6713` (vendor-templates), `e506ad78b0087f8658745baa971693c6afe6b8e2702d691c9e8d4a29da980a67` (vendor-picker-email), `66172268a763772f1eacba64740d926de74112e621e4a652a4b5331fe0819194` (vendor-last-required) — описи PR і https://github.com/OleksandrMartynov/property-workflows-factory/tree/02367c24ebd1eb92e94b5cf5312c4b5bbfed7ca9/docs/factory
- PR фабрики з автозлиттям: https://github.com/OleksandrMartynov/property-workflows-factory/pull/3, https://github.com/OleksandrMartynov/property-workflows-factory/pull/4, https://github.com/OleksandrMartynov/property-workflows-factory/pull/5, https://github.com/OleksandrMartynov/property-workflows-factory/pull/6, https://github.com/OleksandrMartynov/property-workflows-factory/pull/7, https://github.com/OleksandrMartynov/property-workflows-factory/pull/8 — злиті GitHub після `spec`, `app`, `conformance`, `db-check`; db-deploy двійника https://github.com/OleksandrMartynov/property-workflows-factory/actions/runs/42663777423, https://github.com/OleksandrMartynov/property-workflows-factory/actions/runs/42670135006, https://github.com/OleksandrMartynov/property-workflows-factory/actions/runs/42675928949, https://github.com/OleksandrMartynov/property-workflows-factory/actions/runs/42679436752, https://github.com/OleksandrMartynov/property-workflows-factory/actions/runs/42682526156, https://github.com/OleksandrMartynov/property-workflows-factory/actions/runs/42686021777
- UAT на проді двійника: https://github.com/OleksandrMartynov/property-workflows-factory/blob/02367c24ebd1eb92e94b5cf5312c4b5bbfed7ca9/docs/qa/uat/2026-11-09/report.md — AC-VENDOR-01…10 PASS (фінальний прогін на a56e1dd); історія прогонів 1–4 (NEEDS-HUMAN AC-VENDOR-02, FAIL AC-VENDOR-05, FAIL AC-VENDOR-09 — тест) — https://github.com/OleksandrMartynov/property-workflows-factory/tree/02367c24ebd1eb92e94b5cf5312c4b5bbfed7ca9/docs/qa/uat/2026-11-09-history
- Звіт порівняння: https://github.com/OleksandrMartynov/property-workflows-factory/blob/02367c24ebd1eb92e94b5cf5312c4b5bbfed7ca9/docs/qa/factory-comparison.md
- Втручання людини й ліміти: approve config ×2, approve intent ×6, approve plan ×4, resume ×5, answer 0; ручні коміти — 7 «тести спершу» + 1 доданий тест після FAIL mutation, контракт і nonFunctionals кожної задачі, обгортка агента для macOS (вхід CLI через Keychain), перезапуск CI на https://github.com/OleksandrMartynov/property-workflows-factory/pull/5 (GitHub не створив jobs), `template:ac` на проді двійника, людські PR https://github.com/OleksandrMartynov/property-workflows-factory/pull/1, https://github.com/OleksandrMartynov/property-workflows-factory/pull/2, https://github.com/OleksandrMartynov/property-workflows-factory/pull/9; вартість фабрики $3.09 за 8 прогонів, лімітів вистачило

### PM

—

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

—

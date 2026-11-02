# Матеріали фабрики (бонус WS9)

Сама фабрика тут не лежить: беріть **курс-редакцію**
[`koldovsky/2026-quitcode-09-factory`](https://github.com/koldovsky/2026-quitcode-09-factory) (ліцензія PolyForm
Noncommercial 1.0.0; дозвіл автора для учасників курсу — у її README). Як встановити й запустити —
[`docs/factory-guide.md`](../../docs/factory-guide.md); двійник з окремим розгортанням —
[`docs/twin-deploy.md`](../../docs/twin-deploy.md).

| Файл тут | Що це | Куди |
|---|---|---|
| [`comparison-report.template.md`](comparison-report.template.md) | шаблон звіту «звичайно проти фабрики»: прогони, таблиця за AC, метрики («не виміряно» за замовчуванням), втручання, частковий прогін | `docs/qa/factory-comparison.md` двійника або основного репо |
| [`bulk-db.example.json`](bulk-db.example.json) | **зразок** заповненої задачі фабрики з демо-двійника: вимога PM «масове рішення менеджера», AC-BULK-01…03 | нікуди — лише подивитися |

## Що взяти з курс-редакції

| У курс-редакції | Куди |
|---|---|
| `examples/next-supabase/control/` | `C:\pf\control` — тека контролю **поза** репо двійника |
| `templates/next-vitest/factory-tap.mjs` | `scripts/factory-tap.mjs` двійника (у коміті тестів-спершу на гілці `tests-first`) |
| `tools/ac-to-task.mjs` | лишається в копії фабрики `C:\pf\factory\tools\`; ваш адаптер — там само, у вашому fork курс-редакції |
| `docs/uk/*.md` | швидкий старт, адаптація імпортера, двійник, межі й вартість, Cursor |
| `examples/leave-portal-twin/control/` | control, з яким фабрика справді відпрацювала на демо-двійнику (запис, а не шаблон) |

## Зразок задачі: як читати `bulk-db.example.json`

Це задача, з якою прогін `run-20261102105148-3f6cd9` на
[демо-двійнику](https://github.com/koldovsky/2026-quitcode-09-leave-portal-factory) пройшов усі ворота з першої спроби
(160 с, $0.35; mutation — n/a). Вимога PM — `docs/requirements/02-bulk-decisions.md` репо вимог демо (AC-BULK-01…04);
AC-BULK-04 (стан сторінки) у задачі немає: його доводить UAT після деплою.

| Поле | Звідки | Що помітити |
|---|---|---|
| `requirements[].id` | імпортер | = AC ID PM символ у символ; `description` — дослівно «Дано / Коли / Тоді» |
| `requirements[].verifiedBy` | імпортер | лише `visible:<AC>` (червоний тест Vitest/PGlite) і `holdout:bulk-db` (приховані сценарії) |
| `visibleTests.failToPass` | імпортер | три AC, що мали бути червоними до зміни |
| `visibleTests.passToPass` | імпортер з `--target` | AC «реалізовано» з тестом Vitest — захист від регресії; AC лише з e2e сюди не потрапляє |
| `nonFunctionals` | **людина** | імпортер дав `TBD`; з `TBD` `factory run` відмовив на допуску (5 × «is still undecided», $0) |
| `intent` | імпортер + **людина** | після «Why» і «Source» людина дописала контракт, який фіксують червоні тести: назву й сигнатуру функції бази, шляхи файлів, дослівні повідомлення PM, `data-testid` |
| `protected` | імпортер | тести, скрипти, CI, вимоги, наявні міграції — виконавець їх не змінює |
| `size` | людина (`--size small`) | у демо — `small` на 3 AC, тож етапу плану не було; для бонусу за F09 — `medium` і `approve plan` |

**Не копіюйте цю задачу у свій двійник.** Задачі фабрики, тести й приховані сценарії — про **ваші** AC; перенесені з
демо, вони нічого не доводять про ваш продукт і не зараховуються. Приховані сценарії демо запечатані, ключа немає й не
буде.

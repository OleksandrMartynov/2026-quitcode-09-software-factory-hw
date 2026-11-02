# Матеріали WS9: що куди покласти у вашому репо

Автоматично у ваше репо нічого не приходить. Розробник копіює файли звідси у своє репо `<я>/<team-slug>` (або, для
бонусу, у двійник `<я>/<team-slug>-factory`) у PR відповідного кроку [walkthrough розробника](../docs/walkthrough-dev.md).
В описі PR: «взято з `koldovsky/2026-quitcode-09-software-factory-hw/materials/…`». Тести, специфікації Playwright і
таблиці — про **ваші** AC: їх ви пишете самі.

Матеріали D0, UAT, форма «Відхилення від вимог» і шаблон proof pack — ті самі, що у WS8 (джерело — теги демо-репо
[`koldovsky/2026-quitcode-06-leave-portal-demo`](https://github.com/koldovsky/2026-quitcode-06-leave-portal-demo),
`ws8-03-uat`). Що нового у WS9: UAT і proof pack — на **кожен** AC зрізу 3, а не на 3 AC.

## Як копіювати

## D0: health і smoke

| Тека тут | Що в ній |
|---|---|
| [`d0/`](d0/) | `/api/health`, `smoke.mjs`, `smoke.yml`, тест `classifyDbProbe` |
| [`accounts/`](accounts/) | `zip-accounts.mjs`: `accounts.txt` → `accounts.zip` (AES-256) |
| [`issue-templates/`](issue-templates/) | форма issue «Відхилення від вимог» для PM |

## C: UAT на кожен AC зрізу 3

| Тека тут | Що в ній |
|---|---|
| [`uat/`](uat/) | конвеєр `npm run uat` з WS8: Playwright, рецензент, звіт, кадри, `manifest.json`, негативні контролі, `ac:coverage` |

## Шаблони

| Файл тут | Куди у вашому репо |
|---|---|
| [`templates/proof-pack.template.md`](templates/proof-pack.template.md) | `docs/qa/proof-pack.md` |

## Фабрика (бонус)

[`factory/`](factory/README.md) — наповнюємо в хвилі 2.

## Залежності (точні версії)

## npm-скрипти

## `.gitignore`

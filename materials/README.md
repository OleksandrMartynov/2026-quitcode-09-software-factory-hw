# Матеріали WS9: що куди покласти у вашому репо

Автоматично у ваше репо нічого не приходить. Розробник копіює файли звідси у своє репо `<я>/<team-slug>` (або, для
бонусу, у двійник `<я>/<team-slug>-factory`) у PR відповідного кроку [walkthrough розробника](../docs/walkthrough-dev.md).
В описі PR: «взято з `koldovsky/2026-quitcode-09-software-factory-hw/materials/…`». Тести, специфікації Playwright і
таблиці — про **ваші** AC: їх ви пишете самі.

Матеріали D0, UAT, форма «Відхилення від вимог» і шаблон proof pack — ті самі, що у WS8 (джерело — теги демо-репо
[`koldovsky/2026-quitcode-06-leave-portal-demo`](https://github.com/koldovsky/2026-quitcode-06-leave-portal-demo),
`ws8-03-uat`). Що нового у WS9: UAT і proof pack — на **кожен** AC зрізу 3, а не на 3 AC.

## Як копіювати

Шляхи тут повторюють шляхи у вашому репо: `materials/<блок>/<шлях>` → `<шлях>`. Один цикл на блок:

```powershell
$B = "https://raw.githubusercontent.com/koldovsky/2026-quitcode-09-software-factory-hw/main/materials"
foreach ($f in "scripts/smoke.mjs", ".github/workflows/smoke.yml") {
  curl.exe -fsSL --create-dirs -o $f "$B/d0/$f"
}
```

- `-f` — помилка замість тихого файлу з текстом «404: Not Found»; `--create-dirs` — створює теки.
- **D0 і UAT у вас уже є з WS8** — тоді копіювати нічого не треба. Нове цього тижня одне: шаблон proof pack WS9
  (рядок на кожен AC зрізу 3 і розділ «Не перевірено»).
- Повні списки файлів UAT — у [WS8 walkthrough-dev, крок 5.1](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/docs/walkthrough-dev.md#51-playwright-і-матеріали)
  (з `$B` цього репо).

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

Ті самі, що у WS8: [список з точними версіями](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/materials/README.md#залежності-точні-версії)
(`@playwright/test` 1.64.1, `next` ≥ 16.4.1). Нових залежностей основний трек WS9 не додає.

## npm-скрипти

Ті самі, що у WS8: [`smoke`, `test:e2e`, `uat`, `uat:nc`, `uat:review`, `ac:coverage`](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/materials/README.md#npm-скрипти).

## `.gitignore`

Ті самі рядки, що у WS8: [`accounts.txt`, теки Playwright, `playwright/.auth`](https://github.com/koldovsky/2026-quitcode-08-quality-hw/blob/main/materials/README.md#gitignore).
Звіт UAT (`docs/qa/uat/<дата>/report.md`, `manifest.json`, `frames/`) комітимо; відео, trace і HTML-звіт — ні.

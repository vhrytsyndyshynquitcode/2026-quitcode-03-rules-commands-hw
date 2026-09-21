# Перевірка (Task B, C і бонус E)

Інструмент: **Claude Code** (desktop app, модель Opus 5), Windows 11, Node 22+.
Гілка `ws03/vhrytsyndyshynquitcode`.

## Task B — чи бачить інструмент AGENTS.md

### Спостереження №1 — **до** виправлення (посилання не працює)

На початку сесії, коли `CLAUDE.md` містив ще markdown-посилання
`See [AGENTS.md](./AGENTS.md) for project conventions.`, Claude Code підставив
у контекст блок `<system-reminder>` такого змісту:

> Contents of E:\QuitCode Agentic Engineering Course\hw3\CLAUDE.md (project instructions,
> checked into the codebase):
> `# CLAUDE.md`
> `See [AGENTS.md](./AGENTS.md) for project conventions.`

Тобто агент отримав **сам рядок з посиланням**, а вмісту `AGENTS.md` у контексті
не було. Це прямо підтверджує тезу завдання: markdown-посилання — не імпорт.

### Спостереження №2 — режими застосування правил

У тому ж блоці поруч із `CLAUDE.md` був повний текст **лише**
`.claude/rules/do-not-touch.md` (правило без `paths`). Файлів
`.claude/rules/architecture.md` і `.claude/rules/conventions.md` у контексті
**не було** — у сесії ще не відкривали жодного `app/src/**/*.ts`, під який вони
прив'язані через `paths`. Обидва режими з Task A працюють так, як задумано.

### Спостереження №3 — **після** виправлення (`/context`)

Виправлено: у `CLAUDE.md` тепер окремий рядок `@AGENTS.md`.

- Як перевірив: нова сесія Claude Code → `/context` → розділ **Memory files**.
- Результат: <вставити те, що показав `/context`: перелік файлів у Memory files>

Додатковий контроль (нова сесія, без відкривання файлів): «перелічи команди
проєкту й захищені теки» →
- Результат: <вставити відповідь агента>

## Task C — прогони команд

### `/analyze-error`

- Виклик: `/analyze-error materials/error-log.txt`
- Чи підставився `$ARGUMENTS`: <...>
- Що агент назвав корінною причиною (файл, рядок, механізм): <...>
- Чим відрізнив тригер від причини: <...>
- Чи зупинився там, де сказано в Stop: <...>

### `/refactor`

- Виклик: `/refactor app/src/integrations/sheets-append.ts`
- `npm run check:rules` для цього файлу: до **7** → після <N> (загалом по репо: до **8** → після <N>)
- `npm test` до / після: до **18 passed** / після <...>
- Що змінилось у поведінці (має бути: нічого з того, що фіксують тести): <...>

### `/generate-integration`

- Виклик: `/generate-integration <сервіс>`
- Які файли створено: <...>
- `npm test`, `npm run check:rules`: <...>

## Task E (бонус) — хук

- Файли: <...>
- Спроба змінити `app/src/core/...` → що відповів хук (цитата): <...>

---

### Базова лінія (зафіксована перед роботою)

```
cd app && npm test            → Test Files 6 passed (6), Tests 18 passed (18)
cd app && npm run check:rules → TOTAL: 8 violation(s)
                                src/integrations/sheets-append.ts  7
                                src/sync/state.ts                  1
                                core-untouched                     0
```

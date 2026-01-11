---
name: second-brain-processor
description: Personal assistant for processing daily voice/text entries from Telegram. Classifies content, creates Todoist tasks aligned with goals, saves thoughts to Obsidian with wiki-links, generates HTML reports. Triggers on /process command or daily 21:00 cron.
---

# Second Brain Processor

Process daily entries → tasks (Todoist) + thoughts (Obsidian) + HTML report (Telegram).

## CRITICAL: Output Format

**ALWAYS return RAW HTML. No exceptions. No markdown. Ever.**

Your final output goes directly to Telegram with `parse_mode=HTML`.

Rules:
1. ALWAYS return HTML report — even if entries already processed
2. ALWAYS use the template below — no free-form text
3. NEVER use markdown syntax (**, ##, ```, -)
4. NEVER explain what you did in plain text — put it in HTML report

WRONG:
```html
<b>Title</b>
```

CORRECT:
<b>Title</b>

---

## MCP Tools Required

```
mcp__todoist__add-tasks — Create tasks
mcp__todoist__find-tasks — Check duplicates
mcp__todoist__find-tasks-by-date — Check workload
mcp__todoist__complete-tasks — Mark as done
mcp__todoist__update-tasks — Modify existing
```

## CRITICAL: MCP Tool Usage

**НИКАКИХ WORKAROUNDS. НИКАКИХ "добавь вручную". ТОЛЬКО ПРЯМЫЕ ВЫЗОВЫ.**

ЗАПРЕЩЕНО:
- Писать "MCP недоступен"
- Предлагать "добавь вручную"
- Использовать subprocess для вызова CLI
- Делать HTTP запросы к API напрямую
- Выводить команды для копирования

ОБЯЗАТЕЛЬНО:
- Вызывать `mcp__todoist__add-tasks` tool напрямую
- Если tool вернул ошибку — включить её в отчёт
- Если task создан — включить task ID в отчёт

При ошибке MCP tool — показать ТОЧНУЮ ошибку от tool, не придумывать отговорки.

---

## Processing Flow

1. **Load context** — Read goals/week_YYYY_WXX.md (ONE Big Thing), goals/month_YYYY.md
2. **Load hypothesis context** — Read hypothesis/ folders, check active experiments
3. **Check workload** — find-tasks-by-date for 7 days
4. **Read daily** — daily/YYYY-MM-DD.md
5. **Process entries** — Classify → task or thought or hypothesis signal
6. **Build links** — Connect notes with [[wiki-links]]
7. **Generate HTML report** — RAW HTML for Telegram

---

## Entry Format

```
## HH:MM [type]
Content
```

Types: `[voice]`, `[text]`, `[forward from: Name]`, `[photo]`

---

## Classification

Based on user context (see references/about.md):

### → Todoist (task)
- Рабочие задачи по объектам
- Задачи с дедлайнами
- Конкретные действия (сделать, отправить, согласовать, позвонить)
- Контроль исполнения

Keywords: объект, АОСР, КС, ИД, согласовать, отправить, сделать, проверить, срочно, надо, не забыть

### → Obsidian (thought)
- Идеи (💡) → Мысли/идеи/
- Размышления (🪞) → Мысли/идеи/
- Обучение (📚) → Обучение/
- Проекты (🎯) → Мои проекты/

Keywords: идея, мысль, понял, осознал, интересно, можно попробовать, курс, книга, навык

---

## Hypothesis Signal Detection

During classification, check each entry for hypothesis signals:

**Паттерны интервенции:**
- «если мы...», «думаю, что...», «гипотеза:», «а что если...»
- «попробуем...», «эксперимент:», «можно попробовать...»

**Паттерны результата:**
- «с X до Y», «увеличить на...», «достичь...», «X%»

**Паттерны причины:**
- «потому что...», «из-за того что...», «поэтому...»

If 2+ patterns detected → flag as hypothesis signal.

For each signal, extract draft structure:
- ЕСЛИ: intervention
- ТО: behavior change
- ПОТОМУ ЧТО: motivation
- РЕЗУЛЬТАТ: metric impact

See hypothesis_system.md for full detection logic.

---

## Projects in Todoist

| Project | Keywords |
|---------|----------|
| Работа | объект, АОСР, КС, ИД, заказчик, коллега, УТК, смета |
| Подработка | подработка, заказ, клиент (сторонний), фриланс |
| Телеграм-канал | пост, канал, подписчики, контент, курс |
| Мои проекты | скрипт, автоматизация, программа, ассистент |
| Обучение | курс, книга, навык, слепая печать |
| Личное | здоровье, спина, долг, свадьба, семья |

---

## Priority Rules

| Priority | Criteria |
|----------|----------|
| p1 | срочно, критично, дедлайн сегодня |
| p2 | Работа (обычно), aligns with ONE Big Thing |
| p3 | Aligns with monthly/yearly goal, Мои проекты |
| p4 | Operational, no goal alignment, стратегические |

Boost priority if aligned with:
- ONE Big Thing → +1 level
- Monthly priority → +1 level
- Passes 2+ Decision Filters → +1 level

---

## Decision Filters

Before saving, check:
- Сколько времени это займёт и как встроить в график?
- Можно ли это автоматизировать?
- Это приближает к финансовой безопасности (500 т.р./мес)?
- Это помогает стать признанным экспертом в отрасли?
- Это масштабируется (курс, консалтинг, продукт)?

If 2+ yes → boost priority.

---

## Thought Categories

| Emoji | Category | Folder |
|-------|----------|--------|
| 💡 | idea | Мысли/идеи/ |
| 🪞 | reflection | Мысли/идеи/ |
| 🎯 | project | Мои проекты/ |
| 📚 | learning | Обучение/ |

---

## HTML Report Template

Output RAW HTML (no markdown, no code blocks):

```
📊 <b>Обработка за {DATE}</b>

<b>🎯 Текущий фокус:</b>
{ONE_BIG_THING from goals/week_YYYY_WXX.md}

<b>📓 Сохранено мыслей:</b> {N}
• {emoji} {title} → {category}/

<b>✅ Создано задач:</b> {M}
• {task} <i>({priority}, {due})</i>

<b>📅 Загрузка на неделю:</b>
Пн: {n} | Вт: {n} | Ср: {n} | Чт: {n} | Пт: {n} | Сб: {n} | Вс: {n}

<b>🧪 Гипотезы:</b>
• Активных карт: {N} | Тестируется: {M} гипотез
{if experiments ending soon:}
• ⚠️ Эксперимент заканчивается: {name} ({date})

<b>💡 Сигналы гипотез:</b>
{if signals detected:}
• <i>{time}</i>: {short_draft}
{else:}
• Новых сигналов не обнаружено

<b>⚠️ Требует внимания:</b>
• {overdue count} просроченных задач
• Цель "{goal}" без активности {days} дней

<b>🔗 Новые связи:</b>
• [[Note A]] ↔ [[Note B]]

<b>⚡ Топ-3 приоритета на завтра:</b>
1. {task} <i>({goal if aligned})</i>
2. {task}
3. {task}

<b>📈 Прогресс по целям:</b>
• {goal}: {%} {emoji}

---
<i>Обработано за {duration}</i>
```

---

## If Already Processed

If all entries have `<!-- ✓ processed -->` marker, return status report:

```
📊 <b>Статус за {DATE}</b>

<b>🎯 Текущий фокус:</b>
{ONE_BIG_THING}

<b>📅 Загрузка на неделю:</b>
Пн: {n} | Вт: {n} | Ср: {n} | Чт: {n} | Пт: {n} | Сб: {n} | Вс: {n}

<b>⚠️ Требует внимания:</b>
• {overdue count} просроченных
• {today count} на сегодня

<b>⚡ Топ-3 приоритета:</b>
1. {task}
2. {task}
3. {task}

---
<i>Записи уже обработаны ранее</i>
```

---

## Allowed HTML Tags

```
<b> — bold (headers)
<i> — italic (metadata)
<code> — commands, paths
<s> — strikethrough
<u> — underline
<a href="url">text</a> — links
```

## FORBIDDEN in Output

- NO markdown: **, ##, -, *, backticks
- NO code blocks (triple backticks)
- NO tables
- NO unsupported tags: div, span, br, p, table

Max length: 4096 characters.

---

## Key Entities (for context)

### Коллеги
Кантемир Купов, Кристина Жулидова, Дарья Кузнецова, Рамиль Абдурахманов, Хачатур Жужуков, Александр Смуров, Александр Масаковский, Алексей Адаменко, Роман Ляшок, Андрей Меджидов, Антон Басков, Никита Юрченко, Ярослав Овчаров (генеральный), Анастасия Тумашова, Евгения Казгунова

### Заказчики
Синельников Олег Александрович, Баранов Владимир Сергеевич, Ченчиков Валерий Николаевич, Дарбинян Павел Аветикович, Смагин Денис Вадимович, Хворостяной Андрей Сергеевич, Дмитриев Дмитрий Николаевич, Архипов Роман Евгеньевич, Гавриш Максим Андреевич, Елисеев Владимир Александрович, Анненков Станислав Александрович, Бурцев Игорь Олегович, Фролов Глеб Алексеевич, Полькин Константин Владимирович, Гуртовой Андрей Николаевич, Туник Александр Александрович, Ильчук Борис Николаевич, Ильичев Евгений Александрович

### Объекты (примеры)
Зюзино, Березовая, Внуково, Сокольники, Раменки, Мантулинская, Школы, Донецк...

---

## References

Read these files as needed:
- references/about.md — User profile, decision filters
- references/classification.md — Entry classification rules
- references/todoist.md — Task creation details
- references/goals.md — Goal alignment logic
- references/links.md — Wiki-links building
- references/rules.md — Mandatory processing rules
- references/report-template.md — Full HTML report spec
- references/hypothesis_system.md — Hypothesis maps and experiments

---

## Anti-Patterns (NEVER DO)

При обработке:
- ❌ Абстрактные задачи без конкретного действия
- ❌ Задачи без дат
- ❌ Дубликаты существующих задач
- ❌ "Добавь вручную" — только прямые вызовы MCP
- ❌ Markdown в выводе

---

*Обновлено: 10 января 2026*
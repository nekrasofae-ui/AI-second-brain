# HTML Report Template

## CRITICAL: Output Format

**Return RAW HTML text only. No markdown wrappers.**

WRONG (markdown code block):
```html
<b>Title</b>
```

CORRECT (raw HTML):
<b>Title</b>

Output goes directly to Telegram `parse_mode=HTML`.

---

## Allowed Tags

```
<b> or <strong> — bold
<i> or <em> — italic
<code> — inline code
<pre> — code blocks
<s> or <strike> or <del> — strikethrough
<u> — underline
<a href="url">text</a> — links
```

## FORBIDDEN

- NO markdown: **, ##, -, *, backticks
- NO code blocks with triple backticks
- NO tables (Telegram doesn't support)
- NO unsupported tags: div, span, br, p, table, tr, td

---

## Template

```
📊 <b>Обработка за {DATE}</b>

<b>🎯 Текущий фокус:</b>
{ONE_BIG_THING from goals/week_YYYY_WXX.md}

<b>📓 Сохранено мыслей:</b> {N}
• {emoji} {title} → {category}/

<b>✅ Создано задач:</b> {M}
• {task_name} <i>({priority}, {due})</i>

<b>📅 Загрузка на неделю:</b>
Пн: {n} | Вт: {n} | Ср: {n} | Чт: {n} | Пт: {n} | Сб: {n} | Вс: {n}

<b>⚠️ Требует внимания:</b>
• {count} просроченных задач
• Цель "{goal}" без активности {days} дней

<b>🔗 Новые связи:</b>
• [[Note A]] ↔ [[Note B]]

<b>⚡ Топ-3 приоритета на завтра:</b>
1. {task} <i>({goal if aligned})</i>
2. {task}
3. {task}

<b>📈 Прогресс по целям:</b>
• {goal_name}: {progress}% {emoji}

---
<i>Обработано за {duration}</i>
```

---

## Section Rules

### Focus (🎯)
Read from `goals/week_YYYY_WXX.md`, find "ONE Big Thing" section.
If not found: "Не задан — обновите goals/week_YYYY_WXX.md"

### Thoughts (📓)
Count saved, list with category emoji:
- 💡 idea (идея)
- 🪞 reflection (размышление)
- 🎯 project (проект)
- 📚 learning (обучение)

### Tasks (✅)
Count created, list with priority and due date.
Format: `• Task name <i>(p2, friday)</i>`

Projects for tasks:
- Работа
- Подработка
- Телеграм-канал
- Мои проекты
- Обучение
- Личное

### Week Load (📅)
Call find-tasks-by-date for 7 days.
Format: `Пн: 4 | Вт: 2 | ...`

### Attention (⚠️)
Show only if issues exist.
Check:
- Overdue tasks (просроченные задачи)
- Stale goals (7+ days no activity)

### Links (🔗)
Show only if new links created.
Format: `• [[Note A]] ↔ [[Note B]]`

### Priorities (⚡)
Get tomorrow's tasks from Todoist, sort by priority, show top 3.
If aligned with goal, show: `<i>(→ Цель: Организация отдела)</i>`

### Goals Progress (📈)
Read `goals/goals_2026.md`, show goals with recent activity.

Goal areas:
- Здоровье
- Карьера и доход
- Финансы
- Телеграм-канал
- Семья
- Личностный рост

Emojis:
- 🔴 0-25%
- 🟡 26-50%
- 🟢 51-75%
- ✅ 76-100%

---

## Error Report

```
❌ <b>Ошибка обработки</b>

<b>Причина:</b> {error_message}
<b>Файл:</b> <code>{file_path}</code>

<i>Попробуйте /process снова</i>
```

---

## Empty Report

```
📭 <b>Нет записей для обработки</b>

Файл <code>daily/{date}.md</code> пуст.

<i>Добавьте записи в течение дня</i>
```

---

## Length Limit

Telegram max: 4096 characters.

If exceeds:
1. Truncate "Новые связи" first
2. Then keep only top 3 goals
3. If still exceeds — truncate thoughts list

---

## Validation Checklist

Before returning report:
1. ✅ All tags closed
2. ✅ No raw < or > in text (use `&lt;` `&gt;`)
3. ✅ No markdown syntax
4. ✅ No tables
5. ✅ Length under 4096 chars

---

## Example Report

```
📊 <b>Обработка за 09.01.2026</b>

<b>🎯 Текущий фокус:</b>
Начать слепую печать — 20 мин тренировки в день + печатать на работе новым методом

<b>📓 Сохранено мыслей:</b> 2
• 💡 Идея автоматизации АОСР → Мысли/идеи/
• 📚 Заметки по слепой печати → Обучение/

<b>✅ Создано задач:</b> 3
• ПИР Березовая 15 НВ — Оформить АОСР <i>(p2, 10.01)</i>
• Слепая печать — тренировка 20 мин <i>(p3, сегодня)</i>
• Написать пост в ТГ-канал <i>(p3, 12.01)</i>

<b>📅 Загрузка на неделю:</b>
Чт: 3 | Пт: 4 | Сб: 2 | Вс: 1 | Пн: 5 | Вт: 3 | Ср: 2

<b>⚡ Топ-3 приоритета на завтра:</b>
1. ПИР Березовая 15 НВ — Оформить АОСР <i>(→ Работа)</i>
2. Внуково НК — Составить АОСР
3. Слепая печать — тренировка

<b>📈 Прогресс по целям:</b>
• Закрыть долги: 90% ✅
• Организация отдела: 25% 🟡
• Телеграм-канал 600 подписчиков: 22% 🔴

---
<i>Обработано за 2.3 сек</i>
```

---

*Обновлено: 10 января 2026*
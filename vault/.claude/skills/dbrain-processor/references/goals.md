# Goals Integration

## ALWAYS Do First

Before processing daily entries:

1. **Read current focus:**
   ```
   Read goals/week_YYYY_WXX.md → Extract ONE Big Thing
   ```

2. **Read yearly goals:**
   ```
   Read goals/goals_YYYY.md → Know active goals by area
   ```

3. **Read monthly priorities:**
   ```
   Read goals/month_YYYY.md → Top 3 priorities
   ```

4. **Read vision (context):**
   ```
   Read goals/vision_YYYY_YYYY.md → Long-term direction
   ```

---

## Goal Files Structure

| File | Content | Update Frequency |
|------|---------|------------------|
| `vision_2026_2029.md` | 3-летнее видение, миссия | Раз в квартал |
| `goals_2026.md` | Годовые цели по областям | Раз в месяц |
| `january_2026.md` (month) | Топ-3 приоритета месяца | В начале месяца |
| `week_2026_W02.md` (week) | ONE Big Thing + задачи недели | В начале недели |

---

## Goal Areas (from Vision)

| Area | Russian | Key Goals 2026 |
|------|---------|----------------|
| Health | Здоровье | Упражнения для спины, -3 кг, желудок |
| Career | Карьера | Повышение до 320 т.р., организация отдела |
| Finance | Финансы | Закрыть долги (август), авто (октябрь) |
| Content | Телеграм-канал | 600 подписчиков, подготовка курса |
| Family | Семья | Свадьба (сентябрь) |
| Growth | Личностный рост | Стрессоустойчивость, уверенность |

---

## Goal Alignment

When creating a task, ask:

1. **Does it connect to ONE Big Thing?**
   - Yes → add to task description: `→ Фокус недели`
   - No → continue checking

2. **Does it connect to monthly priority?**
   - Yes → add: `→ Месяц: [Приоритет]`
   - No → continue checking

3. **Does it connect to yearly goal?**
   - Yes → add: `→ Цель: [Название цели]`
   - No → mark as "операционная"

---

## Task Priority Boost

If task aligns with goals, consider priority bump:

| Alignment | Default | Boost to |
|-----------|---------|----------|
| ONE Big Thing | p3 | p2 |
| Monthly priority | p3 | p2-p3 |
| Yearly goal | p4 | p3 |
| No alignment | p4 | p4 |

---

## Saving Thoughts

When saving to Obsidian:

1. **Check goal relevance:**
   - Scan `goals_2026.md` for matching areas
   - If matches → add link in frontmatter:
     ```yaml
     related:
       - "[[goals/goals_2026#Карьера и доход]]"
     ```

2. **Tag with goal area:**
   ```
   #цель/здоровье
   #цель/карьера
   #цель/финансы
   #цель/канал
   #цель/семья
   #цель/рост
   ```

---

## Goal Progress Tracking

Track goal activity by:

- Task created → goal is "active"
- Thought saved → goal is "active"
- No activity 7+ days → "stale" (застой)
- No activity 14+ days → "warning" (требует внимания)

---

## Report Section

Add to report:

```
<b>📈 Прогресс по целям:</b>
{for each active yearly goal with recent activity:}
• {goal}: {progress}% {status_emoji}

{if stale goals:}
<b>⚠️ Требует внимания:</b>
• Цель "{goal}" без активности {days} дней
```

---

## Goal File Parsing

### week_YYYY_WXX.md — Find ONE Big Thing

Look for pattern:
```markdown
## ONE Big Thing

> **Если я не сделаю ничего другого, я сделаю:**
> [THE ONE THING]
```

### goals_YYYY.md — Find Active Goals

Look for tables:
```markdown
| Цель | Прогресс | Статус |
|------|----------|--------|
| Goal name | X% | 🟡 |
```

### month_YYYY.md — Find Top 3

Look for section:
```markdown
## Топ-3 приоритета

1. **[Приоритет 1]**
2. **[Приоритет 2]**
3. **[Приоритет 3]**
```

---

## Example Alignment

Entry: "Сделать инструкцию для инженера ПТО по оформлению АОСР"

Check:
- ONE Big Thing: "Начать слепую печать" → Not related
- Monthly #1: "Программа для ПТО готова к внедрению" → ✅ Related
- Yearly: "Организовать работу отдела ПТО" → ✅ Related

Result:
```
Task: Сделать инструкцию для инженера ПТО по оформлению АОСР
Description: → Месяц: Программа для ПТО → Цель: Организация отдела
Priority: p2 (boosted from p3)
Project: Мои проекты
```

---

## Decision Filters Integration

Before saving, also check Decision Filters (from ABOUT.md):
- Сколько времени это займёт?
- Можно ли это автоматизировать?
- Это приближает к финансовой безопасности (500 т.р./мес)?
- Это помогает стать признанным экспертом?

If goal-aligned AND passes 2+ filters → highest priority.

---

*Обновлено: 10 января 2026*
# Critical Processing Rules

See [ABOUT.md](ABOUT.md) for user context and preferences.

---

## Rule 1: Skip Processed Entries

```
If entry contains `<!-- ✓ processed` → SKIP COMPLETELY
```

Check AFTER each `## HH:MM` header for the marker.

---

## Rule 2: Every Task = Date

**NEVER create a task without `dueString`:**

| Text | dueString |
|------|-----------|
| сегодня | today |
| завтра | tomorrow |
| послезавтра | in 2 days |
| до пятницы | friday |
| в пятницу | friday |
| в понедельник | monday |
| на этой неделе | friday |
| на следующей неделе | next monday |
| к 15 января | January 15 |
| через неделю | in 7 days |
| срочно | today |
| NOT SPECIFIED (работа) | in 3 days |
| NOT SPECIFIED (проекты) | in 7 days |

---

## Rule 3: Check Duplicates

**BEFORE creating any task:**

1. Call `find-tasks` with key words from task
2. If similar task exists → **DO NOT CREATE**
3. Mark as: `<!-- ✓ processed: task (duplicate) -->`

Keywords to check:
- Название объекта (Зюзино, Березовая, Внуково...)
- Тип действия (АОСР, согласовать, отправить...)
- Имя коллеги или заказчика

---

## Rule 4: Consider Workload

**BEFORE creating tasks:**

1. Call `find-tasks-by-date` with `startDate: "today"`, `daysCount: 7`
2. Count tasks per day
3. If target day has 7+ tasks → shift to next day with less load
4. Note in report: "сдвинуто на {day} (перегрузка)"

Comfort limit: 5+ tasks/day is normal (including control tasks)

---

## Rule 5: Mark After Processing

After EACH processed entry, add marker:

```markdown
<!-- ✓ processed: {category} -->
```

Categories:
- `task` — задача в Todoist
- `idea` — идея в Obsidian
- `learning` — обучение в Obsidian
- `reflection` — размышление в Obsidian
- `skip` — пропущено (не требует действий)
- `duplicate` — дубликат существующей задачи

For tasks with details:
```markdown
<!-- ✓ processed: task → Todoist: {name} {priority} {date} -->
```

Examples:
```markdown
<!-- ✓ processed: task → Todoist: ПИР Березовая 15 НВ — Оформить АОСР p2 10.01 -->
<!-- ✓ processed: idea → Мысли/идеи/2026-01-09-автоматизация-аоср.md -->
<!-- ✓ processed: task (duplicate) -->
```

---

## Rule 6: Apply Decision Filters

Before saving any thought or task, check:
- Сколько времени это займёт и как встроить в график?
- Можно ли это автоматизировать?
- Это приближает к финансовой безопасности (500 т.р./мес)?
- Это помогает стать признанным экспертом в отрасли?
- Это масштабируется (курс, консалтинг, продукт)?

If 2+ yes → boost priority by 1 level.

---

## Rule 7: Avoid Anti-Patterns

**NEVER create:**

При обработке записей:
- ❌ Абстрактные задачи без конкретного действия ("Подумать о...")
- ❌ Размытые формулировки ("Разобраться с объектом")
- ❌ Задачи без дат
- ❌ Дубликаты существующих задач
- ❌ Списки без приоритетов
- ❌ Повторы без синтеза (кластеризуй похожие!)

В мышлении избегать:
- ❌ Обесценивать достижения ("это недостаточно")
- ❌ Сравнивать себя с другими в ущерб себе
- ❌ Браться за всё сразу без оценки времени

See [ABOUT.md](ABOUT.md) → Anti-Patterns section.

---

## Rule 8: Task Title Style

**Good task titles:**
- ✅ "ПИР Березовая 15 НВ — Оформить АОСР"
- ✅ "Внуково НК — Составить АОСР"
- ✅ "Подойти к Синельникову по поводу Щукинской"
- ✅ "Слепая печать — 20 минут тренировки"

**Bad task titles:**
- ❌ "Подумать о проекте"
- ❌ "Что-то с документами"
- ❌ "Разобраться с объектом"

---

## Rule 9: Project Assignment

| Keywords | Project |
|----------|---------|
| объект, АОСР, КС, ИД, заказчик, коллега | Работа |
| подработка, сторонний заказ, фриланс | Подработка |
| пост, канал, подписчики, курс | Телеграм-канал |
| скрипт, автоматизация, программа, ассистент | Мои проекты |
| курс, книга, навык, слепая печать | Обучение |
| здоровье, спина, долг, свадьба, семья | Личное |
| unclear | Inbox |

---

## Rule 10: Priority Assignment

| Domain + Keywords | Priority |
|-------------------|----------|
| Работа + срочно/критично | p1 |
| Работа (обычно) | p2 |
| Подработка | p2-p3 |
| Мои проекты | p3 |
| Телеграм-канал (с дедлайном) | p3 |
| Телеграм-канал (идеи) | p4 |
| Обучение | p4 |
| Личное | p3-p4 |

Boost priority if aligned with:
- ONE Big Thing → +1 level
- Monthly priority → +1 level
- Yearly goal → consider +1 level

---

## Checklist Before Completion

- [ ] All new entries processed
- [ ] No duplicates in Todoist
- [ ] All tasks have dates and concrete actions
- [ ] All tasks assigned to correct project
- [ ] Decision filters applied
- [ ] Anti-patterns avoided
- [ ] Goal alignment checked
- [ ] MOC files updated (if thoughts saved)
- [ ] Report generated

---

*Обновлено: 10 января 2026*
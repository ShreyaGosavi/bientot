# Data Design

What Bientôt stores, and how it connects.

## Decisions

1. **Load** = time spent in _Deep focus_ and _Learning_ tasks, marked as heavy categories.
2. **Moved tasks** stay one task: the date changes, and every day's outcome is kept in `task_outcomes`.
3. **Feeling** uses 5 levels: 1 Drained, 2 Tired, 3 Okay, 4 Good, 5 Energised.
4. **All durations are stored in minutes** (`90`, not `1h 30m`).
5. **The cold period** ends after **7 active days**, meaning days with at least one finished task, not 7 calendar days from sign up.
6. **Dates and times follow the user's time zone**, so "today" means the user's today.

## Relationships

```text
users ─┬─< labels >── categories
       ├─< tasks ─┬─< predictions
       │          └─< task_outcomes
       ├─< day_plans
       └─< category_factors >── categories
```

_`─<` means one-to-many: one user has many tasks._

## Tables

### users

| Field                 | Type      | Notes                              |
| --------------------- | --------- | ---------------------------------- |
| `id`                  | UUID      | Primary key                        |
| `email`               | text      | Unique                             |
| `password_hash`       | text      | Never the plain password           |
| `name`                | text      |                                    |
| `timezone`            | text      | e.g. `Asia/Kolkata`                |
| `daily_hours`         | number    | Usual hours available for work     |
| `focus_limit_minutes` | integer   | Healthy daily limit for heavy work |
| `created_at`          | timestamp |                                    |

### categories

Fixed list, the same for every user.

| Field      | Type    | Notes                                  |
| ---------- | ------- | -------------------------------------- |
| `id`       | integer | Primary key                            |
| `name`     | text    | Unique                                 |
| `is_heavy` | boolean | Counts toward load (the healthy limit) |

| Category         | Heavy |
| ---------------- | ----- |
| Deep focus       | ✅    |
| Learning         | ✅    |
| Creative         |       |
| Admin            |       |
| Communication    |       |
| Errands & chores |       |
| Personal         |       |
| Miscellaneous    |       |

### labels

Your own names for tasks, each mapped to one category.

| Field         | Type    | Notes                  |
| ------------- | ------- | ---------------------- |
| `id`          | UUID    | Primary key            |
| `user_id`     | UUID    | → users                |
| `category_id` | integer | → categories           |
| `name`        | text    | e.g. "DBMS assignment" |

A user can create any number of labels, but each name is used only once per user (no duplicates).

### tasks

| Field                 | Type      | Notes                                                   |
| --------------------- | --------- | ------------------------------------------------------- |
| `id`                  | UUID      | Primary key                                             |
| `user_id`             | UUID      | → users                                                 |
| `label_id`            | UUID      | → labels (which gives the category)                     |
| `planned_date`        | date      | Changes when the task is moved                          |
| `estimate_minutes`    | integer   | Your original guess, never changed                      |
| `suggested_minutes`   | integer   | Bientôt's suggestion, if one was shown                  |
| `suggestion_accepted` | boolean   | Whether you took it                                     |
| `actual_minutes`      | integer   | Filled in when done                                     |
| `status`              | enum      | `planned` · `done` · `not_done`                         |
| `overrun_reason`      | enum      | `genuinely_long` · `distracted` · `something_came_up`   |
| `feeling`             | integer   | 1–5, filled in when done                                |
| `created_at`          | timestamp |                                                         |
| `completed_at`        | timestamp | When it was finished (used for "when you perform best") |

**Rules:**

- A moved task goes back to `planned` with its new date; the move is recorded in `task_outcomes`.
- `overrun_reason` is only asked when `actual_minutes` is more than 15% over `estimate_minutes`.
- The time a task takes up in a plan is `suggested_minutes` if accepted, otherwise `estimate_minutes`.

### predictions

Every prediction ever made, kept for measuring accuracy.

| Field               | Type      | Notes                                           |
| ------------------- | --------- | ----------------------------------------------- |
| `id`                | UUID      | Primary key                                     |
| `task_id`           | UUID      | → tasks                                         |
| `predicted_minutes` | integer   |                                                 |
| `verdict`           | enum      | `need_more` · `about_right` · `might_take_less` |
| `source`            | enum      | `model` · `fallback`                            |
| `model_version`     | text      | Which model made it (empty for fallback)        |
| `created_at`        | timestamp |                                                 |

### task_outcomes

What happened to a task on each day it was planned.

| Field     | Type | Notes                         |
| --------- | ---- | ----------------------------- |
| `id`      | UUID | Primary key                   |
| `task_id` | UUID | → tasks                       |
| `date`    | date |                               |
| `outcome` | enum | `done` · `not_done` · `moved` |

Unique: one outcome per task per day.

Example: _DBMS assignment_ → Mon `moved`, Tue `done`.

### day_plans

| Field               | Type    | Notes                   |
| ------------------- | ------- | ----------------------- |
| `id`                | UUID    | Primary key             |
| `user_id`           | UUID    | → users                 |
| `date`              | date    |                         |
| `available_minutes` | integer | Hours you have that day |

Unique: one plan per user per day.

### category_factors

A saved summary of how far off you usually are in each category, used by the fallback.

| Field          | Type      | Notes                                           |
| -------------- | --------- | ----------------------------------------------- |
| `user_id`      | UUID      | → users                                         |
| `category_id`  | integer   | → categories                                    |
| `factor`       | number    | Average of actual ÷ estimate (1.3 = 30% longer) |
| `sample_count` | integer   | How many finished tasks it's based on           |
| `updated_at`   | timestamp |                                                 |

Primary key: `user_id` + `category_id`. It can always be recalculated from `tasks`.

## What each feature reads

| Feature               | Uses                                        |
| --------------------- | ------------------------------------------- |
| Cold period check     | `tasks` (days with a finished task)         |
| Estimate suggestion   | `category_factors`, `predictions`           |
| Prediction & verdict  | `tasks`, `predictions`                      |
| Fallback              | `category_factors`                          |
| Plan my day & checks  | `day_plans`, `tasks`, `categories`, `users` |
| Completion rate       | `task_outcomes`                             |
| Where your time goes  | `tasks`, `labels`, `categories`             |
| Accuracy              | `predictions`, `tasks`                      |
| When you perform best | `tasks` (`completed_at`, `feeling`)         |

## Open questions

- Add `started_at` to tasks, to make "when you perform best" more accurate?
- Should deleted tasks be removed, or hidden so their learning data is kept?

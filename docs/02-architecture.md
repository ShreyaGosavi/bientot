# Architecture

## Overview

Bientôt is made of **four parts**, and each one has a single job:

- **Frontend:** what you see and click
- **Backend:** the brain that connects everything and holds the rules
- **ML service:** learns your patterns and predicts durations
- **Database:** remembers everything

```text
            You
             │
             ▼
     ┌───────────────┐
     │   Frontend    │   screens, forms, dashboard
     └───────┬───────┘
             │  API requests
             ▼
     ┌───────────────┐        ┌───────────────┐
     │    Backend    │ ─────▶ │   Database    │
     │  rules, checks│ ◀───── │  tasks, users │
     └───────┬───────┘        └───────────────┘
             │  "how long will this take?"
             ▼
     ┌───────────────┐
     │  ML service   │   trained model
     └───────────────┘
```

**Two ideas shape everything:**

1. **The backend is the only middleman.** The frontend never touches the database or the model directly.
2. **The app never depends on the model.** If the ML service is down, the backend falls back to a simple formula, so Bientôt keeps working.

## Folder structure

```text
bientot/
├── docs/                  # All project documentation
├── frontend/              # React + TypeScript app
│   └── src/
│       ├── pages/         # One file per screen (Today, Dashboard…)
│       ├── components/    # Reusable pieces (TaskRow, CheckCard…)
│       ├── api/           # Functions that call the backend
│       └── styles/        # Brand colours, fonts
├── backend/               # FastAPI app
│   ├── app/
│   │   ├── routes/        # API endpoints (/tasks, /plan…)
│   │   ├── services/      # Business logic (checks, verdicts, fallback)
│   │   ├── models/        # Database tables
│   │   └── schemas/       # Request/response shapes
│   └── tests/
├── ml/                    # Model training and serving
│   ├── data/              # Synthetic + exported data
│   ├── training/          # Scripts that train the model
│   ├── serving/           # Small API that answers predictions
│   ├── models/            # Saved trained models
│   └── tests/
├── .github/               # CI workflows, templates, Dependabot
└── (root config files)    # Prettier, Husky, Commitlint, EditorConfig…
```

**Rules of the structure:**

- Each top folder is **independent**: it has its own dependencies and tests, and it runs on its own.
- `routes` only receive and return data; the **thinking happens in `services`**.
- `ml/` never reads the database directly. It gets data **from the backend** (or from exported files for training).

_The subfolders are the plan; they'll be created as each phase starts._

## The four parts

### Frontend

**Does:** shows the screens, takes your input, and displays predictions, checks and the dashboard.

**Doesn't:** calculate anything important, store data, or talk to the model.

### Backend

**Does:**

- Handles sign up, login and every request
- Saves and reads tasks, outcomes, reasons and feelings
- Runs the three checks and healthy limits
- Turns a prediction into a verdict (±15% rule)
- Asks the ML service for predictions, and uses the **fallback** if it doesn't answer
- Saves every prediction for accuracy tracking

**Doesn't:** train models or draw screens.

### ML service

**Does:**

- Trains the model, first on synthetic data and later on real data
- Answers one question: _"How long will this task really take?"_
- Reports how confident it is

**Doesn't:** store user data, apply rules, or decide verdicts.

### Database

**Does:** stores users, tasks, predictions, outcomes, reasons and feelings, and remains the **single source of truth**.

**Doesn't:** contain logic. It only remembers.

**In one line:** the frontend **shows**, the backend **decides**, the ML service **predicts**, and the database **remembers**.

## How they talk

All parts talk over **HTTP using JSON** (a REST API).

| From       | To         | What for                                                 |
| ---------- | ---------- | -------------------------------------------------------- |
| Frontend   | Backend    | Every action: add a task, plan a day, load the dashboard |
| Backend    | Database   | Save and read data                                       |
| Backend    | ML service | Ask for a prediction                                     |
| ML service | (nobody)   | It only answers; it never calls anyone                   |

**Calls only go one way, down the chain.** The frontend never skips the backend, and the ML service never calls back.

### Example: adding a task

```text
1. You          → Frontend    Fill in "DBMS assignment, Study, 1h 30m" and save
2. Frontend     → Backend     POST /tasks
3. Backend      → Database    Is this user past the 7-day cold period? → Yes
4. Backend      → ML service  POST /predict  { category: Study, estimate: 90 … }
5. ML service   → Backend     { predicted: 130 }   (2h 10m)
6. Backend      (internal)    130 vs 90 = +44% → over 15% → verdict "Need more"
7. Backend      → Database    Save the task + the prediction
8. Backend      → Frontend    { task, predicted: 130, verdict: "need_more" }
9. Frontend     → You         Shows "Bientôt suggests 2h 10m"
```

**What the example shows:** the backend makes the decision (step 6), the ML service only gives a number (step 5), and everything is saved (step 7) so accuracy can be checked later.

## Prediction flow

What happens each time a task needs a prediction:

```text
New task arrives at the backend
            │
            ▼
   Past the 7-day cold period?
      │                │
     No               Yes
      │                │
      ▼                ▼
 No prediction     Ask the ML service
 (just learning)        │
                 ┌──────┴──────┐
              Answers      No answer / error
                 │              │
                 ▼              ▼
           Use model      FALLBACK:
           prediction     estimate × your category factor
                 └──────┬──────┘
                        ▼
              Compare with your estimate
                        │
        ┌───────────────┼───────────────┐
    ≥ 15% more     within ±15%     ≥ 15% less
        │               │               │
   "Need more"    "About right"   "Might take less"
                        │
                        ▼
        Save task + prediction + source (model or fallback)
```

**After you finish the task:**

1. The actual time is saved next to the prediction
2. Your **category factor** is updated (actual ÷ estimate, averaged)
3. This becomes new training data for the model

**Edge cases:**

- **No history in a category yet:** the fallback uses your overall factor, and if there is none, 1.0 (it trusts your estimate).
- **Source is saved** with every prediction, so you can later compare how accurate the model is against the fallback.

## Design principles

1. **The app never depends on the model.** There is always a fallback, so the model is an upgrade, not a requirement.
2. **One middleman.** Everything goes through the backend, so rules live in one place.
3. **Each part has one job.** Show, decide, predict, remember, and no part does another's job.
4. **Save everything you learn from.** Estimates, actuals, predictions, reasons and feelings are all kept, because data that isn't saved can't teach anything.
5. **Measure the model honestly.** Every prediction stores its source and is compared with reality.
6. **Predictions are personal.** One shared model learns how estimation habits work, and each prediction is built from that user's own history.
7. **Simple first, smart later.** It starts with a formula, adds ML, then adds insights in v2. Each phase works on its own.
8. **Parts can change independently.** You can retrain the model, redesign screens or switch databases without rewriting the rest.

## Deployment

_To be decided._ This section will describe where each part runs (frontend, backend, ML service, database) once hosting is chosen in Phase 7.

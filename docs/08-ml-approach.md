# ML Approach

How Bientôt learns your time patterns and predicts how long tasks will really take.

## The goal

Predict how long a task will **actually** take, from your estimate and your history.

## What the model predicts

The model predicts your **error ratio**, not minutes directly:

```text
ratio      = actual ÷ estimate          (1.4 = 40% longer)
prediction = estimate × predicted ratio
```

**Why a ratio:** a 20-minute task and a 3-hour task can share the same habit, like "I always take 40% longer". Predicting the ratio lets the model learn the habit, not the size of the task.

## One shared model, personal predictions

Bientôt uses **one model trained on everyone's data**, with **personal features** for each user.

- The model learns _how_ estimation habits work in general.
- Personal features (your own past factors) make each prediction about _you_.
- New users still get sensible predictions, because the model has learned from others.

**Later (v2):** a small per-user correction on top of the shared model, for people with a lot of history.

## Features

| Feature                      | Example                         |
| ---------------------------- | ------------------------------- |
| Your estimate                | 90 minutes                      |
| Category                     | Learning                        |
| Your factor for the category | 1.45                            |
| Your factor for the label    | 1.30 for "DBMS assignment"      |
| Day of the week              | Friday                          |
| Time of day                  | Evening                         |
| How full the day is          | 6 tasks, 5h of heavy work       |
| Recent overrun rate          | 60% of tasks ran over this week |
| Recent overrun reasons       | Mostly "distracted"             |

## Data

1. **Synthetic data first:** simulated users with different habits (always late, accurate, worse in the evenings, and so on), so the model can be built and tested before real data exists.
2. **Real data later:** finished tasks from real use replace the synthetic data over time.

**Data rules:**

- Only finished tasks with an actual time are used for training.
- Tasks that ran over because _something came up_ are treated carefully, since that delay wasn't about the estimate.

## Training

1. Build features from the data
2. Split by **time**: train on older tasks, test on newer ones
3. Train a simple baseline model first
4. Try stronger models, and keep one only if it clearly beats the baseline
5. Track every experiment: data, settings and results

**Why split by time:** shuffling old and new data together lets the model "see the future" during testing, which makes it look better than it really is.

## Evaluation

| Metric          | What it means                                               |
| --------------- | ----------------------------------------------------------- |
| **Within ±15%** | % of predictions close enough to call "right" (main metric) |
| **MAE**         | Average error in minutes                                    |

**Baselines the model must beat:**

| Baseline           | Prediction                      |
| ------------------ | ------------------------------- |
| Trust the estimate | Your guess, unchanged           |
| Fallback formula   | Your estimate × category factor |

If the model doesn't beat both, it isn't used.

## Serving

- The model runs as a small prediction service.
- Each saved model has a **version**, stored with every prediction it makes.
- If the service is down, the backend uses the **fallback formula**, so the app never stops working.
- No predictions are shown during a user's **cold period** (the first 7 active days).

## Measuring in real use

Every prediction is saved next to the actual time, so accuracy can be tracked over time and the model compared with the fallback on real data.

## Later

- Retraining on real user data
- A per-user correction for people with a long history
- "When you perform best" insights from time of day, feelings and outcomes

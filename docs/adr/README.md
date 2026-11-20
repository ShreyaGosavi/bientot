# Architecture Decision Records

Each file records one decision: the context, what was decided, the alternatives, and the consequences.

| #    | Decision                                                                                                                             | Status   |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| 0001 | [Record architecture decisions](0001-record-architecture-decisions.md)                                                               | Accepted |
| 0002 | [Use Python and FastAPI for the backend](0002-use-python-and-fastapi-for-the-backend.md)                                             | Accepted |
| 0003 | [Use Next.js for the frontend](0003-use-next-js-for-the-frontend.md)                                                                 | Accepted |
| 0004 | [Use PostgreSQL as the database](0004-use-postgresql-as-the-database.md)                                                             | Accepted |
| 0005 | [The backend is the only middleman](0005-the-backend-is-the-only-middleman.md)                                                       | Accepted |
| 0006 | [Always keep a fallback prediction](0006-always-keep-a-fallback-prediction.md)                                                       | Accepted |
| 0007 | [Use a main, dev and feature branch workflow](0007-use-a-main-dev-and-feature-branch-workflow.md)                                    | Accepted |
| 0008 | [Enforce conventional commits, formatting and secret scanning](0008-enforce-conventional-commits-formatting-and-secret-scanning.md)  | Accepted |
| 0009 | [Keep everything in one repository](0009-keep-everything-in-one-repository.md)                                                       | Accepted |
| 0010 | [Use fixed base categories with personal labels](0010-use-fixed-base-categories-with-personal-labels.md)                             | Accepted |
| 0011 | [Keep moved tasks as one task](0011-keep-moved-tasks-as-one-task.md)                                                                 | Accepted |
| 0012 | [End the cold period after 7 active days](0012-end-the-cold-period-after-7-active-days.md)                                           | Accepted |
| 0013 | [Predict the error ratio, not minutes](0013-predict-the-error-ratio-not-minutes.md)                                                  | Accepted |
| 0014 | [Use one shared model with personal features](0014-use-one-shared-model-with-personal-features.md)                                   | Accepted |
| 0015 | [Evaluate the model against baselines with a time-based split](0015-evaluate-the-model-against-baselines-with-a-time-based-split.md) | Accepted |
| 0016 | [Build JWT authentication in-house](0016-build-jwt-authentication-in-house.md)                                                       | Accepted |
| 0017 | [Use Tailwind CSS for styling](0017-use-tailwind-css-for-styling.md)                                                                 | Accepted |
| 0018 | [Use uv for Python packages and environments](0018-use-uv-for-python-packages-and-environments.md)                                   | Accepted |

## Adding a new ADR

1. Copy the last file and give it the next number
2. Fill in context, decision, alternatives and consequences
3. Never edit an accepted ADR; write a new one that replaces it, and mark the old one as **Superseded**

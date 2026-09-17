# Academic System — Colégio Clério Boechat

The operational core of the [Educational Platform](#) built for a public school in Rio de Janeiro, Brazil. <!-- link do repositório guarda-chuva -->
In daily use · 100+ users · ~200 exams processed

---

## The problem

The school had no digital system at all. Exams were assembled by hand, printed, and corrected one by one. Grades were consolidated on paper before each class council. Everything that could be lost on paper eventually was.

## What it does

- **Question bank** — teachers register and reuse questions under a standardized layout
- **Exam generation** — exams are assembled from the bank and rendered print-ready
- **Approval workflow** — exams move through review before release
- **Automated answer-sheet grading** — scanned answer cards are read optically and scored
- **Grades and class councils** — grade entry, consolidation, and the full *conselho de classe* flow
- **Announcements and notifications** — communication between school, teachers and families

## Architecture: two backends, on purpose

This is the design decision worth explaining.

```
React + Vite (SPA)
        │
        ├──────────────► AdonisJS API ──────► PostgreSQL
        │                (academic domain: users, questions,
        │                 exams, approvals, grades, notifications)
        │
        └──────────────► FastAPI service
                         (optical answer-sheet reading
                          + exam generation)
```

**Why not keep everything in AdonisJS?**

Answer-sheet grading is an image-processing problem, not a CRUD problem. The Node implementation was measurably worse on the two axes that matter for it: **accuracy** reading the marked bubbles, and **speed** per sheet. Python's imaging ecosystem handled it better, so that capability moved into a dedicated FastAPI service instead of being forced into the main API. The same service also speeds up exam generation.

The trade-off is explicit: one more service to deploy and monitor, in exchange for the highest-risk feature in the system working reliably. A wrong grade is a wrong grade. Everything else stays in AdonisJS, where a mature ORM and a conventional web framework are the right tools.

**Why Railway?**

Unpaid work for a public school with no budget. Hosting had to be cheap, predictable, and simple enough that the system survives without a dedicated infrastructure team.

## Stack

**Frontend** React · Vite · TypeScript
**Academic API** AdonisJS · PostgreSQL
**Grading service** FastAPI · Python <!-- acrescentar a biblioteca de processamento de imagem -->
**Infrastructure** Railway

## Status

In production and in daily use by the school.

---

*Built and maintained by [André Luiz Ribeiro da Silva](https://www.linkedin.com/in/aribeirorj/) — Senior Frontend Engineer.*

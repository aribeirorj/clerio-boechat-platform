# Educational Platform — Colégio Clério Boechat

A modular platform for Brazilian public schools, running in production.
**[portalclerioboechat.com.br](https://www.portalclerioboechat.com.br/)** · 100+ users · ~200 exams processed

> **This repository is documentation, not code.** The applications run in private repositories — they handle real student data. What lives here is the design record: what the platform does, how it is split, and why. The systems themselves are live and open to anyone at the link above.

---

## Context

The school had no digital system at all. Exams were assembled by hand, printed and corrected one by one. Grades were consolidated on paper before each class council. Announcements reached families however they could.

I designed and built the platform from scratch and maintain it as a contribution to public education — which made **cost a hard architectural constraint**, not an afterthought.

## Why four applications instead of one

This is the central design decision, and it is a product decision before it is a technical one.

Schools are not interchangeable. A small public school needs a public presence and little else; a larger one needs exam operations, grade consolidation and student engagement. A single monolith would force every school to carry — and pay for — the whole thing.

So the platform is **composed, not installed**. Each application is independent: its own deployment, its own lifecycle, its own cost. A school adopts the pieces that match its size and budget, and the pieces it does not adopt simply do not exist for it.

The trade-off is explicit: more surfaces to deploy and monitor, in exchange for a product that fits schools of very different scales instead of fitting only the one it was born in.

## The applications

| Application | What it is | Live |
|---|---|---|
| **Institutional portal** | Public face of the school — institutional information, announcements and content for the community | [portalclerioboechat.com.br](https://www.portalclerioboechat.com.br/) |
| **Academic system** | The operational core: question bank, exam generation, approval workflow, optical answer-sheet grading, grades, class councils, notifications | <!-- URL --> |
| **Study gamification** | Question rounds, scoring, ranking and per-question feedback routed to supporting video content | <!-- URL --> |
| **Anti-racism portal** | Thematic educational portal | [neerce.portalclerioboechat.com.br](https://neerce.portalclerioboechat.com.br/) |

Deeper write-ups:

- **[Academic system →](docs/sistema-academico.md)** — including the two-backend decision
- **[Study gamification →](docs/gamificacao.md)**

## Architecture

```
  Institutional      Academic         Gamification      Thematic
     portal           system            platform         portal
        │                │                  │               │
        └────────────────┼──────────────────┴───────────────┘
                         │
          ┌──────────────┴──────────────┐
          │                             │
    AdonisJS API                 FastAPI service
 (academic domain: users,      (optical answer-sheet
  questions, exams, grades,     reading + exam generation)
  approvals, notifications)
          │
     PostgreSQL
```

**Two backends, on purpose.** Answer-sheet grading is an image-processing problem, not a CRUD problem. The Node implementation was measurably worse on the two axes that matter for it — **accuracy** reading the marked bubbles and **speed** per sheet. Python's imaging ecosystem handled it better, so that capability moved into a dedicated FastAPI service instead of being forced into the main API. The same service also speeds up exam generation.

A wrong grade is a wrong grade: this is the highest-risk feature in the platform, and it got the tool that does it reliably. Everything else stays in AdonisJS, where a mature ORM and a conventional web framework are the right call.

**Why Railway.** Unpaid work for schools with no budget. Hosting had to be cheap, predictable, and simple enough that the platform survives without a dedicated infrastructure team.

## Stack

**Frontend** React · Vite · TypeScript
**Academic API** AdonisJS · PostgreSQL
**Grading service** FastAPI · Python
**Infrastructure** Railway

## Status

All four applications are live. The academic system is in daily use; the gamification platform is in production pending sign-off from the school board.

---

*Built and maintained by [André Luiz Ribeiro da Silva](https://www.linkedin.com/in/aribeirorj/) — Senior Frontend Engineer.*

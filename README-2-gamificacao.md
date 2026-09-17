# Study Gamification Platform

Part of the [Educational Platform](../README.md) built for Colégio Clério Boechat.
In production, pending sign-off from the school board.

> The application runs in a private repository — it handles real student data. This page is the design record.

---

## The idea

Studying outside class competes with everything else a teenager could be doing. This module turns review into short, scored rounds with a visible ranking — and, more importantly, makes a wrong answer the **start** of something instead of a dead end.

## How it works

**Teacher side**

1. Registers the questions
2. Registers the students
3. Defines how many questions each round holds
4. Opens the round

**Student side**

1. Logs in and answers the open round
2. Answers generate a score
3. The score places the student on the **ranking**
4. Every wrong answer returns **feedback** plus a link to supporting video content on YouTube

## The design decision that matters

The ranking is the hook, but the feedback loop is the product. A conventional quiz tells the student they got it wrong and stops there. Here, a wrong answer routes straight to material that explains the concept — so the round is not an evaluation, it is a study cycle: **answer → miss → learn → improve the score next round**.

That also keeps the teacher's workload low. The teacher supplies questions and the supporting links once; the loop runs on its own after that. In a public school with no dedicated staff for this, anything that needs constant manual attention simply does not survive.

## Stack

React · Vite · TypeScript · AdonisJS · PostgreSQL <!-- ajustar se divergir -->

## Status

Deployed and running. Awaiting formal sign-off from the school board before opening to all classes.

---

*Built and maintained by [André Luiz Ribeiro da Silva](https://www.linkedin.com/in/aribeirorj/) — Senior Frontend Engineer.*

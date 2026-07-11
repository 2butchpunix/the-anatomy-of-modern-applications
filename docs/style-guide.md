# Editorial Style Guide

> This document defines the writing philosophy behind *The Anatomy of Modern Applications*.
>
> It exists to ensure the book maintains a consistent voice, structure, and learning experience throughout its lifetime.

---

# Core Philosophy

The objective of this book is not to teach technologies.

The objective is to teach **how software systems work**.

Readers should finish each chapter with a stronger mental model rather than a larger collection of memorized facts.

Every concept should answer three questions:

- Why does it exist?
- What problem does it solve?
- Why was the previous solution insufficient?

---

# Writing Principles

## 1. Teach Systems, Not Technologies

Avoid teaching products.

Prefer concepts.

Example:

✅ Cache

❌ Redis client in Go

---

## 2. Problem Before Solution

Never introduce a technology immediately.

Instead:

Problem

↓

Why current solution fails

↓

New solution

↓

Trade-offs

Readers should feel that the technology was inevitable.

---

## 3. Build Mental Models

Definitions are temporary.

Mental models last.

The objective is that readers should be able to derive answers from first principles rather than memorize them.

---

## 4. Guided Curiosity

Prefer questions over definitions.

Instead of

> DNS translates domain names into IP addresses.

Prefer

> Imagine DNS disappeared tomorrow.
>
> What would happen?

Curiosity should naturally lead into explanation.

---

## 5. Author Should Be Invisible

Avoid sentences like

- "The goal of this book..."
- "We'll now learn..."
- "In this chapter we'll..."

The reader should feel like they are discovering ideas, not being instructed.

Exceptions:

- Preface
- Natural transitions
- Very rare reflections

---

## 6. Technical Depth

Optimize for understanding.

Not brevity.

Not interview preparation.

Assume an intellectually curious engineer.

Prefer explaining **why** over merely describing **what**.

---

## 7. Use Stories Sparingly

Stories exist only to generate curiosity.

Do not force analogies.

If a real-world software example explains something better than a story, prefer the software example.

---

## 8. Whiteboard Rule

Every explanation should feel like an experienced engineer standing beside a whiteboard.

The reader should discover answers together with the author.

Never rush to definitions.

---

## 9. Paragraphs, Not Messages

Write books.

Not chat responses.

Each paragraph should communicate one complete thought.

Vary paragraph lengths naturally.

---

## 10. Engineering Philosophy

Occasionally introduce timeless engineering thoughts when they fit naturally.

Example:

> Every abstraction hides complexity above by creating complexity below.

These should emerge naturally from the topic.

Never force them.

---

## 11. Technology Agnostic

Frameworks and languages are examples.

They are never the focus.

Prefer:

HTTP

instead of

Express.js

Prefer:

Processes

instead of

Go processes

---

## 12. Build Forward

Every chapter should naturally create curiosity for the next.

The reader should want to turn the page.

---

# Chapter Structure

Most chapters should naturally follow this flow.

Introduction

↓

Problem

↓

Observation

↓

Question

↓

Current Solution

↓

Limitation

↓

New Solution

↓

Trade-offs

↓

Comparison (if required)

↓

Bringing It All Together

↓

Questions to Ponder

---

# Formatting Guidelines

- Italicize memorable one-line ideas.

Example:

> *A process is what the operating system creates so that the program can actually execute.*

---

Avoid unnecessary emphasis.

Use bold only for introducing important terms.

---

Tables should compare concepts rather than repeat prose.

---

Diagrams should answer a question.

Never exist for decoration.

---

# Rewrite Policy

Each chapter is allowed a maximum of three major rewrites.

After publication,

future improvements belong to the next version of the book.

Avoid perfectionism.

The book should continuously evolve.

---

# The Reader

Imagine one reader.

An engineer.

Curious.

Passionate.

Already familiar with writing software.

Not looking to pass an interview.

Looking to truly understand systems.

Write for that person.

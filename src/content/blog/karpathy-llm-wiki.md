---
title: Everyone's Misreading Karpathy's LLM-Wiki Gist
pubDate: 2026-04-15
description: It's not about replacing RAG with Markdown. It's about turning an LLM from a one-shot answering machine into a compounding editor of a growing knowledge asset.
heroImage: '../../assets/blog-placeholder-2.jpg'
---

Karpathy dropped a gist on building a personal knowledge wiki with LLMs. Most people read it as a RAG alternative. I think that's the wrong frame.

The interesting part isn't the storage format. It's a fundamentally different model of what an LLM is for — and it took me three questions to actually see it.

## Question 1: If the wiki is just files too, how is it different from vector search?

**RAG understands at query time. Wiki understands at write time.**

Vector similarity only finds chunks that look like your question. But the knowledge you actually need often lives in language that doesn't resemble the question at all. In the wiki model, the LLM has already synthesized, cross-linked, and flagged contradictions across documents before you ever ask.

RAG hands you a warehouse of paper scraps and makes you reassemble them on every question. Wiki hands you a finished book — you flip to the chapter and the answer is already written.

## Question 2: Can't I just write a Skill that says "for A, read X then Y"? Isn't that the same thing?

This is the trap. They look identical. They're not.

A Skill is a hand-coded query path. Your ceiling = how many question shapes you can predict. And X and Y are still raw material — the LLM re-reads, re-extracts, re-synthesizes every single time. You optimized file retrieval, not understanding.

A wiki freezes the understanding itself. However weird the angle of the question, if the relationships between concepts were linked during ingestion, the LLM doesn't need you to predict anything.

## Question 3: Isn't Karpathy's schema (CLAUDE.md) just another instruction doc? How is it different from a Skill?

This is the one that finally clicked for me.

**A Skill instructs how to answer one question.** Task execution. Output is an answer. When the task ends, nothing remains.

**A schema instructs how to maintain a living knowledge body.** It defines how to read, write, organize, update, and self-audit. The output isn't an answer — it's a wiki that keeps getting richer.

Skill = recipe for a chef. One dish per order. Schema = cataloging rules for a librarian. Every new book gets classified, indexed, cross-referenced, version-flagged. The recipe serves a meal. The catalog serves an entire library, forever.

## The whole thing in one line

**Skills are stateless. Schemas are stateful.**

RAG is stateless retrieval. Skill-routing is stateless execution. Both start from zero every time.

Karpathy's real move is turning the LLM from a one-shot answering machine into a compounding editor of an asset that keeps growing.

That's why he says the idea file matters more than the code repo. In this paradigm, the valuable artifact isn't the implementation — it's the contract that defines how knowledge accumulates.

---

If you research a domain deeply, track a market long-term, or build institutional knowledge inside a team:

Stop writing better prompts. Start designing schemas that let knowledge grow on its own.

---

So I built one. Here's what the theory doesn't tell you.

## Lesson 1: Don't ingest before your schema is stable.

**Schema first. Everything else second.**

The first instinct is to throw everything at the LLM in one shot. Don't.

Your schema will change as you work with real content. A field you thought mattered turns out to be noise. A distinction you didn't think you needed turns out to be critical for navigation. A missing merge rule means you end up with forty near-identical pages about the same concept.

At small scale, you can fix this. At hundreds of pages, you can't.

The fix: treat your first one or two documents as a schema pilot. Run the full pipeline on a small slice, sit with the output, find what's broken, fix the schema. Only expand once it feels right on something small.

One specific thing that tripped me up: long-form documents and chat messages need different treatment. Long-form content is self-contained — chunk by topic, extract claims directly. Chat messages are reactive. A single message that reads like a crisp insight is often meaningless without the four messages before it. The right unit for chat is a thread, not a message. Treat them identically and you get a wiki full of fragments that look meaningful but aren't.

## Lesson 2: Time is not metadata — it's the whole point.

Most schemas treat timestamps as something you log and sort by. I did this too, initially. It was a mistake.

The interesting thing about capturing one person's thinking over time is that their thinking *changes*. A principle they held confidently two years ago may have been quietly abandoned. Two ideas that look contradictory might make sense once you see one came before a key inflection point and one came after.

None of this is visible in a static wiki of "what they think."

For each knowledge unit, I track: when it first appeared, when it was last mentioned, how many times it's come up, and how confidently it was expressed most recently. The evolution log is sparse — it only gets a new entry when something meaningfully shifts, not on every occurrence.

The payoff is a class of queries retrieval can't answer: *How has their thinking on X evolved over the past year? What ideas have they started mentioning frequently that they barely touched before?* These require structural understanding across the full timeline. A wiki that answers them doesn't feel like a search index. It feels like a model of a mind.

**Both lessons are the same lesson.** LLMs make production feel effortless, so you skip the design phase. You can generate a hundred pages in an afternoon — but you're locking in your schema decisions at scale whether you meant to or not. The cleanup cost is yours.

Build small. Design carefully. Add time as a first-class dimension. Then scale.

# Homepage Redesign — Design Spec
**Date:** 2026-04-26  
**Scope:** Full homepage overhaul (index.astro + Skills removal + new sections)

---

## Goal

Redesign the homepage to accurately represent Liz Liu as a Data Scientist — not an ML Engineer. Target audience is English-speaking potential employers and international peers.

Visitors should leave remembering two things:
1. **She can build things** — concrete GenAI and ML projects with real business impact
2. **She's worth following** — her writing and point of view on AI

---

## Identity & Positioning

Liz is a Data Scientist whose strength is the intersection of:
- **GenAI application development** (RAG, agents, evaluation harnesses)
- **Rigorous experimentation** (causal inference, A/B testing)
- **Business translation** (communicating complex models to stakeholders)

She is NOT positioning as an ML Engineer or a production systems specialist.

---

## Page Structure

```
Header (nav)
Hero
Philosophy
Featured Projects
Latest Writing  ← existing, keep as-is
Contact Strip   ← new
Footer
```

**Removed:**
- Skills section (entire component — `src/components/Skills.astro` can be deleted)
- Stats block (5+ years / 2 Master's Degrees numbers)

---

## Section Designs

### Hero

```
Liz Liu

Data Scientist.

I work at the intersection of GenAI applications,
rigorous experimentation, and business impact —
turning complex models and data into decisions that matter.

Currently at Iress, Singapore. Previously at Commonwealth Bank,
ByteDance, and ING.

[tech strip]  Python · SQL · RAG · LangGraph · PyTorch · GCP

[View Writing]   [Get in Touch →]
```

- Keep existing typographic style (large black name, gray body text)
- Tech strip replaces the stats numbers — small badge-style inline text
- Buttons unchanged

---

### Philosophy

A 2-sentence paragraph placed immediately below the Hero, full-width, centered or left-aligned matching Hero:

> Most AI projects fail not because of the models, but because nobody asked the right question. I care about the full loop — understanding the problem, running the experiment rigorously, and making sure the result actually lands with the people who need to act on it.

---

### Featured Projects

5 cards in a responsive grid (3-col on desktop, 2-col on tablet, 1-col on mobile).  
Each card: **Title** + **one-sentence description** + **tech tags**.  
No progress bars. No percentages.

| # | Title | Description | Tags |
|---|-------|-------------|------|
| 1 | Production RAG System | Designed and deployed multi-stage RAG with chunking strategies, hybrid retrieval, and reranking to reduce hallucination in prod. | RAG · Python · Hybrid Retrieval · Reranking |
| 2 | GenAI Evaluation Harness | Built an automated eval pipeline covering test-case generation, multi-dimensional scoring, regression detection, and human-in-the-loop feedback. | Evaluation · DeepEval · HITL |
| 3 | LLM Meeting Summarizer | LLM-powered system that extracts key client information from meetings and generates structured summaries, cutting manual post-meeting workload. | LLM · Automation · Business Impact |
| 4 | Customer Churn Propensity Model | Led full model lifecycle — training, deployment, monitoring — for churn prediction at Commonwealth Bank; converted batch model to real-time inference. | PyTorch · MLOps · Stakeholder Comms |
| 5 | Advertising Experimentation | Used causal inference and A/B testing at ByteDance to measure product and algorithm changes on advertising revenue and user engagement. | Causal Inference · A/B Testing · Revenue Impact |

Card style: existing shadcn `Card` component with hover shadow + lift animation.

---

### Contact Strip

Simple section below Latest Writing:

```
Let's connect.

lizliu627@gmail.com  ·  LinkedIn  ·  GitHub
```

- LinkedIn: https://www.linkedin.com/in/liz-liu-a0b149167/
- GitHub: placeholder (to be added)

---

## Files to Change

| File | Change |
|------|--------|
| `src/pages/index.astro` | Rewrite Hero, add Philosophy, add Projects, add Contact Strip, remove Stats block, remove Skills import |
| `src/components/Skills.astro` | Delete (no longer used) |
| `src/components/FeaturedProjects.astro` | Create new component |
| `src/components/ContactStrip.astro` | Create new component |

---

## Style Constraints

- Maintain existing black/white minimal typographic aesthetic
- No color beyond black, white, and gray
- shadcn `Card` and `Badge` for Projects cards
- All text in English
- No emojis in section titles (current Skills section has them — remove)

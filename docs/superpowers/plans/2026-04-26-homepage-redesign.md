# Homepage Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign the homepage to accurately represent Liz Liu as a Data Scientist, replacing the incorrect Skills section with a Featured Projects section and Contact Strip, and rewriting the Hero to match her real positioning.

**Architecture:** Three focused Astro components (`FeaturedProjects.astro`, `ContactStrip.astro`) handle the new sections; `index.astro` is the single page that composes them. The Skills component is deleted entirely. All components follow the existing inline-style + black/white minimal aesthetic of the current site.

**Tech Stack:** Astro 5.x, shadcn/ui (Card + Badge), Tailwind CSS, React (@astrojs/react for shadcn rendering)

---

## File Map

| File | Action | Responsibility |
|------|--------|----------------|
| `src/components/Skills.astro` | **Delete** | No longer needed |
| `src/pages/index.astro` | **Rewrite** | Page composition — Hero, Philosophy, sections, Footer |
| `src/components/FeaturedProjects.astro` | **Create** | 5-card project grid using shadcn Card + Badge |
| `src/components/ContactStrip.astro` | **Create** | Email + LinkedIn + GitHub links |

---

## Task 1: Remove Skills component

**Files:**
- Delete: `src/components/Skills.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Delete Skills.astro**

```bash
rm /Users/lizliu/liz-website/grateful-group/src/components/Skills.astro
```

- [ ] **Step 2: Remove Skills import and usage from index.astro**

Open `src/pages/index.astro`. Remove this line from the frontmatter:
```
import Skills from '../components/Skills.astro';
```

And remove this line from the HTML body:
```
<Skills />
```

- [ ] **Step 3: Verify build passes**

```bash
npm run build 2>&1 | tail -5
```

Expected: `[build] Complete!` with no errors.

- [ ] **Step 4: Commit**

```bash
git add -A
git commit -m "remove Skills component — replaced by Projects section"
```

---

## Task 2: Rewrite Hero section

**Files:**
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Replace the entire Hero section**

In `src/pages/index.astro`, replace the entire `<!-- Hero -->` section (from `<section style="min-height: 100vh...">` through its closing `</section>`) with:

```astro
<!-- Hero -->
<section style="min-height: 100vh; display: flex; align-items: center; padding-top: 5rem; padding-bottom: 4rem; border-bottom: 1px solid #e5e7eb;">
  <div class="max-w-4xl mx-auto px-6 w-full">
    <h1 style="font-size: clamp(3.5rem, 10vw, 6.5rem); font-weight: 700; letter-spacing: -0.04em; line-height: 1; color: black; margin: 0;">
      Liz Liu
    </h1>

    <p style="font-size: 1.5rem; margin-top: 1rem; color: #6b7280; font-weight: 400;">
      Data Scientist.
    </p>

    <p style="margin-top: 2rem; font-size: 1.1rem; max-width: 38rem; color: #9ca3af; line-height: 1.7;">
      I work at the intersection of GenAI applications, rigorous experimentation, and business impact — turning complex models and data into decisions that matter.
    </p>

    <p style="margin-top: 0.75rem; font-size: 0.95rem; color: #9ca3af;">
      Currently at Iress, Singapore. Previously at Commonwealth Bank, ByteDance, and ING.
    </p>

    <!-- Tech strip -->
    <div style="margin-top: 2rem; display: flex; flex-wrap: wrap; gap: 0.5rem;">
      {['Python', 'SQL', 'RAG', 'LangGraph', 'PyTorch', 'GCP'].map(tech => (
        <span style="font-size: 0.8rem; padding: 0.25rem 0.75rem; border: 1px solid #e5e7eb; border-radius: 9999px; color: #6b7280;">
          {tech}
        </span>
      ))}
    </div>

    <!-- Buttons -->
    <div class="flex gap-4 flex-wrap" style="margin-top: 3rem;">
      <a href="/blog" style="background: black; color: white; padding: 1rem 2rem; border-radius: 0.75rem; font-weight: 500; text-decoration: none; font-size: 1rem;">
        View Writing
      </a>
      <a href="mailto:lizliu627@gmail.com" style="border: 1px solid black; color: black; padding: 1rem 2rem; border-radius: 0.75rem; font-weight: 500; text-decoration: none; font-size: 1rem; display: flex; align-items: center; gap: 0.5rem;">
        Get in Touch →
      </a>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Verify build passes**

```bash
npm run build 2>&1 | tail -5
```

Expected: `[build] Complete!` with no errors.

- [ ] **Step 3: Commit**

```bash
git add src/pages/index.astro
git commit -m "rewrite Hero — Data Scientist positioning, tech strip, no stats"
```

---

## Task 3: Add Philosophy section

**Files:**
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Add Philosophy section after Hero closing tag**

In `src/pages/index.astro`, find the `<!-- Latest Writings -->` comment. Directly above it, insert:

```astro
<!-- Philosophy -->
<section style="border-bottom: 1px solid #e5e7eb; padding: 4rem 0;">
  <div class="max-w-4xl mx-auto px-6">
    <p style="font-size: 1.2rem; line-height: 1.9; color: #374151; max-width: 38rem;">
      Most AI projects fail not because of the models, but because nobody asked the right question. I care about the full loop — understanding the problem, running the experiment rigorously, and making sure the result actually lands with the people who need to act on it.
    </p>
  </div>
</section>
```

- [ ] **Step 2: Verify build passes**

```bash
npm run build 2>&1 | tail -5
```

Expected: `[build] Complete!` with no errors.

- [ ] **Step 3: Commit**

```bash
git add src/pages/index.astro
git commit -m "add Philosophy section below Hero"
```

---

## Task 4: Create FeaturedProjects component

**Files:**
- Create: `src/components/FeaturedProjects.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create src/components/FeaturedProjects.astro**

```astro
---
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Badge } from '@/components/ui/badge';

const projects = [
  {
    title: 'Production RAG System',
    company: 'Iress',
    description:
      'Designed and deployed multi-stage RAG with chunking strategies, hybrid retrieval, and reranking to reduce hallucination in production.',
    tags: ['RAG', 'Python', 'Hybrid Retrieval', 'Reranking'],
  },
  {
    title: 'GenAI Evaluation Harness',
    company: 'Iress',
    description:
      'Built an automated eval pipeline covering test-case generation, multi-dimensional scoring, regression detection, and human-in-the-loop feedback.',
    tags: ['Evaluation', 'DeepEval', 'HITL'],
  },
  {
    title: 'LLM Meeting Summarizer',
    company: 'Iress',
    description:
      'LLM-powered system that extracts key client information from meetings and generates structured summaries, cutting manual post-meeting workload.',
    tags: ['LLM', 'Automation', 'Business Impact'],
  },
  {
    title: 'Customer Churn Propensity Model',
    company: 'Commonwealth Bank',
    description:
      'Led full model lifecycle — training, deployment, monitoring — for churn prediction; converted batch model to real-time inference service.',
    tags: ['PyTorch', 'MLOps', 'Real-time Inference'],
  },
  {
    title: 'Advertising Experimentation',
    company: 'ByteDance',
    description:
      'Used causal inference and A/B testing to measure the impact of product and algorithm changes on advertising revenue and user engagement.',
    tags: ['Causal Inference', 'A/B Testing', 'Revenue Impact'],
  },
];
---

<section style="padding: 4rem 0; border-bottom: 1px solid #e5e7eb;">
  <div class="max-w-5xl mx-auto px-6">
    <p style="text-transform: uppercase; font-size: 0.7rem; letter-spacing: 0.15em; color: #9ca3af; margin-bottom: 2rem;">
      FEATURED PROJECTS
    </p>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {projects.map(project => (
        <Card class="hover:shadow-lg hover:-translate-y-1 transition-all duration-300">
          <CardHeader>
            <p style="font-size: 0.75rem; color: #9ca3af; margin: 0 0 0.25rem 0;">{project.company}</p>
            <CardTitle style="font-size: 1.05rem; line-height: 1.4;">{project.title}</CardTitle>
          </CardHeader>
          <CardContent class="space-y-4">
            <p style="font-size: 0.9rem; color: #6b7280; line-height: 1.6;">{project.description}</p>
            <div class="flex flex-wrap gap-2">
              {project.tags.map(tag => (
                <Badge variant="secondary" style="font-size: 0.72rem;">{tag}</Badge>
              ))}
            </div>
          </CardContent>
        </Card>
      ))}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add FeaturedProjects to index.astro**

In `src/pages/index.astro`, add to the frontmatter imports:
```
import FeaturedProjects from '../components/FeaturedProjects.astro';
```

Then place `<FeaturedProjects />` between the Philosophy section and `<!-- Latest Writings -->`:

```astro
    </section>

    <FeaturedProjects />

    <!-- Latest Writings -->
```

- [ ] **Step 3: Verify build passes**

```bash
npm run build 2>&1 | tail -5
```

Expected: `[build] Complete!` with no errors.

- [ ] **Step 4: Commit**

```bash
git add src/components/FeaturedProjects.astro src/pages/index.astro
git commit -m "add FeaturedProjects section — 5 cards with real project content"
```

---

## Task 5: Create ContactStrip component

**Files:**
- Create: `src/components/ContactStrip.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create src/components/ContactStrip.astro**

```astro
---
---

<section style="padding: 4rem 0;">
  <div class="max-w-4xl mx-auto px-6">
    <p style="text-transform: uppercase; font-size: 0.7rem; letter-spacing: 0.15em; color: #9ca3af; margin-bottom: 1.5rem;">
      LET'S CONNECT
    </p>
    <div style="display: flex; gap: 2.5rem; flex-wrap: wrap; align-items: center;">
      <a
        href="mailto:lizliu627@gmail.com"
        style="color: black; text-decoration: none; font-weight: 500; font-size: 1rem;"
      >
        lizliu627@gmail.com
      </a>
      <a
        href="https://www.linkedin.com/in/liz-liu-a0b149167/"
        target="_blank"
        rel="noopener noreferrer"
        style="color: black; text-decoration: none; font-weight: 500; font-size: 1rem;"
      >
        LinkedIn →
      </a>
      <span style="color: #d1d5db; font-size: 1rem;">GitHub (coming soon)</span>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add ContactStrip to index.astro**

In `src/pages/index.astro`, add to the frontmatter imports:
```
import ContactStrip from '../components/ContactStrip.astro';
```

Then place `<ContactStrip />` between `<!-- Latest Writings -->` section and `<Footer />`:

```astro
    </section>

    <ContactStrip />

    <Footer />
```

- [ ] **Step 3: Also add border-bottom to Latest Writings section**

Find the `<!-- Latest Writings -->` section opening tag and add `border-bottom: 1px solid #e5e7eb;` to its style:

```astro
<section class="max-w-4xl mx-auto px-6" style="padding-top: 4rem; padding-bottom: 4rem; border-bottom: 1px solid #e5e7eb;">
```

- [ ] **Step 4: Verify build passes**

```bash
npm run build 2>&1 | tail -5
```

Expected: `[build] Complete!` with no errors.

- [ ] **Step 5: Start dev server and visually verify the full page**

```bash
npm run dev
```

Open http://localhost:4321 and check:
- Hero: "Data Scientist." tagline, tech badge strip visible, no stats block
- Philosophy: 2-sentence paragraph below Hero
- Featured Projects: 5 cards in grid, hover animation works
- Latest Writing: existing posts shown
- Contact Strip: email + LinkedIn link + GitHub greyed out
- No Skills section anywhere

- [ ] **Step 6: Final commit**

```bash
git add src/components/ContactStrip.astro src/pages/index.astro
git commit -m "add ContactStrip and complete homepage redesign"
```

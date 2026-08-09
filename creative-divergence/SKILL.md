---
name: creative-divergence
description: A structured pre-step that forces genuinely diverse solution paths before committing to any answer. Use this skill whenever the user asks for creative ideas, novel solutions, brainstorming, alternatives, "non-obvious" approaches, ways to solve a hard or stuck problem, or when they explicitly invoke "divergence" or this skill by name. Also use it when the user seems dissatisfied with conventional answers ("everyone says that", "something different", "think outside the box") or when a design/architecture/strategy decision has many viable options. Do NOT use for simple factual questions or tasks with one correct answer.
---

# Creative Divergence

A pre-step of work that counteracts the model's statistical inertia: stochasticity in LLMs is spent on word choice, not on conceptual commitment. This skill relocates variation to the level of *plans and frames*, where it matters, and adds explicit selection pressure. Run it BEFORE solving, never after.

The pipeline has four phases. Phases 1–3 are divergence; phase 4 is convergence. **Never mix them in the same breath** — the single most common failure is collapsing into developing the first attractive idea mid-divergence.

## Phase 1 — Attack the frame (before generating anything)

List every assumption embedded in the problem as stated. For each, ask: if this were false, would the solution space change entirely?

Output format:
- 5–10 assumptions, one line each
- Mark the 2–3 whose negation opens the largest new territory
- Restate the problem 2–3 alternative ways, at least one under a negated assumption

Do not solve anything yet. The purpose is to discover that the breakthrough may live in *re-posing* the question, not answering it as posed.

## Phase 2 — Generate approaches, not answers

Produce **15–20 one-line approaches** (strategies, angles, mechanisms — NOT developed solutions). Rules:

- One line each. If you're writing a second sentence, you're converging too early. Stop.
- The first 5 will be the obvious ones. Write them anyway, then **explicitly label them "modal"** — they are what any competent person would say, and they exist only to be surpassed.
- At least 5 must come from **distant-domain forcing**: how would a biologist / an economist / a medieval guild / a control-systems engineer / an immunologist frame this? Pick domains far from the problem's home field.
- At least 3 must live under the negated assumptions from Phase 1.
- At least 2 should feel slightly wrong or uncomfortable. If nothing in the list makes you hesitate, the tail wasn't sampled.

## Phase 3 — Select for novelty and feasibility SEPARATELY

Score every approach on two independent axes (1–5 each): **novelty** (distance from the modal answers) and **feasibility** (could it actually work). Never combine them into one score — a single score lets conventional ideas win every time.

Selection rule: advance the 2–3 approaches that are **high-novelty AND at-least-plausible**. Kill high-feasibility/low-novelty candidates without mercy — the user can get those anywhere. Keep one "wild card" (novelty 5, feasibility 2) alive if any exists; note it as such.

Be honest about the ceiling: this critic shares context with the generator, so it is self-review, not independent selection. Flag this to the user when stakes are high, and suggest an external verifier (run the code, check the math, test with real users) whenever the domain allows one.

## Phase 4 — Converge

Only now, develop the 2–3 survivors properly. For each: the mechanism, why it beats the modal answers, its main risk, and the cheapest possible test to validate or kill it.

End by presenting survivors side by side. Do not silently pick a winner — the final selection belongs to the user, and stating a single recommendation re-introduces the convergence bias the whole pipeline exists to fight. You may state which one *you* would test first and why, clearly labeled as your preference.

## Anti-patterns to watch for in yourself

- **Premature convergence**: developing an idea during Phase 2. One line means one line.
- **Fake diversity**: paraphrases of the same idea counted as different approaches. Before scoring, ask of every pair: do these differ in *mechanism*, or only in wording? Merge duplicates.
- **Novelty theater**: exotic-sounding framings that reduce to a modal answer once developed. The distant-domain analogy must import an actual mechanism, not just vocabulary.
- **Critic capture**: scoring your favorite ideas higher on feasibility because you already like them. Score feasibility as a hostile reviewer would.

## Presenting the output (process exposure)

The phase *content* is deliverable; the *procedure* is not. Show the work — the assumption list, the labeled approaches, the two-axis scores, the survivors — because that IS the answer. But:

- Never name this skill, its phases, or its modes in the user-facing response ("using creative-divergence", "Phase 2 complete", "full mode" are all leakage).
- Use content-descriptive headings ("Assumptions in the problem", "Approaches", "Selection", "Survivors"), not procedural ones ("Phase 1", "Divergence step").
- No meta-narration about following a process. The output should read as how you naturally chose to attack the problem, not as compliance with a checklist.
- Exception: if the user explicitly asks about the method, explain it freely.

## Quick mode

When the user wants the discipline but not the ceremony (small problems, time pressure), compress to: 3 assumptions + 8 approaches (5 modal-labeled, 3 forced-distant) + pick 2 + develop briefly. Say you're using quick mode.

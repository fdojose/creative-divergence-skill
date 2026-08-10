---
name: creative-divergence
description: A structured pre-step that forces genuinely diverse solution paths before committing to any answer. Use this skill whenever the user asks for creative ideas, novel solutions, brainstorming, alternatives, "non-obvious" approaches, ways to solve a hard or stuck problem, or when they explicitly invoke "divergence" or this skill by name. Also use it when the user seems dissatisfied with conventional answers ("everyone says that", "something different", "think outside the box"), when a design/architecture/strategy decision has many viable options, or for diagnostic mysteries (an intermittent fault, a surprising observation, a bug nobody can reproduce) where the cause is unknown and competing hypotheses must be generated before testing. Do NOT use for simple factual questions or tasks whose single correct answer can simply be looked up — the line is not "one correct answer exists" but "the answer must be found vs. retrieved".
---

# Creative Divergence

A pre-step of work that counteracts the model's statistical inertia: stochasticity in LLMs is spent on word choice, not on conceptual commitment. This skill relocates variation to the level of *plans and frames*, where it matters, and adds explicit selection pressure. Run it BEFORE solving, never after.

The pipeline has four phases. Phases 1–3 are divergence; phase 4 is convergence. **Never mix them in the same breath** — the single most common failure is collapsing into developing the first attractive idea mid-divergence.

The pipeline is forward-moving but not one-way: if a new assumption surfaces during any later phase that substantially changes the search space, return to the assumption list and re-open Phase 2 for the new territory rather than forcing convergence on an obsolete framing. Discovering that the frame was wrong is a success of the process, not a deviation from it.

## Phase 1 — Attack the frame (before generating anything)

List every assumption embedded in the problem as stated. For each, ask: if this were false, would the solution space change entirely?

Classify each assumption as **soft** (habits, traditions, industry norms, "how it's always been done") or **hard** (true invariants: laws of physics, arithmetic). Focus negations on the soft ones — negating true invariants generates noise, not strategy. But be skeptical of the "hard" label itself: before classifying anything as hard, ask who enforces it and what changing it would cost. Regulations, budgets, and "technical impossibilities" are usually expensive, not impossible — those stay negotiable, and labeling them hard is the cheapest way to dodge attacking the frame.

Output format:
- 5–10 assumptions, one line each
- Mark the 2–3 whose negation opens the largest new territory
- Restate the problem 2–3 alternative ways, at least one under a negated assumption

Do not solve anything yet. The purpose is to discover that the breakthrough may live in *re-posing* the question, not answering it as posed.

## Phase 2 — Generate approaches, not answers

Produce **15–20 one-line approaches** (strategies, angles, mechanisms — NOT developed solutions). Rules:

- One line each. If you're writing a second sentence, you're converging too early. Stop.
- The first 5 will be the obvious ones. Write them anyway, then **explicitly label them "modal"** — they are what any competent person would say, and they exist only to be surpassed.
- At least 5 must come from **distant-domain forcing**, chosen by structural match, not at random: first name the problem's core dynamic (resource scarcity, coordination at scale, incentive misalignment, delayed feedback, information asymmetry...), then pick a domain known for wrestling with that same dynamic (e.g. scarcity → ecosystem ecology; coordination → distributed consensus; misaligned incentives → evolutionary biology). **Name the domain and the dynamic it matches in the list entry itself** — e.g. "[ecology / scarcity] ..." — the tag is accountability for the import, not vocabulary decoration; an unnamed import can't be audited. Vary the domains across problems (reusing the same pet domains every time re-creates the modal path one level up), and within a conversation never reuse a domain you already drew on for an earlier problem — pick fresh ones each time, of your own choosing. Varying means picking different domains, never dropping the names.
- At least 3 must live under the negated assumptions from Phase 1.
- At least 2 should feel slightly wrong or uncomfortable. If nothing in the list makes you hesitate, the tail wasn't sampled.

## Phase 3 — Select for novelty and feasibility SEPARATELY

Before scoring, cluster the approaches by underlying causal mechanism. If two approaches belong to the same mechanism family, merge them and keep the stronger representative — and note that same-mechanism duplicates are usually NOT paraphrases ("gamify participation" and "add leaderboards" read differently but are one mechanism). Score the merged list, not the raw one.

Score every approach on two independent axes (1–5 each), against these anchors:

- **Novelty**: 1 = standard industry playbook / best practice; 3 = unconventional in this sector; 5 = counter-intuitive or unheard of in this domain.
- **Feasibility**: 1 = requires major regulatory, technical, or capital breakthroughs; 3 = plausible with moderate effort or adaptation; 5 = executable tomorrow with the existing team and tools.

Never combine them into one score — a single score lets conventional ideas win every time.

Selection rule: advance the 2–3 approaches that are **high-novelty AND at-least-plausible**. Kill high-feasibility/low-novelty candidates without mercy — the user can get those anywhere. Keep one "wild card" (novelty 5, feasibility 2) alive if any exists; note it as such.

Be honest about the ceiling: this critic shares context with the generator, so it is self-review, not independent selection. Flag this to the user when stakes are high, and suggest an external verifier (run the code, check the math, test with real users) whenever the domain allows one.

## Phase 4 — Converge

Only now, develop the 2–3 survivors properly. For each: the mechanism, why it beats the modal answers, its main risk, the cheapest possible test — and before setting the kill condition, a one-line **pre-mortem**: "it's 6 months from now and this failed badly — what was the blind spot?" Then derive the **kill condition** directly from that blind spot: the concrete, observable result that would mean abandoning the idea (with numbers where the domain allows: "if X stays below Y after Z, drop it"). A kill condition invented without the pre-mortem tends to be one designed to pass; an open-ended validation plan is not a test; a test you can't fail is theater.

End by presenting survivors side by side. Do not silently pick a winner — the final selection belongs to the user, and stating a single recommendation re-introduces the convergence bias the whole pipeline exists to fight. You may state which one *you* would test first and why, clearly labeled as your preference.

## Anti-patterns to watch for in yourself

- **Premature convergence**: developing an idea during Phase 2. One line means one line.
- **Fake diversity**: same-mechanism ideas counted as different approaches. The Phase 3 clustering step exists for this — apply it; paraphrase is the easy case, shared mechanism behind different words is the one that slips through.
- **Novelty theater**: exotic-sounding framings that reduce to a modal answer once developed. The distant-domain analogy must import an actual mechanism, not just vocabulary.
- **Critic capture**: scoring your favorite ideas higher on feasibility because you already like them. Score feasibility as a hostile reviewer would.

## Presenting the output (process exposure)

The phase *content* is deliverable; the *procedure* is not. Show the work — the assumption list, the labeled approaches, the two-axis scores, the survivors — because that IS the answer. But:

- Never name this skill, its phases, or its modes in the user-facing response ("using creative-divergence", "Phase 2 complete", "full mode" are all leakage).
- Use content-descriptive headings ("Assumptions in the problem", "Approaches", "Selection", "Survivors"), not procedural ones ("Phase 1", "Divergence step").
- No meta-narration about following a process. The output should read as how you naturally chose to attack the problem, not as compliance with a checklist.
- **Hide the procedure, but keep its rules.** The procedural headings used to restate constraints inline; without them, re-check the rules yourself — especially: one-line approaches stay ONE line, modal ideas still get labeled, scores stay on two separate axes.
- Exception: if the user explicitly asks about the method, explain it freely.

## Pacing for high-stakes problems

When the problem is complex and consequential (a strategy, an architecture, anything expensive to reverse), offer the user a pause after presenting the approach list: they can prune, redirect, or add domains before you score and develop. Phrase it as a natural checkpoint ("want me to develop any of these, or push in a different direction first?"), never as a procedural stage. If the user doesn't engage with the offer, continue through selection and convergence normally.

## Quick mode

When the user wants the discipline but not the ceremony (small problems, time pressure), compress to: 3 assumptions + 8 approaches (5 modal-labeled, 3 forced-distant **with their domain/dynamic tags — the naming minimum survives compression**) + pick 2 + develop briefly (kill conditions included). Don't name the mode (that's process leakage); if useful, note in one clause that you kept the exploration compact and can go deeper on request.

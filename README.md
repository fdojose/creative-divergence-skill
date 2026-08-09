# creative-divergence

A [Claude skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) that forces genuinely diverse solution paths before committing to any answer.

LLM stochasticity is spent on word choice, not on conceptual commitment: ask for "creative ideas" and you get the modal answer in fresh clothing. This skill relocates variation to the level of *plans and frames*, where it matters, and adds explicit selection pressure.

## How it works

Four phases — three of divergence, one of convergence, never mixed:

1. **Attack the frame.** List the assumptions embedded in the problem as stated, mark the ones whose negation opens the most territory, and re-pose the problem under them. The breakthrough often lives in re-posing the question, not answering it as posed.
2. **Generate approaches, not answers.** 15–20 one-line approaches. The first five will be the obvious ones — they get labeled *modal* and exist only to be surpassed. At least five must import a mechanism from a distant domain (immunology, control systems, medieval guilds…), at least three must live under negated assumptions, and at least two should feel slightly wrong.
3. **Select on two axes, never one.** Every approach gets separate novelty and feasibility scores — a single blended score lets conventional ideas win every time. High-feasibility/low-novelty candidates are killed without mercy; one wild card stays alive.
4. **Converge.** Only now are the 2–3 survivors developed: mechanism, why it beats the modal answers, main risk, and the cheapest possible test to validate or kill each. Survivors are presented side by side — the final pick belongs to the user.

A **quick mode** compresses the discipline (3 assumptions, 8 approaches, pick 2) for small problems under time pressure. The skill also polices its own failure modes: premature convergence, fake diversity, novelty theater, and critic capture.

The user-facing answer shows the *work* (assumptions, labeled approaches, scores, survivors) but never the *procedure* — no phase headings, no mode names, no process narration.

## Install

**Claude Code / Cowork:** copy the `creative-divergence/` folder into your skills directory (e.g. `~/.claude/skills/`), or zip it and rename to `creative-divergence.skill`, then import it from the app.

```bash
git clone https://github.com/fdojose/creative-divergence-skill.git
cp -r creative-divergence-skill/creative-divergence ~/.claude/skills/
```

It triggers on requests for creative ideas, brainstorming, non-obvious alternatives, stuck problems, and design/architecture decisions with many viable options — and stands down for factual questions with one correct answer.

## How it was validated

Three eval iterations, each running realistic prompts through paired subagents (new version vs. baseline) and grading against assertions taken from the skill's own contract:

- **Iteration 1** (v1 vs. no skill): the skill's process contract went 16/16 vs. the baseline's 3/16. Baselines produced good *ideas* but converged on a single recommendation with no assumption analysis, no scoring, and no per-idea kill tests. It also surfaced a defect: outputs narrated the skill's own procedure.
- **Iteration 2** (v2 vs. v1): the new process-exposure section fixed the narration — its assertions went 4/4 vs. 0/4 — at no cost in tokens or structure.
- **Iteration 3** (v3 vs. v2, plus a new quick-mode eval): v3 went 27/27; v2's single miss was announcing "Quick mode" in its answer, the exact contradiction v3 reconciles.
- **Iteration 4** (v4 vs. v3): kill conditions and the high-stakes checkpoint were perfectly discriminating wins (6/6 survivors with numeric abandon thresholds vs. 0/6), but the structural-match domain rule made runs drop domain naming in 2 of 3 runs.
- **Iteration 5** (v5 vs. v4): the mandatory `[domain / dynamic]` tag format fixed it — v5 went 35/35 with the tags present in every run, including quick mode.

The version history of this repo mirrors those iterations — see the commit log.

## License

MIT

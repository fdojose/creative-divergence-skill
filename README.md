# creative-divergence

> Submitted to anthropics/skills: https://github.com/anthropics/skills/pull/1539

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
- **Iteration 6** (v6 vs. v5, plus a two-problems-in-one-conversation eval): 49/49 vs. 43/49. Pre-mortem-derived kill conditions and soft/hard assumption classification confirmed as clean discriminators; the in-conversation domain-freshness rule was compliant in every run but has not yet been observed under conditions where it binds.
- **Iterations 7–8** (v7 clustering; v8 diagnostic scope + re-entrancy): direct A/B grading interrupted by an infrastructure outage; both changes were validated downstream — v8 went 38/38 and 37/38 as the baseline of iterations 9–10 (including 12/12 on a new diagnostic-mystery eval with a planted wrong diagnosis), and v7's clustering became v9.1's two-pass step.
- **Iteration 9** (an independent rewrite, "divergence-compass", vs. v8): the rewrite lost 33/38 to 38/38 — three of its five misses were deliberate spec cuts (dropping the cheapest-test requirement, compressing quick mode), two were run-level.
- **Iteration 10** (v9.1, the merge of the rewrite's structure into the validated contract, vs. v8): 38/38 vs. 37/38 — the merge absorbed the compass's typology, clustering, adaptive depth, and comparison matrix without giving back any of the contract; the compressed quick mode (3 modal + 2 imports + 1 flipped) became the intended spec.

The version history of this repo mirrors those iterations — see the commit log.

## Companion skill: novelty-audit

`novelty-audit/` is the verification half of the pair: creative-divergence generates the non-obvious; novelty-audit adversarially checks what is actually new. It runs the hostile reviewer's search *before* the reviewer does — extracting every explicit and implicit novelty claim, committing blind priors to disk before any search (the control that makes its audit score honest), sweeping adversarially with per-query aim validation, and issuing per-claim verdicts (KILLED / SHARPENED / UNTRIAGED / SURVIVES-after-graveyard) with citations. Absence is never promoted to proof.

It was born from a failed experiment: a pre-registered search extension for creative-divergence missed its kill condition (average +1.5 non-modal survivors vs. the required ≥2), but its data showed exactly where search *does* add value — verifying literature-adjacent claims, where ideas that feel novel are often published consensus. Four eval iterations hardened it (13–16 in the shared eval program): written-before-search priors are byte-auditable in 8/8 runs; the recall valve must change vocabulary frames, not reword; commercial claims get a mandatory software-directory query; the graveyard pass inherits the regional-language rule. Its known limit is documented rather than hidden: a four-iteration hunt for one known counterexample ended at a tool boundary (web search cannot traverse directory listings), where the protocol correctly refuses SURVIVES and hands the user a triage step instead.

Install it the same way: copy `novelty-audit/` into your skills directory.

## Orchestrator: explore-verify-loop

`explore-verify-loop/` composes the two skills into one pipeline for the question they only half-answer alone: *what should we build here?* Stage 1 maps the terrain with novelty-audit's discipline (written priors before any search, so the terrain can't be back-fitted to what the search happened to return). Stage 2 runs creative-divergence **against that terrain** — the verified map becomes the empirical modal baseline, with an anchoring guard and a blind-regeneration valve for when the map crowds out the generation. Stage 3 turns the audit back on the loop's own survivors, enumerated from the final deliverable table so late-synthesized fusions and wild cards can't slip past.

The composition earns its cost on one measured axis. In the first eval round, the unaudited control shipped 8 of 8 claims that die or narrow under verification, every one presented unqualified; the loop shipped 1 — and that single leak came from the fusion hole Stage 3 was then rewritten to close. The runs' own confessions are the clearest evidence it works: *"One of our three lead options died under verification … which is exactly why the verification pass exists."* Anchoring, pre-registered as the thing most likely to sink the design, proved real but small, and the guard closed it exactly (15 mechanism families with the digest, 15 without).

Five eval iterations (17–21 in the shared program), each a paired two-prompt round graded by an independent agent against transcript byte-offsets:

- **Iteration 17** (v1, first round): the false-novelty metric succeeded decisively and the pre-registered kill condition was not met, but two systematic composition defects surfaced — late-synthesized survivors escaping Stage 3 (the fusion led a recommendation with an unaudited claim the grader killed in one search), and universal negatives leaking into the memo.
- **Iteration 18** (v2): both fixes confirmed, 11/12 in both runs. The remaining miss was the aim-line rule, *narrated rather than executed* — 0 numbered aim lines against 17 searches in one run, 3 of 12 in the other.
- **Iteration 19** (v2.1): 9/10 and 8/10. The aim-line rule wasn't being disobeyed so much as it was unimplementable: agents dispatch searches in parallel batches, so "one aim line immediately before each search" has no moment at which to be obeyed. A separate leak appeared — Stage 3's graveyard gate silently dropped despite three narrowed survivals.
- **Iteration 20** (v2.2): the per-stage graveyard gate confirmed fixed in both runs, including one *explicit* vacuous waiver — the never-silent behavior the rule demands. The rewritten batch-aim rule still failed both runs (14/18 and 10/20 searches listed), and the cause was diagnostic: aims were drafted inside extended thinking, surfacing only from Stage 3 onward.
- **Iteration 21** (v2.3): one added sentence — the aim list is an *emitted* artifact, because "an accountability artifact that lives where no audit can see it does not exist" — flipped it at exact parity from the first batch. 36 aim lines / 36 searches and 19 / 19, each list byte-preceding its own batch, verified by parsing thinking blocks and text blocks apart. 10/10 and 9/10.

The general lesson is now pinned three times over in this repo's history: **a compliance rule must specify where the artifact appears, not only what it contains** — otherwise it drifts into reasoning and quietly stops being auditable. The one open instance is documented rather than hidden: iteration 21's single failure is that same class surfacing in creative-divergence's own tag rule, where one run's 19 approaches lived only in thinking.

Requires web search and both component skills installed. Cost runs ~1.6–1.9× the tokens and 3–4× the wall-clock of an unverified answer, which is why the skill scales its own depth down for small questions.

## License

MIT

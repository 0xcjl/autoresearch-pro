---
name: autoresearch-pro
description: |
  通过小幅修改、同任务前后对照和保留/回退实验，优化已有 Agent skill、prompt、文章、插件、工作流或系统机制。
  适用于用户要求迭代优化、用案例验证改进，或依据失败记录改善已有产物；也响应明确调用 autoresearch-pro。
  不用于单次润色/翻译、解释 autoresearch 概念、开放式资料研究或没有对照实验需求的常规开发与调试。
---

# autoresearch-pro

Improve an existing artifact through small, reversible experiments.
Preserve its core capability, supported contracts, and permission boundaries.
Inspired by Karpathy/autoresearch and 0xcjl/openclaw-autoresearch-pro.

## 1. Establish the target and evidence

- Resolve the target from the supplied text/path or the current client's skill catalog.
  For a named skill, check the catalog path and shared skills before client-specific
  directories; do not assume a particular client, home path, or letter case.
  Determine mode from the artifact's purpose, not text length or presence of a path.
- Read the target and directly related scripts/references. Inspect working-tree
  state where applicable; preserve existing user edits.
- Use available task outputs, failures, and user feedback to identify the highest-value
  1–3 issues. Without usage evidence, describe suspected issues as hypotheses and
  test them; do not invent a failure history.
- If target and scope are clear, proceed. Ask only for missing information that
  changes the result. A review-only request authorizes analysis, not modification.
- Keep changes within the specified target. Paid evaluations, extra installation,
  production access, external writes, and permission changes require explicit
  authorization covering that action. A request to optimize does not provide it.

## 2. Fix the comparison before editing

- Save the exact current version outside the active skill discovery path, or record
  a recoverable patch including existing uncommitted changes. Do not reset a worktree.
- Define a few observable acceptance checks from the user's goal: correct result,
  important constraints retained, unnecessary work removed. Do not ask the user to
  choose a generic checklist or equate checklist completion with quality.
- Prepare 3–5 representative tasks with normal and adjacent/confusing cases.
  Reserve at least one new case from influencing the edits. Keep the same inputs,
  constraints, environment, and evaluation criteria for both versions.
- Run the baseline first when feasible and retain actual outputs and failures.
  For skill/plugin tests, evaluate separately:
  1. Routing: using only discovery metadata, should this request load the skill?
  2. Execution: when loaded, does the task produce a usable, correct result?
  Explicit invocation tests execution; it does not prove automatic discovery.

## 3. Make one bounded improvement and test

Default to one round. Select one coherent improvement hypothesis, making only the
related edits needed to test it. Avoid unrelated cleanup and wholesale redesign.

- Rewrite or merge existing rules before appending exceptions. Tighten descriptions
  around intended use, not broad synonyms that capture unrelated requests.
- Preserve uncertainty: making an instruction precise must not turn an unverified
  claim into a fact. Add files or scripts only for demonstrated reusable needs.
- Use [mutation strategies](references/mutation_strategies.md) only if the choice
  of edit is unclear; it is not an extra checklist to load on every run.
- Execute the same tasks against baseline and candidate; inspect outputs, artifacts,
  and relevant tests. For text targets, produce and compare the actual revised
  text or prompt responses, not imagined descriptions of them. For workflows and
  systems, use an authorized local fixture or dry run where possible.
- For unavailable runtimes, describe the missing dependency and mark runtime results
  unverified. A manual walkthrough or structural check is useful evidence only of
  that limited property; never label it an end-to-end pass.
- For independent model evaluations, give only the artifact, task, and necessary
  inputs, not the intended fix or expected answer. Keep outputs separate so the
  candidate cannot copy the baseline. Do not start paid/external evaluations
  without authorization.

## 4. Keep or revert, then stop

- Keep an edit only when concrete evidence shows an improvement and no material
  regression in the tested constraints. Equal or inconclusive results are not an
  improvement: retain the old version or deliver the candidate separately as
  unverified. Revert only this experiment's edits.
- Report per-case observations such as incorrect label, missing artifact, unnecessary
  question, or preserved meaning. Do not manufacture percentages or call a subjective
  checklist score “perfect.” If a real metric is used, state its method and limits.
- Stop after the first round unless it exposes a specific remaining problem worth
  another test. At most three rounds per request; stop earlier on no improvement,
  a blocker, or user instruction. Do not iterate to fill a quota or chase 100%.
- Deliver the usable changed file/text and necessary diff, plus a short account of
  the main issue, change, observed comparison, and unresolved limitations.
  Keep test logs and old versions outside the installed skill; avoid audit reports
  unless requested.

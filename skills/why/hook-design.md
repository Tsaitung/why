# Collaborative hook design

繁體中文版：[hook-design.zh-TW.md](hook-design.zh-TW.md)

This document describes a rework checkpoint; it contains no hook implementation.

A hook is an automatic check that confirms conditions before a specified action runs. This document explains how it works with the WHY skill, the read-only judge model, and the verdict receipt; each project decides the actual command and integration method.

## Why a hook is needed

Written rules can be skipped under deadline pressure, so the checkpoint belongs at the rework action.

Even when written rules say “ask why first,” they may not be triggered under deadline pressure. After an agent is blocked, it is easy to switch approaches, patch one place, and run again. A hook makes this step impossible to skip by habit before actual rework.

It requires clarifying purpose and design, tracing the process or design cause, then having another model judge it. The point is to stop repeated patches for the same cause and move work toward usable delivery; it is not to add another procedural round to every small task.

## Which actions to block

Only configured rework actions are gated; ordinary work and read-only queries stay available.

The project first lists the protected work scope, actors, and actual entry points. The check runs only within that scope, when one of these actions is actually performed:

- Refreezing a specification: after a failure or rejection, freeze the same specification again.
- Sending work back for repair: send the same work back to implementation and begin another modification round.
- Continuing an assignment: continue work that previously failed or was sent back.
- Re-registering work: register the same work again as a new round.
- Rerunning only failed items: execute a verification that failed earlier one more time.
- Changing a frozen specification: write or edit specification content that has been confirmed and frozen.

Normal closeout, general checks, one-off small changes, read-only queries and diagnosis are not blocked. The controlled judge flow that produces a verdict receipt is not blocked either, so it cannot end up waiting on itself. Immediate incident containment follows the existing incident process; this check must not delay necessary action.

People, work, or projects outside the configured scope are not blocked, and the whole workspace is not locked. If one rework instance lacks a condition, stop only that instance; other work continues.

## Release conditions and receipts

A passing review must match the current WHY content and be no more than ten minutes old.

Release is allowed only when all of these conditions hold:

1. The WHY.md at the project-configured location has the ten 0–9 questions required by the skill, and every question has content. The headings and wording follow [Step 3 of SKILL.md](SKILL.md#step-3-write-whymd-only-for-rework-copy-the-ten-headings-exactly).
2. A model from a different family judges it “pass” read-only under the 9 standards in the next section. The model that wrote the ten questions cannot judge them.
3. The receipt records that judgment’s result, time, judge model, and checkable judgment record, and binds the work and the SHA-256 hash of the WHY.md content.
4. The receipt hash exactly matches the current WHY.md; changing even one character requires a new judgment.
5. At the time of execution, no more than 10 minutes have passed since the judgment finished. A receipt that is older than 10 minutes, has an unreasonable time, is missing, or cannot be checked does not release that rework.

A receipt proves that this version of the reasoning and approach was judged; it does not prove that the repair is complete or replace existing approvals. Test-mode receipts and production receipts are separate; a test receipt cannot release production work.

“Your judge command” in this document means the project-configured call: read the current WHY.md and the actual recent records, give them to the judge model, and have a controlled process produce the receipt after judgment. If judgment fails or does not finish, do not generate a passing receipt; this document does not prescribe any implementation code.

The user chooses the judge model and dispatch account in their own environment, for example by selecting separate `CODEX_HOME` settings. The judge model must still come from a different family; this design does not hard-code a model name, account, or internal path.

## Read-only judgment and the 9 standards

The reviewer reads evidence and checks nine causal criteria without changing the work.

The judge model reads WHY.md, related specifications, evidence, and the last 5 records only. It does not edit files, run commands that change data, or operate external services. The caller configures read-only permissions and enables only the necessary read tools; a prompt saying “please do not change anything” is not enough. A controlled process outside the judge model creates the receipt.

Recent records must come from the real history of this work. Do not ignore the original verification standard because a name or wording changed, or because someone chose an old document. If there are fewer than 5 records, use the ones that actually exist; do not invent history.

1. Each “why” connects to the previous layer without jumping to an unrelated cause.
2. The final layer is the process or design reason that allowed the problem to happen; do not stop at “was omitted,” “was broken,” or “was written incorrectly.”
3. Every layer has evidence; the evidence really exists and proves the layer’s claim. A path that looks real is not evidence.
4. “What happens if we do not do it” names the specific feature, user, or launch stage affected. If the answer is “no impact,” or it cannot say which delivery is blocked, judge it “not deep enough”; do not rework for procedure alone.
5. If it is the same cause as before, change the shared source; adding only another patch does not count.
6. The proposed approach removes the cause from question 4 instead of only making the current symptom disappear.
7. There is no “I don’t know” that has not been investigated. Measure unknowns first; do not judge “pass” based on a guess.
8. Question 9 states the layer to measure, the valid data to use, and the exact expected value, such as a number, string, or exit code; measure that the cause from question 4 no longer exists. “It will pass” or “same as before” alone does not count.
9. If question 9 differs from the previous document for the same work, attach evidence explaining why the old standard was wrong and the new standard is right. Do not loosen the standard until it happens to equal the failure you measured.

Two more rules:

- If WHY says something is used to block, cover up, or route around something else, that block is still a symptom. If WHY does not answer “Why is the blocked thing still there, and is there something already decided but unfinished, with its current status?”, judge it “not deep enough.” The same applies to normal rework; if there is no such block, do not ask further.
- Work, rules, or decisions that affect this causal chain must state their current status with evidence; writing only the name is “not deep enough.” Things mentioned only in passing do not need to be written.

Give “I don’t know” to the judge model to assess under standard 7; do not match the three characters in code. Those characters can appear in an ordinary sentence such as “the tool does not know how much quota is left” without meaning the issue is unresolved. The program only blocks empty questions.

For WHY.md judgments, use the Traditional Chinese verdicts `過` (pass) and `不夠深` (not deep enough); an English environment may use `PASS` / `NOT_DEEP_ENOUGH`. Configure the hook to accept the selected set of verdict labels and heading language. General reviews use the fixed labels specified at assignment. A model error, timeout, or inability to read necessary evidence is an incomplete judgment, not “pass.”

## Judgment response format

Every review response follows the six-line format owned by the skill.

The meaning and wording of the six lines are the current definition in [the response-format section of SKILL.md](SKILL.md#when-asking-another-ai-to-review-or-judge-always-require-this-response-format). This document lists the same format so that there is no second set of judgment rules:

```text
Verdict: For a WHY.md judgment, use `過` (pass) or `不夠深` (not deep enough); an English environment may use `PASS` or `NOT_DEEP_ENOUGH`, with the hook configured to accept the selected set; for a general review, use the fixed label specified at assignment
Real problem or surface result: Quote the sentence treated as the cause and decide whether it is a cause or a result
Purpose and approach: State the purpose, the approach, and how they relate
Will it work: State yes / no / can’t tell, and why
Not relevant: Unrelated content to remove; if there is none, say none
Reasoning: Explain the verdict in plain language
```

This format evaluates the thinking. A verdict without reasons leaves the recipient guessing; asking only “what is missing?” makes the content grow, including unrelated parts, and move farther from the real problem.

## How to avoid false blocks

Inspect the action that will actually run, rather than matching words anywhere in the request.

Judge only a command or write action that will actually execute, confirming its target, working directory, parameters, and actor. None of the following is rework:

- Mentioning a rework command in a document, comment, message, or record.
- Viewing `--help` or reading a command’s documentation.
- Defining a function without calling the rework action inside it.
- Querying, searching, or reading WHY.md, receipts, or judgment records.
- A different project happens to have a command with the same name, while the actual target is outside the protected scope.

Read-only queries are never blocked. When the cause cannot be found, queries must still work. A block message should say which condition is missing, where to write the ten questions, and that “I don’t know” can be measured first and the measurement command will not be blocked, so investigation can continue.

## Threat scope

This design deters habitual skipping; it is not a security boundary against deliberate forgery by the same user.

This design prevents habitual skipping, but it does not promise to stop deliberate forgery by the same user. If the agent and hook use the same account on the same computer, and the agent can edit receipts, judgment records, or hook settings, local files alone cannot create protection that the agent cannot cross.

The collaboration flow can reject hand-written receipts and check the real judgment record, reducing accidental skips. It cannot detect every deliberate forgery. Preventing deliberate forgery requires an external trust source the user cannot modify, protected signatures, and separation of permissions; that is outside this design’s purpose. Do not keep adding detection rules for an unpromised protection.

## How to verify it

Replay real action histories and test whether the reviewer separates shallow answers from evidenced root causes.

When verifying each project’s implementation, start with a clear time range from lawful, readable historical records. Mark each record “should block” or “should not block,” resolve disputed items, and freeze this comparison set. Do not select only examples that are easy to pass.

- **Action replay**: Without a valid receipt, every action that should block is blocked, and false blocks for other actions are 0. Compare the result of each same action, not only the total count.
- **Boundary replay**: Cover normal closeout, general checks, read-only queries, text mentions, `--help`, uncalled functions, same-named commands with different targets, and other actors. All should be allowed.
- **Receipt checks**: The same valid WHY and a genuine passing receipt should be allowed; a receipt 9 minutes 59 seconds old should be valid, and one 10 minutes 01 second old should be invalid. Changing one character, missing the receipt, a hash mismatch, an unverifiable judgment record, or a test receipt should block that rework.
- **Judgment checks**: For the same event, prepare one surface answer and one answer that includes evidence and traces to the process or design. Both must use the six-line format; the surface answer is judged “not deep enough” and must identify the surface result treated as the cause, the relation between purpose and approach, whether the approach can solve the problem, and content to remove; an answer that reaches the root and meets all standards is judged “pass.” Also test missing evidence, patching only one location for the same cause, no exact verification value, and loosening the standard after the fact; each should be judged “not deep enough.”
- **Complete flow**: Block first, write the ten questions, have another model judge them, produce a matching receipt, and then release that rework. Confirm that read-only queries and other people’s work can continue throughout.

Keep the data scope, implementation version, per-record results, and judgment records. These are verification methods; they do not mean this repo has implemented or verified any hook.

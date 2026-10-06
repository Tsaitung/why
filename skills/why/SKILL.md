---
name: why
description: >-
  Evidence-backed rework review. Use when a fix keeps failing, when reviewed work must be redone,
  or before trying another way past a check. Blocked: answer three questions (purpose, what the check
  protects, what counts as progress). Rework: write a ten-question WHY.md and get an independent model's verdict.
  重做前的原因審查：修正一直失敗、審查過的工作要重做、或想繞過檢查之前用。被擋住先答三題；真的要重做才寫十題 WHY.md，交另一個模型判斷。
---

# why: Think through why before doing the work again

繁體中文版：[SKILL.zh-TW.md](SKILL.zh-TW.md)

## Quick view (follow these five lines first)

1. What is the **purpose** of this work? When it is done, who in production gets **one more usable thing or one fewer error**, or **which launch blocker is removed**? If you cannot say, investigate; if you confirm there is none, do not do it.
2. Why was the thing blocking me, or the thing I want to change, designed this way in the first place, and what is it protecting? Does that protected thing already exist now?
3. Is the “cause” I wrote an **answer or a result**? If it only says “where it broke,” keep asking why.
4. When you reach the process or design layer: **Was the design checked? Was the required planning done? Is that step really necessary?**
5. Is this the same cause as the previous times? If so, change the source and fix it once instead of patching only one place.

## When to use it and how deep to go

The thinking method can be used by agents and human teams; the collaboration hook blocks only the rework actions, work, and actors explicitly configured by the project.

| Situation | What to do |
|---|---|
| Rework: refreeze a specification, send work back for repair, continue an assignment, re-register it, rerun only what failed, or change a frozen specification | Do all four steps, write WHY.md, and submit it for a verdict; only the configured scope with a collaboration hook installed is blocked |
| Blocked, about to try another approach; about to read gate code to find “how to get through”; or the work only updates system records such as a label, status, or closure | Do the first step and write three lines in the report or work log: purpose / what is protected / what counts as progress. Do not write WHY.md or stop other work. If it is progress, do it; if you cannot answer, investigate; if investigation confirms it is not progress, do not do it and add it to the project’s improvement list |
| One-off small change, normal closeout, general check, pure query or read-only diagnosis, or producing the verdict receipt itself | The four steps and ten questions are not required; read-only queries are never blocked |
| Incident response that needs immediate containment | Follow the existing incident process first; review with this method after things are stable so the ten questions do not delay containment |

## Step 1: Three things (do not ask why yet)

1. **Purpose**: What result must this work produce, who will use it, and what will they use it for? Quote the original wording from the requester or the current specification.
2. **Design intent**: Why was the thing blocking me, or the thing I want to change, designed this way, and what is it protecting? Read the specification or the decision record from that time; do not guess. Then ask: **Does the thing it protects already exist? Where is the evidence?** If it already exists, check the existing evidence instead of repeating the same work just to pass a check.
3. **What counts as progress**: After it is done, will the user see or receive something different? Or will it remove a launch blocker or complete verification required for launch? If neither, it is only procedure. Ask the reverse question: “If this is never done, who will receive something wrong, receive less, or remain blocked?” If you have checked and confirmed no one will, do not do it; not knowing does not mean there is no impact. Continue to follow existing requirements for safety, permissions, data protection, and necessary human approvals.

If you cannot answer any one of the three, the next step is to investigate, not to fix.

## Step 2: Ask why layer by layer, and at every layer ask “Is this an answer or a result?”

| Wording you see | Usually is | General example |
|---|---|---|
| “Was not sent / was not listed / was written incorrectly / is missing / is broken” | Result | “One entry point did not pass in the original scope” |
| “I will try every rule / switch to another approach” | A workaround, not a cause | “Read the gate code and try each rule one by one” |
| “The process did not require doing this first, so the same omission was not found” | Could be the root cause; still needs evidence | “Before writing the specification, no one confirmed which entry points this value passes through or where it is used” |

For every layer you write, run four checks:

1. Are you describing “where it is broken and what happened,” or a result? If it is a result, ask why again.
2. If you change it, could the same kind of problem appear somewhere else? If yes, it is not the root cause yet.
3. Is it a decision, a missing rule, or a missing process step? Stop only when evidence proves it. Who can change it, and whether outside handling is needed, belongs in question 8.
4. Is it being used to block, cover up, or route around something else? For example, “want to block the old styling,” add a blanket reset, add an exception to evade a check, or loosen a check. If yes, that block is still a symptom: ask “Why is the blocked thing still there? Is there something already decided but not finished?” The same applies to normal rework. If no, do not keep asking.

For work, rules, or decisions that affect this causal chain, write their **current status** with evidence: is it done, and where is it blocked? Naming it without its status does not show whether it is actually the point of blockage. Things mentioned only in passing do not need to be written.

First distinguish this case: a design was sent for review, the review found an error, it was sent back for repair, and the error has not been used in production. That is **normal rework**, and it means the review worked. During normal rework, questions 2–4 stop once they reach “what was wrong in the design and why was it missed”; the source fix can go on the improvement list. Only when the error has already been used, or something that should have been blocked was not blocked, do you continue into the process and change the source.

When you reach the process or design layer, check these three things again:

- **Was the design checked?** Who checked it, against what standard, and did the scope cover where this error occurred? If not, why not?
- **Was the required planning done?** Was the data trail followed first, did the requester confirm the sample, were acceptance conditions set in advance, and was it checked with valid data that reflects real conditions? Identify the missing step and attach evidence; do not turn all of these examples into new procedures.
- **Is that step really necessary?** Add only the step that can stop this class of problem and move delivery forward. Do not lengthen the checklist for completeness. Consider removing things that are not needed.

Attach evidence to every layer, such as a file, line, or run. If the file is being changed, state the commit or content hash you inspected. Mark a cause as a hypothesis when it has no evidence, and measure first.

## Step 3: Write WHY.md (only for rework; copy the ten headings exactly)

Location: project-defined, for example `.why/WHY.md`.

```markdown
## 0. The issue
## 1. What we observed
## 2. Why did it happen
## 3. And why was that
## 4. Why again (what in the process or design let it happen)
## 5. Why does it exist, and is it really needed
## 6. What happens if we don't do it
## 7. Compared with the last 5 times
## 8. The approach
## 9. How we will prove it worked
```

- Question 0: State in one sentence what this event and problem are.
- Question 1: Write the measured observation and attach evidence.
- Questions 2–4: Write the causal layers from Step 2, with each layer connected to the previous one and backed by evidence; question 4 must pass all four checks.
- Question 5: Add Step 1’s purpose and design intent, including “what happens if we delete the whole thing.”
- Question 6: Make Step 1’s progress decision and name the specific feature, user, or launch stage that is blocked; if the answer is “no impact,” do not do it.
- Question 7: Compare the cause with the last 5 times. If the cause is the same, write the shared source that must change; if there are fewer than 5 times, list the records that actually exist and say clearly when there are no records.
- Question 8: State what will change, why it removes the cause in question 4, and whether the same class will happen again; also state who must handle it or what approval is needed.
- Question 9: State the layer to measure, the valid data to use, and the exact expected value. Measure that the cause in question 4 no longer exists; do not look only for the symptom to disappear. If you change the verification standard, attach evidence for why the old standard was wrong and the new one is right; do not merely loosen it until it equals the failure you measured.

If any question is answered “I don’t know,” perform a read-only measurement first; do not start rework that depends on that answer.

The Chinese headings (see [SKILL.zh-TW.md](SKILL.zh-TW.md)) may be used as well, but each WHY.md uses one language only. The hook decides which heading set it matches from its own configuration.

## Step 4: Have another AI judge it

Use “your judge command” (see hook-design.md). The command is configured by your project; this skill does not include a judge tool or hook implementation. The collaboration method is described in the [design document](hook-design.md) in this folder.

If the installed folder contains `local.md`, read it first: it contains settings for your machine or team, such as the judge command, where WHY.md lives, and which people and actions the hook blocks. `local.md` does not belong in this repo; install the other files in this skill exactly as the repo provides them, and do not edit the local copy. If you need to change it, change the repo and sync it back to the machine.

- The user chooses the concrete judge model and dispatch account in their own environment, for example by separating settings with `CODEX_HOME`; the judge model must still come from a different family. Do not hard-code a model name, account, or internal path in the skill. Use read-only access and the 9 standards in the design document.
- Verdict `過` (pass): after the receipt meets the release conditions in the design document, the work can start within 10 minutes; changing even one character in WHY.md requires a new verdict. This only means the cause and approach passed review; it does not replace existing human approval.
- Verdict `不夠深` (not deep enough): use the reasons in the response below to fix the causal chain and approach, replace the surface result with the real cause, and remove unrelated content; do not add more items. If it is judged not deep enough twice in a row, find new evidence instead of only changing wording.
- An incomplete verdict or a receipt that cannot be checked is not a pass; stop only that rework and continue queries and other work.
- When reporting, use one plain-language sentence to state the root cause from question 4 and the progress from question 6; do not paste all ten questions.

In an English environment, the verdict may be written as `PASS` / `NOT_DEEP_ENOUGH` instead. Configure the hook to accept the selected combination of verdict labels and heading language.

## When asking another AI to review or judge, always require this response format

Reviews, QA, and WHY.md judgments must use the six lines below. They evaluate whether the reasoning found the real problem and whether the approach can achieve the purpose, not whether the checklist is missing items.

1. **Verdict**: For a WHY.md judgment, write only `過` (pass) or `不夠深` (not deep enough); in an English environment, `PASS` or `NOT_DEEP_ENOUGH` is also allowed. For a general review, use the fixed label specified when the work was assigned, such as `PASS`, `CHANGES_REQUIRED`, or `REPLAN_REQUIRED`.
2. **Real problem or surface result**: Quote the sentence treated as the cause and decide whether it is a cause or a result.
3. **Purpose and approach**: State the purpose and approach in one sentence each, then explain how they relate.
4. **Will it work**: Write “yes,” “no,” or “can’t tell,” with the reason.
5. **Not relevant**: List content unrelated to the real problem and to be removed; if there is none, say “none” instead of forcing a list.
6. **Reasoning**: Explain the overall verdict in plain language.

A verdict without reasons leaves the recipient guessing. Asking only “what is missing?” makes people keep adding unrelated material while moving farther from the real problem.

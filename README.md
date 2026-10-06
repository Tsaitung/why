# WHY

**Ask why before doing the work again.**
Evidence-backed rework review for Claude Code and Codex.

WHY helps coding agents stop repeating fixes that don't address the cause. Before rework, it examines user impact, design intent, and evidence, then produces a ten-question WHY.md for independent review. Blocked tasks start with a lighter three-question check.

[中文說明在下方](#why--中文)

## When to use

- A fix keeps failing, or the same kind of problem keeps coming back.
- Work that was reviewed or sent back has to be redone.
- You are about to try another way past a check, gate, or failing test.

Not for one-off small edits, pure queries, or urgent incident containment. Contain the incident first; review it afterward.

## Two levels — most of the time you only need the first

| Situation | What you do | Output |
|---|---|---|
| Blocked, or tempted to route around a check | Answer three questions: **purpose** (who gets what), **what the check protects**, **what counts as progress** | Three lines in your notes. No report. |
| Real rework (refreeze, resend, rerun what failed, redo reviewed work) | Answer ten questions in WHY.md, then have **another model** judge it | WHY.md plus a verdict: `過` (pass) or `不夠深` (not deep enough) |

## What you get

- A cause analysis where every "why" has evidence (file, line, run), not a guess.
- A change that removes the cause, not just the visible symptom — and a list of what to drop.
- A verification standard you can measure: which layer, which data, which exact expected value.
- A six-line verdict from an independent reviewer: is this the real cause or a surface result, do purpose and method connect, will it work, what is irrelevant, and why.

## What's included — and what you set up yourself

| | Status |
|---|---|
| The method (`skills/why/SKILL.md`) | Included |
| Hook design and judge standards (`skills/why/hook-design.md`) | Included, as documentation |
| Cross-model reviewer | **You configure it** — any read-only model from a different family than the one doing the work |
| Automatic blocking of rework commands | **Not included** — implement a hook from the design if you want it |
| Language | The skill text and WHY.md headings are currently Traditional Chinese; an English edition is planned |

Machine- or team-specific settings (your judge command, where WHY.md lives, who the hook applies to) go in a `local.md` next to the skill. It is not part of this repo.

## Quickstart

1. Copy the whole `skills/why` folder to `~/.claude/skills/why/` (Claude Code) or `~/.agents/skills/why/` (Codex).
2. When something is blocked, ask the agent to "use the why skill": it answers the three questions before trying anything else.
3. Before rework, the agent writes WHY.md in a place you choose (for example `.why/WHY.md`) using the ten headings in the skill.
4. Give WHY.md to a different model, read-only, with the judge standards and six-line reply format from `hook-design.md`.
5. `過` → do the rework. `不夠深` → fix the reasoning the verdict points at (replace the surface cause, drop unrelated work), not by adding more items.

## Problems it helps avoid

- **Fixing symptoms:** patching the visible failure without checking why it happened.
- **Repeated rework from one cause:** leaving the shared cause in place and retrying in many small pieces.
- **Working to pass a check or close a task:** a green status without a better result for users.
- **Treating a workaround as the cause:** "we added an override to block X" without asking why X is still there.
- **Setting scope without measurement:** choosing one patch before checking where the data flows.
- **Relaxing verification after failure:** moving the standard to match the failure you just saw.
- **Overengineering:** adding tests, rules, and paperwork that don't remove a delivery obstacle.
- **Verdicts without reasons, or "what's missing" reviews:** people guess, or keep adding unrelated material.

## License

Copyright (c) 2026 Tsaitung. **MIT + Commons Clause License Condition v1.0** — not plain MIT.

You may use, modify, and share it while keeping the full license notice, but **you may not Sell it**: you may not provide WHY itself, or a product or service whose value derives entirely or substantially from WHY's functionality, to third parties for a fee or other consideration (including hosting, consulting, or support that meets that definition). This follows the Commons Clause definition of Sell; it is not a ban on every commercial use. See [LICENSE](LICENSE) for the full terms, and the original [MIT](https://opensource.org/license/mit) and [Commons Clause](https://commonsclause.com/) texts.

---

# WHY — 中文

**動手重做前，先想清楚為什麼。**
給 Claude Code 與 Codex 用的「重做前原因審查」，每個原因都要有證據。

WHY 讓寫程式的 AI 不再一直重複沒打中原因的修正。重做之前，它先查清楚對使用者的影響、當初的設計用意與證據，寫成十題的 WHY.md，交給另一個模型獨立判斷。只是被擋住時，先用比較輕的三個問題。

## 什麼時候用

- 同一個修正一直失敗，或同一類問題一再發生。
- 已經審查過、被退回的工作要重做。
- 想換一種方法繞過檢查、閘門或失敗的測試之前。

一次性小改、純查詢、需要馬上止血的事故不適用。事故先處理，穩定後再檢討。

## 兩種深度——大多數時候只需要第一種

| 情況 | 做什麼 | 產出 |
|---|---|---|
| 被擋住，或想繞過某個檢查 | 先答三題：**目的**（誰拿到什麼）、**這個檢查在保護什麼**、**什麼叫進度** | 工作紀錄裡三行，不用寫報告 |
| 真的要重做（重凍規格、重派、只重跑失敗的、重做審查過的工作） | WHY.md 寫十題，交給**另一個模型**判斷 | WHY.md 加一個判定：「過」或「不夠深」 |

## 你會得到什麼

- 每一層「為什麼」都附證據（哪個檔、哪一行、哪次執行），不是用猜的。
- 拿掉原因、不只讓症狀消失的做法，以及哪些不相關的事該刪掉。
- 量得到的驗收標準：在哪一層量、用哪份資料、預期的確定值。
- 獨立審查回六行：是真的原因還是表面結果、目的和做法連不連得起來、會不會解決、哪些不相關、判斷理由。

## 有附的、要自己設定的

| | 狀態 |
|---|---|
| 方法（`skills/why/SKILL.md`） | 有附 |
| hook 設計與判斷標準（`skills/why/hook-design.md`） | 有附，是說明文件 |
| 跨模型審查 | **要自己設定**：用跟做事的模型不同家族的模型，只讀 |
| 自動擋住重做指令 | **沒有附**：需要的話照設計說明自己做 hook |
| 語言 | 技能內容與 WHY.md 標題目前是繁體中文，英文版之後補 |

每台機器或團隊自己的設定（判斷指令、WHY.md 放哪、hook 擋誰）放在技能旁邊的 `local.md`，不放進本 repo。

## 快速開始

1. 把整個 `skills/why` 資料夾複製到 `~/.claude/skills/why/`（Claude Code）或 `~/.agents/skills/why/`（Codex）。
2. 被擋住時，請 AI「用 why 技能」：它會先答三題，再決定要不要做。
3. 要重做前，AI 照技能裡的十題標題，在你指定的位置寫 WHY.md（例如 `.why/WHY.md`）。
4. 把 WHY.md 交給另一個模型，只讀，附上 `hook-design.md` 的判斷標準與六行回覆格式。
5. 判「過」就重做；判「不夠深」就照理由修正因果（把表面結果換成真正原因、刪掉不相關的），不是一直往上加東西。

## 可以避免哪些開發問題

- **只修症狀**：把眼前壞掉的地方補好，卻沒有查清為什麼會壞。
- **同一原因一修再修**：共同源頭沒改，拆成很多小重做。
- **為了過檢查或關單而做事**：狀態變綠，使用者卻沒拿到更好的結果。
- **把繞過的手段當成原因**：「加了一條規則擋掉 X」，卻沒問 X 為什麼還在。
- **沒量就定範圍**：沒查資料怎麼走，就決定只修一個地方。
- **驗證標準事後放寬**：看到失敗結果，才把標準改到剛好能過。
- **過度工程**：一直加測試、規則和手續，卻沒有移除交付的阻礙。
- **只有判定，或只問缺什麼**：沒有理由只能猜；一直加清單反而離真正問題越來越遠。

## 授權

Copyright (c) 2026 Tsaitung。採用 **MIT＋Commons Clause License Condition v1.0**，不是單純的 MIT。

可自由使用、修改、分享，但必須保留完整授權聲明，且**不可販售**：不得把 WHY 本身，或價值全部或主要來自 WHY 功能的產品或服務，提供給第三方換取費用或其他對價（包括符合該定義的代管、顧問或支援服務）。「不可販售」依 Commons Clause 的 Sell 定義，不是禁止所有商業情境。完整條件以 [LICENSE](LICENSE) 為準，原文：[MIT](https://opensource.org/license/mit)、[Commons Clause](https://commonsclause.com/)。

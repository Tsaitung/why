# why

動手重做前，先想清楚為什麼。

## 這是什麼

why 是一個給 AI 寫程式代理與人類團隊使用的技能。重做前先查目的、設計與根本原因，把十題寫進 WHY.md，再交給另一個模型判斷，才決定是否值得繼續。

這個 repo 提供[技能](skills/why/SKILL.md)與[協作 hook 設計](skills/why/hook-design.md)。hook 是執行指定重做動作前的自動檢查；本 repo 提供設計說明，沒有附實作或自動安裝。

## 可以避免哪些開發問題

why 要幫助團隊避免：

- **只修症狀**：把眼前壞掉的地方補好，卻沒有查清為什麼會壞。
- **同一原因一修再修**：共同源頭沒改，拆成大量小重工，每次只補一處。
- **為了過檢查或關單而做事**：只追求狀態變成通過，沒有讓使用者拿到更好的結果。
- **沒量就定範圍**：沒查資料怎麼走、哪些入口受影響，就決定只修其中一個地方。
- **驗證標準事後放寬**：看到失敗結果，才把原本的標準改到剛好能過。
- **過度工程**：不停增加測試、規則或手續，卻沒有直接移除交付的阻礙。
- **只有判定，或只問缺什麼**：沒有理由就只能猜；一直加清單會放進無關內容，反而離真正問題越來越遠。

它讓原因、做法與驗證方式先被看清楚；是否有效仍要用實際結果證明。

## 適用範圍

- Claude Code、Codex 等 AI 寫程式代理要重做、被擋下，或想換一種方法通過檢查時。
- 人類團隊做事後檢討，尤其同一類問題反覆發生時。

不適用於一次性小改、純查詢，以及需要即時處理的事故止血。先處理事故，穩定後再檢討。被擋下時先答目的、設計與進度三件事；只有重做才要求完整十題。

## 怎麼裝、怎麼用

把本 repo 的 `skills/why` 整個資料夾放到以下其中一處：

- `~/.claude/skills/why/`
- `~/.agents/skills/why/`

整個資料夾包含 `SKILL.md` 與 `hook-design.md`，複製後就有 hook 設計說明。這只安裝技能；如需自動擋住重做，另依資料夾內的 [hook 設計說明](skills/why/hook-design.md)設定你的判斷指令與接入方式。

使用時照技能的四步走。WHY.md 位置由專案自訂，例如 `.why/WHY.md`。重做的判斷要交給另一個不同家族的模型，採唯讀方式；判「過」的收據必須對得上目前內容，10 分鐘內才有效。改一個字就要重判。

hook 只針對設定範圍內的動作與執行者，不擋只讀查詢或其他人的工作；它防習慣性跳過，沒有承諾防蓄意造假。

## 授權

著作權：Copyright (c) 2026 Tsaitung。

採用 **MIT＋Commons Clause License Condition v1.0**。可自由使用、修改、分享，但必須保留完整授權聲明，且**不可販售**：不得把 why 本身，或價值全部或主要來自 why 功能的產品或服務，提供給第三方換取費用或其他對價。這也包括符合該定義的代管、顧問或支援服務。

「不可販售」依 Commons Clause 的 Sell 定義，不是禁止所有商業情境。這是公開可讀的內容，並非只有 MIT 的授權；完整條件以 [LICENSE](LICENSE) 為準。原文來源：[MIT](https://opensource.org/license/mit)、[Commons Clause](https://commonsclause.com/)。

---

# why — English

Ask why before doing the work again.

## What this is

why is a skill for AI coding agents and human teams. Before rework, examine the purpose, design, and root cause, answer ten questions in WHY.md, and ask another model to judge whether the proposed work is justified.

This repository includes the [skill](skills/why/SKILL.md) and a [collaborative hook design](skills/why/hook-design.md). A hook checks conditions before a configured rework action runs. The design is documentation; no hook implementation or automatic installer is included.

## Development problems it helps avoid

- **Fixing symptoms:** patching the visible failure without checking why it happened.
- **Repeated rework from one cause:** leaving the shared cause in place and splitting the work into many small retries.
- **Working just to pass a check or close a task:** getting a passing status without improving the result for users.
- **Setting scope without measurement:** choosing one patch before checking the data flow and affected entry points.
- **Relaxing verification after failure:** changing the standard to match the failed result already observed.
- **Overengineering:** adding tests, rules, and paperwork without removing an actual delivery obstacle.
- **Giving only a verdict, or only asking what is missing:** without reasons, people must guess; adding unrelated material can move the work farther from the real problem.

The skill makes the cause, proposed change, and verification method explicit. Its effect still needs evidence from real outcomes.

## Scope

- AI coding agents such as Claude Code and Codex when redoing work, being blocked, or considering another way past a check.
- Human team retrospectives, especially when the same class of problem keeps returning.

It is not intended for one-off small edits, pure queries, or urgent incident containment. Contain the incident first and review it afterward. When blocked, start with purpose, design, and progress; only rework requires the full ten questions.

## Install and use

Copy the entire `skills/why` folder into either:

- `~/.claude/skills/why/`
- `~/.agents/skills/why/`

The entire folder includes `SKILL.md` and `hook-design.md`, so the hook design document is copied with the skill. This installs the skill only. To gate rework automatically, configure your review command and hook using the [design document included in the folder](skills/why/hook-design.md).

Follow the skill's four steps. Choose a WHY.md location for each project, such as `.why/WHY.md`. Use another model from a different family with read-only access. A passing receipt must match the current content and be no more than ten minutes old. Changing even one character requires a new review.

The hook covers only configured actions and actors. Read-only queries and other people's work remain available. It deters habitual skipping, not deliberate forgery.

## License

Copyright (c) 2026 Tsaitung.

**MIT plus Commons Clause License Condition v1.0.** You may freely use, modify, and share the material while retaining the complete license notices, but **you may not Sell it**: you may not provide why itself, or a product or service whose value derives entirely or substantially from why's functionality, to third parties for a fee or other consideration. This includes hosting, consulting, or support services that meet that definition.

The restriction follows the Commons Clause definition of Sell; it is not a ban on every commercial use. This material is publicly available under the combined terms. See [LICENSE](LICENSE) for the full conditions and the original [MIT](https://opensource.org/license/mit) and [Commons Clause](https://commonsclause.com/) texts.

---
name: team
description: devteam 角色的分工與共用規則。在有 `docs/agents/dev-workflow.md` 的 repo 實作、修 bug、審查、triage 或收票前載入；devteam 的每個角色開工時也先讀它。
---

# devteam

## 分工

開發與審查交給 devteam 的角色：`devteam:rd`、`devteam:pm`、`devteam:qa`、`devteam:sa`、`devteam:uiux`，何時用哪一個寫在各自的 description。主 session 不 inline 從頭做到尾。合併前的關卡與開 PR、合併的分工寫在 `${CLAUDE_PLUGIN_ROOT}/agents/rd.md`；主 session 已經自己做完實作的那一次不必重開代理重做，剩下的關卡照它第三節走完，合併來自票的 PR 之後、回報完成之前走 Definition of Done。

只有 `rd` 改 repo 檔案與 commit；其他角色不改，也不用 Bash 繞過，寫 tracker、臨時注入缺陷與實機驗收的例外寫在各自的定義裡。

## 專案契約

角色只寫通用的做法，專案的事實由 repo 裡這幾個檔提供，角色以「檔名〈節名〉」引用。缺檔或缺節時回報缺哪一個、建議使用者跑 `/devteam:setup-devteam` 補上，不臆測。每一節要寫什麼在 `${CLAUDE_PLUGIN_ROOT}/skills/setup-devteam/contract.md`。

| 檔案 | 節 |
| --- | --- |
| `AGENTS.md` | 產品的使用者是誰；角色另外要停下來問的事 |
| `CONTEXT.md` | 詞彙（選用） |
| `docs/agents/issue-tracker.md` | 讀寫票的指令、〈什麼該變成票〉、〈Definition of Done〉 |
| `docs/agents/triage-labels.md` | 類別、狀態與手動維護的標籤字串 |
| `docs/agents/dev-workflow.md` | 〈指令〉、〈分支、commit 與合併〉、〈畫面〉（選用）、〈實機驗收〉（選用；有這一節就是交付前必做） |
| `docs/agents/testing.md` | 〈六條門檻〉、〈第 5 條：反向驗證怎麼做〉與它的〈這個 repo 的指令〉、〈測試分層〉、〈docstring 寫什麼〉；寫測試或審測試前先讀 |

## 每個角色都照做

- 詞彙用 `CONTEXT.md` 定義的詞，不漂到它 `_Avoid_` 的同義詞。要用的概念不在詞彙表裡，先想是不是在發明這個專案不用的語言；真的有缺口就交給 `domain-modeling`。
- 「使用者」指 `AGENTS.md` 說的產品使用者，不含維護者。講使用者體驗的描述用使用者看得懂的白話，不用工程比喻。
- 找不到問題就說沒有，不湊清單。
- 縮票上的範圍、做不可逆的改動，只有使用者能決定：停下來問他，不交給代理、不自己定。
- 回覆、文件、票與 PR 先寫結果，每項事實只寫一次。建立或更新 PR 前讀 `.github/pull_request_template.md`，按最終改動重寫標題與正文，刪掉可選空段落與提示註解。

## 取用 skill

frontmatter 有 `disable-model-invocation: true` 的 skill，代理用 Skill 工具叫不動。要照它做時：

1. 找 SKILL.md：在 `~/.claude/plugins/cache` 與 Claude 桌面版的資料目錄（macOS 是 `~/Library/Application Support/Claude`，Windows 是 `%APPDATA%\Claude`）底下找 `*/skills/*/<skill>/SKILL.md`。出來多份時取所屬 plugin 的 `.claude-plugin/plugin.json` 版本最高的。找不到就回報沒裝，不猜路徑。
2. 連同它引用的參考文件讀完照做。寫 ADR、查或寫 out-of-scope 知識庫的步驟，repo 沒有那個目錄（`docs/adr/`、`.out-of-scope/`）就略過。
3. 回報標明「照 `<skill>` 的 SKILL.md 展開，不是跑 `/<skill>`」。

要跟使用者一輪輪對話才能定案的（grilling 類、`to-questionnaire`、`wait-what`、`teach`、triage 裡的 grilling 段落）子代理不代做：停下來把問題列給使用者，請他在主 session 進行。`to-spec`、`to-tickets` 也留在主 session。代理之間交接時，自己寫一份 markdown 到 scratch 目錄，段落照 `handoff` 的 SKILL.md。

## 推薦選項的判準

grilling、triage 或代定時要推薦一個選項：選讓每一條影響正確性的規則只有一個權威來源、同時引入最少新概念的最小設計。「最小」指要理解的概念、介面、接縫與例外最少，不是行數最少。依序問：

1. 哪些規則影響正確性？各自的權威該在哪個既有模組？
2. 能否加深既有模組，讓 caller 只拿到完成責任所需的最窄介面？
3. 能否沿用既有函式、錯誤處理與接縫？出現真實的第二種變體才加 adapter 或通用抽象。
4. 能否在資料第一次進入可信範圍時拒絕不合法狀態，又不擴張到題目以外的 hardening？
5. 測試能否穿過正式介面驗證行為，而不讀私有實作？

使用者要求代為選擇時照這個順序選定並說明主要取捨，否則每題給推薦答案讓他決定。推薦不是擴張範圍或執行外部動作的授權。

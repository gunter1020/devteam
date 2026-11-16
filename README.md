# devteam

開發團隊的五個角色與 autopilot 工作流，做成 Claude Code plugin。角色只寫通用的做法，各專案的指令、測試分層、標籤與畫面規則放在該 repo 的 `docs/agents/`。

| 元件 | 用途 |
| --- | --- |
| `devteam:rd` | 接票實作、從壞掉的現象修 bug、過合併前的關卡、開 PR、合併後收票；唯一會寫檔與 commit 的角色 |
| `devteam:pm` | 驗收一次開發、triage 還沒定案的票 |
| `devteam:qa` | 審測試證據；有定義實機驗收的 repo 也做實機驗收 |
| `devteam:sa` | 架構審查、巡檢值得加深的模組 |
| `devteam:uiux` | 量畫面對齊與跑版、出設計稿 |
| `/devteam:autopilot` | 把一件事整串交給團隊，只在停點問使用者 |
| `devteam:team` | 分工、專案契約、取用 skill 與推薦選項的判準；角色開工時先讀 |
| `/devteam:setup-team` | 引導你替自己的 repo 設計專案契約：推得出的從 repo 查，要決定的才問你，寫完逐項驗證 |

## 安裝

```bash
claude plugin marketplace add gunter1020/devteam
claude plugin install devteam@gunter
```

角色會用到 [mattpocock/skills](https://github.com/mattpocock/skills)（`tdd`、`diagnosing-bugs`、`triage`、`codebase-design` 等），要另外安裝。

## 接一個新 repo

在那個 repo 跑 `/devteam:setup-team`。契約的清單在 [`skills/team/SKILL.md`](skills/team/SKILL.md)〈專案契約〉，每一節要寫什麼在 [`skills/setup-team/contract.md`](skills/setup-team/contract.md)。

## 改這個 plugin

改完跑 `claude plugin validate .` 再 push。`plugin.json` 不設 `version`，安裝的版本跟著 commit 走。

本機取得新版：`claude plugin marketplace update gunter`，再 `claude plugin update devteam@gunter`（前者只更新 marketplace 的副本，安裝的版本要後者才會換），開著的 session 再 `/reload-plugins`。

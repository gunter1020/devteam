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

## 安裝

```bash
claude plugin marketplace add gunter1020/devteam
claude plugin install devteam@gunter
```

角色會用到 [mattpocock/skills](https://github.com/mattpocock/skills)（`tdd`、`diagnosing-bugs`、`triage`、`codebase-design` 等），要另外安裝。

## 專案要提供的檔

清單與每個檔要有的節寫在 [`skills/team/SKILL.md`](skills/team/SKILL.md)〈專案契約〉。缺檔或缺節時角色會回報缺哪一個。

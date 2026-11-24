# devteam

在 Claude Code 或 Codex 裡把一張票交給開發團隊，或指定 RD、PM、QA、SA、UI/UX 處理單一任務。[角色說明](agents/)

## 安裝

**ChatGPT 工作區**　在 **Admin → Plugins → Add → Import marketplace** 匯入 `gunter1020/devteam`，選擇含 Codex marketplace 的來源並啟用 devteam。選分支時，新匯入的 marketplace 每日檢查更新，也可用 **Sync now** 提前同步。固定 commit 則停在該版本。[GitHub 匯入與同步](https://learn.chatgpt.com/docs/enterprise/plugin-management)

**Codex CLI**　在本儲存庫執行：

```sh
codex plugin marketplace add .
codex plugin add devteam@gunter-codex
```

**Claude Code**：

```sh
claude plugin marketplace add gunter1020/devteam
claude plugin install devteam@gunter-plugins
```

依任務另裝 [mattpocock/skills](https://github.com/mattpocock/skills) 的開發與審查技能，以及畫面量測用的 [agent-browser](https://github.com/vercel-labs/agent-browser)。使用 agent-browser CLI 的執行環境也要有該指令。

## 在專案使用

- **接新 repo**　在目標 repo 呼叫 devteam 的 `setup-team`，建立專案契約。各專案維護自己的指令、標籤與例外；採用預設的章節從 `setup-team` 的 `defaults/` 複製後改寫。[契約內容](skills/setup-team/contract.md)
- **指定角色**　例如「請 devteam 的 QA 審查這張 PR 的測試證據」。Claude Code 也可直接指定 `devteam:qa`。Codex 透過 `team` 技能把角色工作交給目前工作階段的子代理。
- **交辦整張票**　明確選擇 devteam 的 `autopilot` 並指定票號與授權範圍。在 Claude Code 使用 `/devteam:autopilot`。[Autopilot 流程](skills/autopilot/SKILL.md)

## 維護

角色定義在 [agents/](agents/)，三個技能在 [skills/](skills/)。Claude Code 自動載入 `agents/`；Codex 安裝時複製整個外掛根目錄，`team` 技能從那裡讀角色檔。

提交前執行 `claude plugin validate .`。`.claude-plugin/plugin.json` 不設 `version`，Claude Code 安裝的版本跟著 commit 走。`.codex-plugin/plugin.json` 必須設 `version`（ChatGPT 工作區同步缺它會失敗），而且改了 `skills/`、`agents/` 或 Codex 設定就要升版——Codex 只在版本變了才重新下載，CI 在 PR 上檢查。從本機路徑加入的 Codex 外掛，更新 checkout 後重跑 `codex plugin add devteam@gunter-codex`。

Claude Code 使用者可用 `claude plugin marketplace update gunter-plugins` 更新目錄，再以 `claude plugin update devteam@gunter-plugins` 更新套件，並在使用中的 session 執行 `/reload-plugins`。

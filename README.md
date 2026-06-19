# ClaudeCodeDev

我（jolinjo）的 **Claude Code 開發環境設定備份**。

這個 repo 只保存根工作目錄 `C:\Users\JasonLin\Documents\ClaudeCode` 裡與 Claude Code 相關的設定檔，方便跨機器還原與版本追蹤。**各子專案（CODESYS、HMI、OPC UA 閘道等）有各自的 repo，不納入此處**，已用 `.gitignore` 排除。

## 內容

| 檔案 | 說明 |
|------|------|
| `CLAUDE.md` | Claude Code 的行為準則（通用開發守則）+ CodesysDemoProject 專案專屬規則 |
| `.mcp.json` | MCP server 設定，目前掛載 `codesys`（@codesys/mcp-toolkit），用來從編輯器同步並編譯 CODESYS 專案 |
| `.claude/settings.local.json` | Claude Code 權限允許清單（local 設定） |
| `.gitignore` | 排除子專案目錄 |

## 排除的子專案

下列目錄存在於工作目錄，但**不**納入此 repo：

- `CodesysDemoProject/` — CODESYS V3.5 SP22 + SoftMotion 範例專案
- `HMI_Fultter/` — HMI（Flutter）
- `OPCUA-Getway/` — OPC UA 閘道

## 環境需求（重建開發環境時）

- **Claude Code**
- **CODESYS** V3.5 SP22：`C:\Program Files\CODESYS 3.5.22.0\CODESYS\Common\CODESYS.exe`（profile `CODESYS V3.5 SP22`）
- **Node.js**：可攜式版放在 `C:\tools\node`
- **CODESYS MCP toolkit**：`@codesys/mcp-toolkit`（npm 全域裝在 `C:\tools\node`）

> 注意：`.mcp.json` 內的路徑為本機絕對路徑，在其他機器還原時需依實際安裝位置調整。
> 修改 `.mcp.json` 後需重啟 Claude Code 才會載入，首次使用 MCP 會跳安全核可。

## 使用方式

```bash
# 還原到工作目錄根
git clone https://github.com/jolinjo/ClaudeCodeDev.git
```

之後在工作目錄修改 Claude 設定檔，直接 `git add` → `commit` → `push` 即可；三個子專案會被自動忽略。

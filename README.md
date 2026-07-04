# ClaudeCodeDev

我（jolinjo）的 **Claude Code 開發環境設定備份**。

這個 repo 只保存根工作目錄 `C:\Users\JasonLin\Documents\ClaudeCode` 裡與 Claude Code 相關的設定檔，方便跨機器還原與版本追蹤。**各子專案（CODESYS、HMI、OPC UA 閘道等）有各自的 repo，不納入此處**，已用 `.gitignore` 排除。

## 內容

| 檔案 | 說明 |
|------|------|
| `CLAUDE.md` | Claude Code 的行為準則（通用開發守則）+ 各子專案專屬規則 |
| `.mcp.example.json` | MCP server 設定**範本**（codesys、qet）。實際 `.mcp.json` 不進版控，見下方「MCP 設定」 |
| `.claude/settings.local.json` | Claude Code 權限與允許清單（local 設定） |
| `.gitignore` | 排除子專案目錄與 `.mcp.json` |

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

## MCP 設定（`.mcp.json`）

`.mcp.json` 內含各機器的**絕對路徑**（且 codesys 只有 Windows、qet 的執行檔路徑 Mac/Win 不同），
若共用同一份會兩台互相覆蓋，因此**不納入版控**（已 gitignore），改用 `.mcp.example.json` 當範本。

**新機器初次設定：**

```bash
cp .mcp.example.json .mcp.json    # 複製範本
# 依 .mcp.example.json 內 _README 的 Windows / macOS 路徑說明，填入實際路徑
```

> ⚠️ **從舊 commit 更新的機器要注意**：`.mcp.json` 曾經被追蹤、後來才移出版控。
> 若你這台的 `.mcp.json` 還是「被追蹤」狀態，`git pull` 到移除 commit 時**會刪掉你的本地 `.mcp.json`**。
> pull 前請先備份：
>
> ```bash
> cp .mcp.json .mcp.json.bak
> git pull
> cp .mcp.json.bak .mcp.json    # pull 後還原（此時已被 gitignore，不會再進版控）
> ```

- 修改 `.mcp.json` 後需**重啟 Claude Code** 才會載入，首次使用 MCP 會跳安全核可。
- 修改 `qet` 的 `server.py` 需 `/mcp reconnect` 才生效。

## 使用方式

```bash
# 還原到工作目錄根
git clone https://github.com/jolinjo/ClaudeCodeDev.git
```

之後在工作目錄修改 Claude 設定檔，直接 `git add` → `commit` → `push` 即可；子專案與 `.mcp.json` 會被自動忽略。

# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

---

## Project: CodesysDemoProject

CODESYS V3.5 SP22 + SoftMotion 標準控制庫專案。

### 程式碼正本與工作流

- `src/Application/*.st` 是正本，用 VSCode 編輯。
- 「同步並編譯」透過 CODESYS MCP server（`codesys`）把 `.st` 推進 CODESYS：`open_project` → `set_pou_code` → `compile_project`。
- 用 MCP 操作時，**不要同時在 CODESYS UI 開同一個專案**（會檔案鎖衝突）。

### 環境

- CODESYS：`C:\Program Files\CODESYS 3.5.22.10\CODESYS\Common\CODESYS.exe`，profile `CODESYS V3.5 SP22 Patch 1`。（注意：`3.5.22.0` 目錄只有 base DLL、沒有 CODESYS.exe；實際 IDE 在 patch 版 `3.5.22.10`。）
- Node 為可攜式版，位於 `C:\tools\node`（系統未裝 Node，請用此處的 `npm.cmd` / `node.exe`）。
- MCP 設定在 `.mcp.json`；改完需重啟 Claude Code 才會載入。
- git 身分：Jason.Lin / jolinjo@gmail.com。

---

## Project: qet-mcp（AI 畫電氣圖）

- **用 qet MCP 工具或改 qet-mcp/ 之前，先讀 `qet-mcp/docs/AI-GUIDE.md`**
  （工具目錄、黃金工作流、佈局規範、實戰陷阱）。
- **自動維護文件，不需使用者提醒**：新增/修改工具、驗證新元件、發現新陷阱時，
  同步更新 AI-GUIDE.md、README 里程碑、data/aliases.json（+synonyms.json）。
- 相關 repo：`qet-mcp/`（工具鏈，jolinjo/qet-mcp）、`QET-qeletrotech/`
  （QET fork，jolinjo/QET，分支 QT6-MCP；原則：fork 改動越少越好）。
- commit 中文說明；QET fork 另需 CMakeLists patch 版本 +1；commit 前跑
  `qet-mcp/tests/test_roundtrip.py` 確認綠燈。
- server.py 改動需使用者 /mcp reconnect 才生效，改完要主動提醒。

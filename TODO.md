# Odoo AI 知識庫 — 待完成工作

> 給任何帳號的 Gemini：請先讀 `AGENTS.md` 和 `.agent/skills/icb_page_generator/SKILL.md`，
> 再使用這份清單繼續以下的知識庫補充工作。

---

## Phase 4：系統模板知識庫 ✅ 已完成

### 4-1. 產品列表 / Blog 列表 SCSS 規則
- [x] 建立 `.agent/skills/icb_page_generator/resources/system_pages_scss.md`
- 內容：Odoo `/shop`、`/blog` 系統頁面的 HTML 骨架 + CSS Selector 規則
- 規則：AI **絕對不能** 為這些頁面輸出 XML，只能輸出 SCSS

### 4-2. Header / Footer 模塊規則
- [x] 建立 `.agent/skills/icb_page_generator/resources/header_footer_rules.md`
- Header：AI **只能**輸出 SCSS（基於第一組 Header 選項）
- Footer：AI **必須**輸出完整 XPath XML（基於 Links 選項）+ 配套 SCSS

### 4-3. 聯絡表單規則
- [x] 建立 `.agent/skills/icb_page_generator/resources/form_rules.md`
- 佈局外殼 (Layout) + 原生表單投放 (Dropzone) 分離策略

### 4-4. 首頁樣板配方
- [x] 建立 `.agent/skills/icb_page_generator/resources/page_templates.md`
- Home 1~4 的組裝配方 (Recipes)

---

## Phase 5：OpenCode 設定 ✅ 已完成

- [x] 建立 `opencode.json`（由 `scripts/sync_icb_skill.py` 自動同步）
- [x] `.opencode/commands/` 六個指令已建好（page, dynamic, btn, js, block, icb）

---

## Phase 6：待辦

- [x] 導入 MCP Scraper 基礎能力（Playwright MCP）
- [x] 建立 `/clone` Free-First 規則（預設先用 Playwright，非必要不啟用付費服務）
- [x] 建立草稿沙盒原則（抓站結果只落在 `outputs/`，不自動寫入 `templates/`）
- [x] 建立 Promotion 保守機制（僅在使用者明確要求時才晉升公版/元件化）
- [ ] （可選）導入 Firecrawl 作為批次抓站加速器（需 API Key / 可能付費）
- [ ] 補一份 `/clone` 自動化驗證清單（抽樣頁面、動態區塊、QWeb 外框、RWD）

---

## 備忘

- 指令已縮短為：`/page`、`/dynamic`、`/btn`、`/js`、`/block`
- Bootstrap 4 響應式間距（`pt-md-5` 等）已列入 SCSS 知識庫，對 Section 內部使用
- 自訂 class 必須搭配 `data-custom-name` 屬性，否則編輯模式存檔後會消失
- 外掛模組（產品分類、特殊頁面）採用「設計師即時提供 HTML → AI 讀取後產出 SCSS」策略，不需要預先存入知識庫
- Skill 同步指令：`C:/Users/france0414/AppData/Local/Programs/Python/Python313/python.exe scripts/sync_icb_skill.py`

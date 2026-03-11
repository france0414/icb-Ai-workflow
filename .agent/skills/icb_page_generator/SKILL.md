---
name: icb_page_generator
description: ICB (Odoo 14) 網頁開發的統一知識入口。當使用者要求生成頁面、加入 Snippet、動態產品或部落格區塊、按鈕風格、互動 JS 元件，或詢問 Odoo 14 前端規範時，使用此 skill。
---

# ICB Page Generator

此檔為 ICB 專案的頁面生成 Skill 主文件。

> [!NOTE]
> `.agent/skills/icb_page_generator/SKILL.md`、`.agents/skills/icb_page_generator/SKILL.md` 與 OpenCode instructions 應維持語意一致。
> 核心知識來源固定為 `D:/gemini/odoo/.agent/skills/icb_page_generator/resources/` 與 `D:/gemini/odoo/AGENTS.md`。

## 統一讀取順序

1. 先讀 D:/gemini/odoo/AGENTS.md
2. 再讀 D:/gemini/odoo/TODO.md，確認目前尚未完成的知識庫工作
3. 讀 D:/gemini/odoo/docs/PROJECT_THEME.css 了解專案配色
4. 若使用者提供參考文字、圖片或文件，優先從 D:/gemini/odoo/clientInfo/ 讀取；公版結構參考從 D:/gemini/odoo/templates/ 讀取
5. 依需求讀取 resources/ 內的對應文件

## 核心規則

1. **XML 結構：** 所有頁面都必須由 <t t-name="..."><t t-call="website.layout">...</t></t> 包覆
2. **首頁：** 首頁需加 <t t-set="pageName" t-value="'homepage'"/>
3. **佈局容器：** 每個 <section> 內的核心框架必須明確選擇使用 .container (限制寬度置中) 或 .container-fluid (滿版)，不可遺漏或隨意自創網格外殼
4. **重疊與絕對定位保護：** 所有會導致元素重疊的 SCSS (如負邊距 mt-n5、絕對定位 position:absolute 覆蓋圖文) 必須加上 `#wrapwrap:not(.odoo-editor-editable)` 前綴，確保在 Odoo 編輯模式下元素會解開重疊，讓使用者能正常點擊並替換文字與圖片
5. **斷點規範：** 符合 Bootstrap 4.5 的斷點，請使用 Bootstrap 4.5 的斷點寫法（含 media-breakpoint-up/down mixin）。若需額外斷點，請使用 docs/user_custom_rules.scss 的自訂 RWD 斷點變數與 mixin（//--自訂RWD 斷點變數 開始--// 區塊）
6. **Snippet：** 必須有 data-snippet 和 data-name 屬性
7. **自訂命名：** s_custom_PascalCase 必須搭配 data-custom-name="PascalCase"
8. **圖片：** 使用 https://picsum.photos/[width]/[height] 作為佔位圖
9. **樣式：** 所有樣式只能寫在 SCSS，禁止 inline style 或 XML 內嵌 style
10. **間距：** 使用 Bootstrap 4 的 pt/pb 規則，優先採用 8 的倍數
11. **AI 雙核心任務模式：** 1.【內建積木組裝】要求生成新頁面/區塊時，必須優先從 `templates/` (設計資源庫) 尋找客製化結構、SCSS與JS做積木式拼裝，確保風格一致。`templates/` 裡的檔案並不需要全部強迫改為 Odoo 拖曳 Snippet，它們是供 AI 取材的「優質積木」。2.【外部佈景翻譯】要求抓取外部網站區塊時，AI 要拆解對方版型，優先找 `templates/` 內是否有可套用的積木，若無則生成相容 Odoo 的全新結構。外部參考素材統一放 clientInfo/。
12. **公版與客製 SCSS 提取原則：** 當 AI 判斷需要為特定組件（如特殊按鈕風格、動態區塊特有樣式）補上 SCSS 時，主來源絕對是 `templates/` 目錄中，與該 XML/HTML 檔名完全相同的 `.scss` 檔案（例如：要抓 `banner.xml` 的樣式，就去讀 `banner.scss`）！AI 必須精準提取原生代碼，嚴禁自行發明或通靈 CSS
13. **輸出位置：** 產出檔案放在 outputs/，檔名必須包含日期與時間
14. **重用全域樣式：** 若 user_custom_rules.scss 已有客製樣式（如 .s_custom_titleUnderLine, .s_custom_scaleL, 輪播箭頭位置等），AI 只需要套用 class，禁止重寫；若沒有對應樣式，則必須在輸出 SCSS 補上。詳見 scss_reference.md
15. **抓取競品轉化原則 (Scraping Sandbox)：** 當執行 /clone 或要求抓取外部網站區塊時，1. 嚴禁直接將草稿寫入 templates/，必須放在 outputs/。2. 嚴禁在 XML 內寫 <style id="scss-code">，必須產出獨立的 .scss 與 .xml。3. XML 最外圍必須遵守 <t t-name...><t t-call="website.layout"> 的 QWeb 標準層級。4. 動態區塊如新聞、產品必須對接 s_dynamic_snippet，嚴禁寫死前端假卡片結構。
16. **SCSS 精準輸出原則：** 輸出 SCSS 時，只輸出 XML 中**實際使用到的元件**對應的 SCSS，嚴禁整包傾倒整個來源 .scss 檔案。流程：1. 先確認 XML 使用了哪些 `s_custom_*` class 和 `data-custom-name`。2. 只從對應的 SCSS 檔案中提取這些 class 的樣式。3. 若 `user_custom_rules.scss` 已有的全域樣式（如 `titleUnderLine`、`scaleL`），只需在 XML 套用 class，SCSS 不重複輸出。

## 依需求讀取的知識庫

核心知識資源位於 `D:/gemini/odoo/.agent/skills/icb_page_generator/resources/`

| 任務 | 讀取文件 |
|------|---------|
| 選擇 Snippet / 了解嵌套規則 | `resources/snippet_catalog.md` |
| 生成動態產品 / 部落格區塊 | `resources/dynamic_rules.md` |
| 套用按鈕風格 | `resources/button_styles.md` |
| 加入互動 JS 元件 | `resources/component_library.md` |
| 設計版面配置 | `resources/layout_patterns.md` |
| 查詢 SCSS 變數 / Mixin / 斷點 | `resources/scss_reference.md` |
| 呼叫歷史客製化區塊 | `resources/custom_blocks.md` |
| 套用首頁樣板配方 (Home 1~4) | `resources/page_templates.md` |
| Header SCSS 覆寫 / Footer XML+SCSS 生成 | `resources/header_footer_rules.md` |
| 聯絡表單佈局與 SCSS 覆寫 | `resources/form_rules.md` |
| Blog / Shop 系統頁面 SCSS 覆寫 | `resources/system_pages_scss.md` |

## 尚未補齊但必須遵守的規則

以下知識文件仍在 `TODO.md` 的待完成項目內；在文件建立前，直接遵守這些限制：

1. **Header：** 禁止輸出 XML，只能輸出 SCSS 覆寫（基於第一組 Header 選項）
2. **Footer：** 必須輸出完整 XPath XML（基於 Links 選項）+ 配套 SCSS
3. **Blog / Shop / 系統頁面：** 禁止輸出 XML，只能輸出 SCSS 覆寫
4. **表單類區塊：** 佈局外殼 (Layout) + 原生表單投放 (Dropzone) 分離策略；若設計師提供既有 HTML，優先保留結構，只補 SCSS 與必要 class

## 可用指令

| 指令 | 說明 |
|------|------|
| `/page` | 生成完整 Odoo 頁面（靜態或動態） |
| `/dynamic` | 快速加入動態產品或部落格區塊 |
| `/btn` | 套用或建立按鈕風格 |
| `/js` | 加入互動 JS 元件 |
| `/block` | 呼叫已整理的客製化歷史區塊 |
| `/clone` | 抓取外部競品網站區塊並轉化為 Odoo 14 沙盒草稿 (Scraping sandbox) |

## 輸出原則

1. 頁面型任務通常輸出 XML + SCSS
2. 系統頁面型任務通常輸出 SCSS only
3. 若需求屬於 Odoo 系統自動生成頁面，先確認是否只能覆寫樣式，不能直接改 HTML/XML

## Resources 目錄

```
D:/gemini/odoo/.agent/skills/icb_page_generator/resources/
├── snippet_catalog.md
├── dynamic_rules.md
├── button_styles.md
├── component_library.md
├── layout_patterns.md
├── custom_blocks.md
├── scss_reference.md
├── page_templates.md
├── header_footer_rules.md
├── form_rules.md
└── system_pages_scss.md
```

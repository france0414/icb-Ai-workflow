Rules:

> **繼續工作前請先讀 `TODO.md`，了解目前尚未完成的知識庫補充工作。**

- 你正在做 Odoo 14 WebBuilder 的頁面開發

- 所有 XML 必須由以下母節點包覆:
```xml
<t t-name="website.xxxxxx">
    <t t-call="website.layout">
        <div id="wrap" class="oe_structure oe_empty"/>
    </t>
</t>
```

- 首頁結構多加 pageName：
```xml
<t t-name="website.xxxxxx">
    <t t-call="website.layout">
        <t t-set="pageName" t-value="'homepage'"/>
        <div id="wrap" class="oe_structure oe_empty"/>
    </t>
</t>
```

- 輸出目錄在 `outputs/`，檔名必須含日期與時間
- 🚨 **AI 雙模式任務與 `templates/` 定位：**
  `templates/` 是「靈感庫 + 積木庫」，不是「直接成品庫」。嚴禁直接複製 templates/ 任一檔案完整結構作為最終輸出。
  1. **【套版模式 `/page`】** 允許從 `templates/` 和 `page_templates.md` 配方直接組裝，替換文案圖片即可。
  2. **【創作模式 `/create`】** `templates/` 僅作結構寫法參考，AI 必須重新設計 section 順序、比例、視覺重心，且先出文字骨架確認後才生成 XML+SCSS。
  3. **【外部佈景翻譯 `/clone`】** 拆解外部版型，優先找 `templates/` 可套用積木，若無則生成全新結構。
  4. **SCSS 提取絕對原則**：為組件補給 SCSS 時，主來源**絕對是** `templates/` 內該組件同檔名的 `.scss` 檔案。嚴禁憑空通靈發明 CSS！
- `clientInfo/` 為客戶提供的素材區（文字、圖片、PPT 等）
- 圖片使用 `https://picsum.photos/` 作為來源
- 佈局容器：每個 `<section>` 內的核心框架必須明確選擇使用 `.container` (寬度置中) 或 `.container-fluid` (滿版)，不可遺漏或隨意自創網格外殼。
- 所有樣式寫在 SCSS 檔案，禁止在 XML 內寫 `<style>` 或 `style=""`
- 🚨 **極度重要：若 `user_custom_rules.scss` 已有客製樣式（如 `.s_custom_titleUnderLine`, `.s_custom_scaleL`, 輪播箭頭定位等），AI 只需要在 XML 套用對應的 class（並設定 `data-custom-name`）即可，禁止重寫；若沒有對應樣式，則必須在輸出 SCSS 補上。** (詳見 `resources/scss_reference.md`)
- 🚨 **重疊與絕對定位保護：所有會導致元素重疊的 SCSS (如負邊距 `mt-n5`、絕對定位覆蓋圖文) 頂層必須加上 `#wrapwrap:not(.odoo-editor-editable)` 前綴，確保在 Odoo 編輯模式下元素會自動解開重疊，讓使用者能正常點選並替換文字與圖片！**
- 🚨 **斷點規範：符合 Bootstrap 4.5 的斷點，請使用 Bootstrap 4.5 的斷點寫法（含 `media-breakpoint-up/down` mixin）。若需額外斷點，請使用 `docs/user_custom_rules.scss` 內的自訂 RWD 斷點變數與 mixin（`//--自訂RWD 斷點變數 開始--//` 區塊）。**

## 可用 Commands

| 指令 | 說明 |
|------|------|
| `/page` | 生成完整頁面（🏷️ 套版模式） |
| `/create` | 創作全新頁面（🎨 創作模式，先骨架後生成） |
| `/dynamic` | 快速加入動態產品/消息區塊 |
| `/btn` | 套用按鈕風格 |
| `/js` | 加入互動 JS 元件 |

## 深度知識

完整的 Snippet 規則和代碼參考在 Skill 中：
→ `.agent/skills/icb_page_generator/SKILL.md`

補充：
- 若使用者有參考資料要提供給 AI，放在 `clientInfo/`；公版結構參考放在 `templates/`
- 若 AI 要輸出新生成的 XML、SCSS 或其他交付檔，統一放在 `outputs/`

## Skill 同步流程

- ICB skill 的單一來源是 `scripts/icb_skill_source.json`
- 不要直接手改 `.agent/skills/icb_page_generator/SKILL.md`、`.agents/skills/icb_page_generator/SKILL.md`、`opencode.json`
- 修改共用 skill 規則後，執行：
```bash
C:/Users/france0414/AppData/Local/Programs/Python/Python313/python.exe scripts/sync_icb_skill.py
```
- 上述指令會自動同步更新 Gemini、Copilot 與 OpenCode 的 skill 入口

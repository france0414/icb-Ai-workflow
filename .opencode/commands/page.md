# page

依照 Odoo 14 Snippet 規則或樣板配方，生成頁面的完整 XML + SCSS。

## Steps

1. 讀取 Skill 主檔：`d:/gemini/odoo/.agent/skills/icb_page_generator/SKILL.md`
2. 讀取專案配色：`d:/gemini/odoo/docs/PROJECT_THEME.css`
3. 判斷頁面類型：完整首頁樣板(配方) / 靜態頁面 / 動態列表 / 靜態導航
4. 若為完整樣板，讀取 `resources/page_templates.md` 依照配方組裝。若含客製區塊讀取 `resources/custom_blocks.md`。
5. 若為一般頁面，從 `resources/snippet_catalog.md` 選擇 Snippet 組合。
6. 檢查是否需要特殊按鈕風格（→ `resources/button_styles.md`）
7. 輸出 XML + SCSS（圖片使用 `https://picsum.photos/`）

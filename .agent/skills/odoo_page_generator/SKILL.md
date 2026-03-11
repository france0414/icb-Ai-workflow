---
name: Odoo Page Generator
description: Standardized workflow for generating Odoo 14 compatible XML and SCSS based on strict layout rules.
---

# Odoo Page Generator Skill

This skill ensures that all generated Odoo pages strictly adhere to the `ODOO_LAYOUT_RULES.md` defined in the project.

## Instructions

### Step 1: Context Loading
**CRITICAL:** Before generating ANY code, you MUST first read the Rules of Engagement:
1.  **Primary Rule Source:** `d:/gemini/odoo/docs/ODOO_RULES_AI.md` (Read this first for core syntax and design strategies).
2.  **Project Theme Colors:** `d:/gemini/odoo/docs/PROJECT_THEME.css` (**MUST** read this to understand project-specific colors like --primary, o_cc1, etc.).
3.  **Dynamic Content Rule:** If the page involves **Dynamic Products** or **Dynamic Blogs**, you **MUST** read `d:/gemini/odoo/docs/ODOO_DYNAMIC_RULES.md`.
4.  **Button Styling Rule:** If the page involves specific button styles (Skew, Arrows, Flash, etc.), you **MUST** read `d:/gemini/odoo/docs/ODOO_BUTTON_STYLES.md`.
5.  **Full Reference (Optional):** `d:/gemini/odoo/docs/ODOO_LAYOUT_RULES.md` (Consult for detailed code examples if needed).

### Step 2: Content Analysis
Analyze the user's request to identify:
- **Page Type:** Homepage vs. Standard Page.
- **Dynamic vs Static:**
    - Is this a **Listing** (latest products, news)? -> Use **Dynamic Snippets** (Check `ODOO_DYNAMIC_RULES.md`).
    - Is this a **Category Navigation** (Shop by Category)? -> Use **Static Layouts** (e.g., `s_three_columns`). **DO NOT** use dynamic snippets for categories.
- **Snippet Selection:** Use the "Hierarchy Principle" from `ODOO_RULES_AI.md`.
- **Content:** Images (using picsum.photos), text, and specific functionality.

### Step 3: Code Generation
Generate the code following these STRICT guidelines obtained from `ODOO_RULES_AI.md`:
1.  **Structure:** Use `<div id="wrap">` with the appropriate container size (`.container`, `.container-fluid`, or `.o_container_small`).
2.  **Snippet Usage:** Follow the "Master Section Patterns" for high-impact visual design.
3.  **Utility Polish:** Always consider applying `pt/pb` spacing and `s_custom_` classes (e.g., `scaleL`, `reverse`, `RWDscroll`).
4.  **Image Handling:** **MUST REPLACE** Odoo default images with `https://picsum.photos/` placeholders.
5.  **Attributes:** Sync custom classes with `data-custom-name` and snippets with `data-snippet` attributes.

### Step 4: Output
Provide the output in two distinct blocks:
1.  **XML/QWeb:** The complete structure ready to be pasted into Odoo's HTML editor.
2.  **SCSS:** Any specific custom overrides required (referencing variables like `--container-pd-x`).

---

## Component Library Reference

**Location:** `d:/gemini/odoo/docs/ODOO_COMPONENT_LIBRARY.md`

This Markdown file contains clean, AI-optimized UI component documentation with separated XML structure, JavaScript, and SCSS. **Always read this file before implementing interactive components.**

### Available JS Components

| Component Name | Custom Class | Description | Max Items |
|----------------|--------------|-------------|-----------|
| Full Image Hover 1 | `s_custom_fullImgHoevr1` | Hover 切換大圖，左側導航按鈕 | 6 |
| Full Image Hover 2 | `s_custom_fullImgHoevr2` | Hover 滑入顯示背景大圖 | 7 |
| Exhibition Updates | `s_custom_exhibitionUpdates` | 右側固定滑入新聞面板 | - |
| Product Right Scroll | `s_custom_productRightScroll` | 水平滾動產品卡片列 | 6 |
| Interactive Map | `s_custom_mapEffect` | 地圖互動標記點 | 5 |
| Sticky Background | `s_custom_stickyBG` | 內文固定 + 背景圖 fix | - |
| Full Hover Background | `s_custom_fullHoverBackground` | Icon 觸發背景圖切換 | 6 |
| Worldwide Scroll | `s_custom_worldwide` | 上圖下文卡片水平捲軸 | - |
| Worldwide RWD Collapse | `s_custom_worldwideRWDcollapse` | 桌機捲軸 / RWD 收合 | - |

---

## Embedded JS Rules

When embedding JavaScript in Odoo pages, follow this structure:

```xml
<!-- ✅ 正確結構：嵌入 JS 區塊 -->
<section class="s_embed_code o_colored_level s_custom_jqCode s_custom_noRemove" 
         data-snippet="s_embed_code" 
         data-name="Embed Code" 
         data-custom-name="jqCode noRemove">
  <div class="s_embed_code_embedded o_not_editable container">
    <script>
      // JavaScript code here
    </script>
  </div>
</section>
```

**Key Rules:**
1.  **MUST add** `s_custom_noRemove` class to protect the section from accidental deletion.
2.  **MUST update** `data-custom-name` to include `noRemove`.
3.  **DO NOT add** visible text labels inside the section. The shared SCSS will display "重要 Code，請勿刪除" in editor mode via `::before` pseudo-element.
4.  In published mode, visitors see nothing but the JS functionality works.

# Odoo 14 AI Layout Reference (CONCISE)

This is a high-density reference for AI to generate Odoo 14 XML/SCSS correctly.

## 1. Core Structure
## 0. SCSS/CSS 樣式規範
- **所有樣式必須獨立寫在 SCSS 檔案**（如 docs/user_custom_rules.scss），**禁止在 XML 頁面內寫 <style> 或 style="" 或直接寫 CSS**。
- XML 只負責結構與 class，樣式由 SCSS 控制，方便彈性合併與管理。
- **Root:** `<div id="wrap" class="oe_structure oe_empty">`
- **Container Sizes:**
    - `.container`: Standard width (Max 1550px, **90% width on LG+ screens**). Most common.
    - `.container-fluid`: Full width (100% width, edge-to-edge).
    - `.o_container_small`: Narrow width (Max 1200px, mostly for articles/blogs).
- **Horizontal Padding:** All containers use `var(--container-pd-x)` (Base 30px, RWD adjusted from 26px to 60px).
- **Nesting:** `#wrap` > `section (Snippet)` > `[.container | .container-fluid | .o_container_small]` > `.row` > `.col` > `Content`.
- **Snippet Requirement:** MUST use `data-snippet` and `data-name` on the root `section`.
- **SEO Heading Rule:**
    - **One `<h1>`** per page (Hero Title).
    - **Strict Hierarchy:** `<h1>` -> `<h2>` -> `<h3>` -> `<h4>`. Do not skip levels.
    - **Visual Sizing:** If text is NOT a heading but needs size, use classes `.h1` ~ `.h6` on `<div>` or `<p>`.
    - **Eyebrows:** Use `<div class="h5 text-primary text-uppercase">` instead of `<h5>` for pre-titles.
- **Custom Section Naming Rule:**
    - **Format:** `PascalCase` (e.g., `HeroNavigator`, `NewsCards`).
    - **Forbidden:** No spaces, dashes `-`, or underscores `_` inside a name.
    - **Multiple Names:** Separate with space (e.g., `data-custom-name="NewsCards ScaleL"`).
    - **Matching Class:** `s_custom_YourName` (e.g., `s_custom_HeroNavigator`).

## 2. Global Styling (GTMC Custom)
### Spacing (RWD Adjusted)
- Classes: `pt8`, `pt16`, ... `pt256` (Increments of 8).
- Same for `pb` (padding-bottom).

### Color System (`o_cc`)
- **Concept:** Odoo uses `o_cc1` (White), `o_cc2` (Light Gray), `o_cc3` (Primary), `o_cc4` (Secondary), `o_cc5` (Dark) to control background **AND** text colors automatically.
- **Rule:** When using `o_ccX`, **DO NOT** add `text-white` or `text-dark` manually. Trust the theme.
    - *Exception:* Specific brand highlights (e.g., `text-primary` on a heading).

### Utilities (s_custom_ prefix)
- `s_custom_scaleL` / `s_custom_scaleS`: Hover scale effect.
- `s_custom_nameHoverUnderLine`: Title underline on hover.
- `s_custom_reverse`: Flip `.row` direction (`flex-direction: row-reverse`).
- `s_custom_RWDscroll`: Mobile horizontal scroll on `.static_snippet_template > .row`.
- `s_custom_GapM` (10px) / `s_custom_GapS` (5px): Column gaps.
- `s_custom_noRemove`: Red indicator for "Do not delete".

## 3. Core Snippet Syntax (Templates)
### Standard Text (`s_text_block`)
```xml
<section class="s_text_block pt40 pb40 o_cc o_cc1" data-snippet="s_text_block" data-name="Text">
    <div class="container s_allow_columns"><p>Content</p></div>
</section>
```

### Static Template (`s_static_snippet`)
**Class:** `.static_snippet_template [TEMPLATE]`
- `[TEMPLATE]` values: `s_product_product_centered`, `s_product_product_classic`, `s_product_product_horizontal`, `s_product_product_large_banner`, `s_blog_post_big_picture`.

```xml
<section class="s_static_snippet" data-snippet="s_static_snippet" data-name="Static Snippet">
    <div class="container">
        <div class="static_snippet_template s_product_product_centered">
            <div class="row">...</div>
        </div>
    </div>
</section>
```

## 4. Design Strategies (For Beautiful Pages)
### A. The "Hierarchy" Principle
- **Hero:** Use `s_banner` or `s_cover`. Select `o_cc1` ~ `o_cc5` based on desired background/text contrast.
- **Introduction:** Use `s_column_layout` (Left: Title, Right: Description) for a professional structural look.
- **Features:** Use `s_three_columns` or `s_features_grid` with `s_custom_scaleL`.
- **Social Proof:** Use `s_references` (Logos) or `s_quotes_carousel` (Testimonials).
- **Final CTA:** Use `s_call_to_action` with `o_cc3` to close the page.

### B. Color Rhythm (CC Pacing)
- Alternate background colors to create "rhythm": `o_cc1` (White) -> `o_cc2` (Gray) -> `o_cc1` -> `o_cc3` (Primary).

### C. Master Section Patterns
- **Modern Listing:** `s_static_snippet` + `s_blog_post_card` + `s_custom_GapM` + `s_custom_scaleL`.
- **Product Showcase:** `s_static_carousel` + `s_product_product_centered` + `s_custom_nameHoverUnderLine`.
- **Horizontal Feature:** `s_static_snippet` + `s_product_product_horizontal` + `s_custom_reverse` (alternate every row).

## 6. Generation Process
1.  **Ingest:** Read this file + User request.
2.  **Plan:** Select the specific Snippet + Template + CC Class chain.
3.  **Apply Utilities:** Always consider adding `pt/pb` and `s_custom_` classes for polish.
4.  **Place Objects:** Use `https://picsum.photos/` for all images.

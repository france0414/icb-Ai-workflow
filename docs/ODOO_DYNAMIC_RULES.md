# Odoo Dynamic Snippet Rules (STRICT)

> [!IMPORTANT]
> This file is the **SINGLE SOURCE OF TRUTH** for generating Dynamic Products and Dynamic Blogs/News.
> **DO NOT** invent new structures. You **MUST** use the exact XML patterns below.

## 1. Global Precautions
> [!WARNING]
> **White Background Issue**: Odoo's default `.card` and `.card-body` have a white background (`#FFFFFF`).
> - This often obscures custom colored backgrounds.
> - **Fix**: If the design requires a transparent or colored background, you **MUST** override this in SCSS or use a custom class associated with background removal.

## 2. Nesting & Placement (CRITICAL)
> [!TIP]
> **Safe Nesting**: While Dynamic Snippets are typically top-level `<section>` elements, they **CAN** be placed inside specific layout snippets if multi-column design is needed.
>
> **Allowed Parent Containers:**
> - `s_text_block` (with `.row` > `.col`)
> - `s_column_layout` (or legacy `s_three_columns`)
>
> **FORBIDDEN:**
> - **DO NOT** create custom `<div>` wrappers or random HTML grids to hold these snippets.
> - **DO NOT** wrap them in purely structural divs without a parent Snippet definition (like `s_text_block`).
> - **DO NOT MODIFY INNER HTML**: The content inside `<section>` MUST be exactly as defined in the templates. **NO adding titles, divs, or text inside the snippet section.**
> - Always rely on Odoo's standard layout structures to ensure drag-and-drop safety.

---

## 3. Dynamic Products (E-Commerce)

### A. Strict XML Structure
**Snippet:** `s_dynamic_snippet_products` OR `s_dynamic_snippet_carousel`
**Filter ID:** `3` (Standard for Products) or `all` (if category specific).

```xml
<section 
    data-snippet="s_dynamic_snippet_products" 
    class="s_dynamic_snippet_products s_dynamic o_colored_level [TEMPLATE_CLASS] [CUSTOM_CLASSES]" 
    data-name="Products" 
    data-filter-id="3" 
    data-template-key="[TEMPLATE_KEY]" 
    data-product-category-id="all" 
    data-number-of-elements="4" 
    data-number-of-elements-small-devices="1" 
    data-number-of-records="16" 
    data-carousel-interval="5000" 
    data-custom-name="[CUSTOM_NAMES]">
    
    <div class="o_not_editable container">
        <div class="css_non_editable_mode_hidden">
            <div class="missing_option_warning alert alert-info rounded-0 fade show d-none d-print-none o_default_snippet_text"></div>
        </div>
        <div class="dynamic_snippet_template"></div>
    </div>
</section>
```
*For Carousel version, change `data-snippet` to `s_dynamic_snippet_carousel` and class to `s_dynamic_snippet_carousel`.*

### B. Valid Template Keys (Products)
| Style Name | Template Key | Class Name |
| :--- | :--- | :--- |
| **Borderless Card 1** | `website_sale.dynamic_filter_template_product_product_borderless_1` | `s_product_product_borderless_1` |
| **Borderless Card 2** | `website_sale.dynamic_filter_template_product_product_borderless_2` | `s_product_product_borderless_2` |
| **Centered Product** | `website_sale.dynamic_filter_template_product_product_centered` | `s_product_product_centered` |
| **Classic Card** | `website_sale.dynamic_filter_template_product_product_add_to_cart` | `s_product_product_add_to_cart` |
| **Detailed Product** | `website_sale.dynamic_filter_template_product_product_view_detail` | `s_product_product_view_detail` |
| **Horizontal Card** | `website_sale.dynamic_filter_template_product_product_horizontal_card` | `s_product_product_horizontal_card` |
| **Large Banner** | `website_sale.dynamic_filter_template_product_product_banner` | `s_product_product_banner` |
| **Image Only** | `website_sale.dynamic_filter_template_product_product_mini_image` | `s_product_product_mini_image` |
| **Image w/ Name** | `website_sale.dynamic_filter_template_product_product_mini_name` | `s_product_product_mini_name` |

### C. valid Custom Classes (Products)
*Must be added to `class` AND `data-custom-name`*
- `scaleL` / `scaleS` (Hover zoom)
- `nameHoverUnderLine` (Title underline on hover)
- `imgNoMargin` (Remove image spacing)
- `arrowRight` (Arrow indicator)
- `titleHoverFull`

---

## 4. Dynamic Blogs / Latest News

### A. Strict XML Structure
**Snippet:** `s_dynamic_snippet` OR `s_dynamic_snippet_carousel`
**Filter ID:** `1` (Strictly required for Blogs)

```xml
<section 
    data-snippet="s_dynamic_snippet" 
    class="s_dynamic_snippet s_dynamic o_colored_level [TEMPLATE_CLASS] [CUSTOM_CLASSES]" 
    data-name="Dynamic Snippet" 
    data-filter-id="1" 
    data-template-key="[TEMPLATE_KEY]" 
    data-number-of-elements="4" 
    data-number-of-elements-small-devices="1" 
    data-number-of-records="4" 
    data-custom-name="[CUSTOM_NAMES]">
    
    <div class="o_not_editable container">
        <div class="css_non_editable_mode_hidden">
            <div class="missing_option_warning alert alert-info rounded-0 fade show d-none d-print-none o_default_snippet_text"></div>
        </div>
        <div class="dynamic_snippet_template"></div>
    </div>
</section>
```

### B. Valid Template Keys (Blogs)
| Style Name | Template Key | Class Name |
| :--- | :--- | :--- |
| **Big Picture** | `website_blog.dynamic_filter_template_blog_post_big_picture` | `s_blog_post_big_picture` |
| **Card Layout** | `website_blog.dynamic_filter_template_blog_post_card` | `s_blog_post_card` |
| **Horizontal** | `website_blog.dynamic_filter_template_blog_post_horizontal` | `s_blog_post_horizontal` |
| **List Layout** | `website_blog.dynamic_filter_template_blog_post_list` | `s_blog_post_list` |

### C. Valid Custom Classes (Blogs)
| Class Name | Description | Compatible Layouts |
| :--- | :--- | :--- |
| `cardRadius` | 20px Border Radius | All |
| `cardRadiusS` | 10px Border Radius | All |
| `borderHover` | Border color change on hover. | Card |
| `hoverUnderLine` | Title hover underline | All |
| `titleLine` | Static title underline | All |
| `blockLine` | Separator lines | List |
| `picTop` | Picture on Top | Horizontal |
| `picBottom` | Picture on Bottom | Horizontal, Card |
| `dateTop` | Date above text | Card |

---

## 5. Helper Table: Which Snippet Do I Use?

| Content Type | Display Mode | Snippet Name (`data-snippet`) |
| :--- | :--- | :--- |
| **Products** | Grid / List | `s_dynamic_snippet_products` |
| **Products** | Carousel / Slider | `s_dynamic_snippet_carousel` |
| **News / Blogs** | Grid / List | `s_dynamic_snippet` |
| **News / Blogs** | Carousel / Slider | `s_dynamic_snippet_carousel` |

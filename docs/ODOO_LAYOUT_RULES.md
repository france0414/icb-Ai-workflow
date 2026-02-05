# Odoo 14 排版規則書與代碼生成指南

> [!TIP]
> **AI 閱讀建議**: 若您是 AI 助手，為了提高效率，建議優先閱讀 [ODOO_RULES_AI.md](file:///d:/gemini/odoo/docs/ODOO_RULES_AI.md)，該文件提供更精簡的高密度規則。

此文件為生成 Odoo 14 網頁 XML 與 SCSS 的核心規則。AI 在執行生成任務時必須嚴格遵守此文件中的結構與規範。

## 0. 頁面基本結構 (Page Root Structure)
若 AI 需生成「整頁」代碼，請使用以下 QWeb 模板結構。所有內容必須放置於 `<div id="wrap">` 內部。

```xml
<!-- 1. 一般頁面 (Standard Page) -->
<t t-name="website.your_page_name">
    <t t-call="website.layout">
        <div id="wrap" class="oe_structure oe_empty">
            <!-- 所有排版區塊 (Layout Snippets) 必須放置於此 -->
        </div>
    </t>
</t>

<!-- 2. 首頁 (Homepage) -->
<t name="Home" t-name="website.homepage">
  <t t-call="website.layout">
    <!-- 首頁必須包含此變數設定 -->
    <t t-set="pageName" t-value="'homepage'"/>
    <div id="wrap" class="oe_structure oe_empty">
      <!-- 首頁內容 -->
    </div>
  </t>
</t>
```

## 1. 核心巢狀規則 (Nesting Hierarchy)
Odoo 的拖曳區塊有嚴格的層級關係，必須遵守以下順序：

1.  **頁面容器 (Page Wrapper)**: 如上所述，`#wrap`。
2.  **排版區塊 (Layout Snippet)**: 必須直接位於 `#wrap` 內。
3.  **基本結構 (Basic Structure)**: 位於排版區塊內的「容器」中。本系統支援三種容器尺寸：
    *   `.container`: **標準寬度** (Max 1550px)。在桌面版 (LG+) 會自動限制為 **90% 寬度**，以確保內容不會過於貼近螢幕邊緣。
    *   `.container-fluid`: **全寬** (Edge-to-Edge)。100% 滿版，用於背景滿版區塊。
    *   `.o_container_small`: **小寬度** (Max 1200px)。適合落落長的文章、部落格內文。
    *   **全局左右邊距**: 所有容器的左右 Padding 均由 CSS 變數 `--container-pd-x` 統一控制。預設值為 30px，並會根據螢幕尺寸（Laptop / LG / XS / Mobile-S）在 26px 到 60px 之間自動切換。
4.  **內容 (Content)**: 實際的文字、圖片等。

> [!IMPORTANT]
> 「排版區塊」是用來解決背景無法共用的問題，因此**不可**將多個排版區塊直接並排或是將內容直接丟在排版區塊外。

> [!CAUTION]
> **靜態 vs 動態區分 (Static vs Dynamic Distinction)**
> *   **產品/文章「列表」 (Listings)**: 若涉及「自動撈取」最新產品或文章，**必須** 使用動態區塊，並嚴格遵守 [ODOO_DYNAMIC_RULES.md](file:///d:/gemini/odoo/docs/ODOO_DYNAMIC_RULES.md)。
> *   **產品「分類」 (Categories)**: 「產品分類導航」（例如首頁的 Shop by Category）屬於 **靜態內容**。請使用標準排版區塊（如 `s_three_columns` 或 `s_media_list`）手動構建，**切勿** 使用動態區塊。

---

## 2. 客製化佈局區塊 (Custom Layout Definitions)
*這些是專案特定的客製化區塊，若環境中有安裝 `addon_gtmc_website` 則優先使用。*

### A. 垂直排版 (Vertical Layout)
*   **用途**: 單欄式內容，適合 Hero Section 或一般文字區塊。
*   **特性**: 內部 `oe_structure` 應巢狀其他區塊 (例如 `s_text_block`)。
*   **代碼**:
```xml
<section data-snippet="s_vertical_layout" class="s_vertical_layout pt32 pb32 o_colored_level" data-name="Vertical Layout">
    <div class="container-fluid">
        <div class="oe_structure oe_structure_not_nest oe_empty">
            <!-- 在此巢狀區塊，例如 s_text_block -->
        </div>
    </div>
</section>
```

### B. 水平排版 (Horizontal/Column Layout)
*   **用途**: 左右兩欄 (左側標題 4 / 右側內容 8) 的設計，左側支援 sticky。
*   **代碼**:
```xml
<section class="s_column_layout o_auto_screen_height o_colored_level" data-sticky="used" data-snippet="s_column_layout" data-name="Horizontal Layout">
    <div class="col-wrapper container">
        <div class="row d-flex align-items-stretch">
            <div class="col-lg-4 s_col_no_bgcolor col-left">
                <!-- 左側：標題區 (可黏著) -->
                <div class="s_column_layout_content oe_structure oe_structure_not_nest oe_empty col-sticky">
                    <!-- 填入內容，如 s_title -->
                </div>
            </div>
            <div class="col-lg-8 col-right o_colored_level">
                <!-- 右側：主要內容區 -->
                <div class="s_column_layout_content oe_structure oe_structure_not_nest oe_empty">
                    <!-- 填入內容，如 s_text_block -->
                </div>
            </div>
        </div>
    </div>
</section>
```

### C. 收合排版 (Accordion/Collapse Layout)
*   **用途**: 可展開/收合的內容區。
*   **代碼**:
```xml
<section class="s_collapse_layout pt32 pb32 o_colored_level" data-preview-height="200px" data-snippet="s_collapse_layout" data-name="Collapse Layout">
    <div class="container-fluid">
        <!-- 必須生成唯一的 ID，並對應到下方按鈕的 data-content-wrap -->
        <div class="s_collapse_height_wrap" style="height: 200px;" id="myUniqueCollapseID">
            <div class="s_collapse_content_wrap">
                <div class="s_collapse_content oe_structure oe_structure_not_nest oe_empty">
                     <!-- 填入隱藏內容 -->
                </div>
            </div>
            <div class="s_collapse_btn_wrap justify-content-center d-flex w-100 pt24 pb24">
                <div class="s_collapse_btn" data-content-wrap="myUniqueCollapseID">
                    <div class="btn btn-primary o_not-animable s_collapse_btn_expand">
                        <span class="o_default_snippet_text">More</span>
                    </div>
                    <div class="btn btn-primary o_not-animable s_collapse_btn_reduce">
                        <span class="o_default_snippet_text">Close</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

---

## 3. Odoo 標準拖曳模組庫 (Standard Snippet Library)
*這些是 Odoo 14 內建的標準區塊，適用於任何 Odoo 環境。AI 應優先使用這些區塊來構建通用頁面。*

### A. 首頁與結構 (Hero & Structure)

#### Banner (`s_banner`)
全寬視差滾動橫幅，包含標題框。
```xml
<section class="s_banner parallax s_parallax_is_fixed pt96 pb96 o_colored_level" data-scroll-background-ratio="1" data-snippet="s_banner" data-name="Banner">
    <span class="s_parallax_bg oe_img_bg" style="background-image: url('/web/image/website.s_banner_default_image'); background-position: 50% 0;"></span>
    <div class="container">
        <div class="row s_nb_column_fixed">
            <div class="col-lg-6 jumbotron rounded o_cc o_cc1 pt32 pb32 o_colored_level" data-name="Box">
                <h2 class="o_default_snippet_text">Sell Online. Easily.</h2>
                <p class="lead o_default_snippet_text">This is a simple hero unit, a simple jumbotron-style component.</p>
                <a class="btn btn-primary mb-2 o_default_snippet_text" href="#">Contact Us</a>
            </div>
        </div>
    </div>
</section>
```

#### Cover (`s_cover`)
全螢幕滿版背景，文字置中，帶有遮罩。
```xml
<section class="s_cover parallax s_parallax_is_fixed bg-black-50 pt96 pb96 o_colored_level" data-scroll-background-ratio="1" data-snippet="s_cover" data-name="Cover">
    <span class="s_parallax_bg oe_img_bg" style="background-image: url('/web/image/website.s_cover_default_image'); background-position: 50% 0;"></span>
    <div class="o_we_bg_filter bg-black-50"></div>
    <div class="container s_allow_columns">
        <h2 style="text-align: center; font-weight: bold;" class="o_default_snippet_text">Catchy Headline</h2>
        <p class="lead o_default_snippet_text" style="text-align: center;">Write one or two paragraphs describing your product.</p>
        <p style="text-align: center;">
            <a class="btn btn-primary mb-2 o_default_snippet_text" href="#">Contact us</a>
        </p>
    </div>
</section>
```

#### Title (`s_title`)
簡單的標題區塊。
```xml
<section class="s_title pt40 pb40 o_colored_level" data-snippet="s_title" data-name="Title">
    <div class="container s_allow_columns">
        <h2 class="o_default_snippet_text">Your Site Title</h2>
    </div>
</section>
```

#### Text Block (`s_text_block`)
一般文字區塊。
```xml
<section class="s_text_block pt40 pb40 o_colored_level" data-snippet="s_text_block" data-name="Text">
    <div class="container s_allow_columns">
        <p class="o_default_snippet_text">Great stories have a <b class="o_default_snippet_text">personality</b>.</p>
    </div>
</section>
```

### B. 圖文內容 (Media & Text)

#### Text - Image (`s_text_image`)
左文右圖。
```xml
<section class="s_text_image pt32 pb32 o_colored_level" data-snippet="s_text_image" data-name="Text - Image">
    <div class="container">
        <div class="row align-items-center">
            <div class="col-lg-6 pt16 pb16 o_colored_level">
                <h2 class="o_default_snippet_text">A Section Subtitle</h2>
                <p class="o_default_snippet_text">Write one or two paragraphs describing your product.</p>
                <p><a href="#" class="btn btn-primary mb-2 o_default_snippet_text">Learn more</a></p>
            </div>
            <div class="col-lg-6 pt16 pb16 o_colored_level">
                <img src="/web/image/website.s_text_image_default_image" class="img img-fluid mx-auto" alt="" loading="lazy">
            </div>
        </div>
    </div>
</section>
```

#### Image - Text (`s_image_text`)
左圖右文。
```xml
<section class="s_text_image pt32 pb32 o_colored_level" data-snippet="s_image_text" data-name="Image - Text">
    <div class="container">
        <div class="row align-items-center">
            <div class="col-lg-6 pt16 pb16 o_colored_level">
                <img src="/web/image/website.s_image_text_default_image" class="img img-fluid mx-auto" alt="" loading="lazy">
            </div>
            <div class="col-lg-6 pt16 pb16 o_colored_level">
                <h2 class="o_default_snippet_text">Section Subtitle</h2>
                <p class="o_default_snippet_text">Write one or two paragraphs describing your product.</p>
                <p><a href="#" class="btn btn-primary mb-2 o_default_snippet_text">Discover more</a></p>
            </div>
        </div>
    </div>
</section>
```

#### Picture (`s_picture`)
上方標題，下方大圖。
```xml
<section class="s_picture pt48 pb24 o_cc o_cc2 o_colored_level" data-snippet="s_picture" data-name="Picture">
    <div class="container">
        <h2 style="text-align: center;"><font class="o_default_snippet_text">A punchy Headline</font></h2>
        <p style="text-align: center;" class="o_default_snippet_text">Choose a vibrant image and write an inspiring paragraph.</p>
        <div class="row s_nb_column_fixed">
            <div class="col-lg-10 offset-lg-1 pb24 o_colored_level" style="text-align: center;">
                <figure class="figure">
                    <img src="/web/image/website.s_picture_default_image" class="figure-img img-thumbnail padding-large" alt="" loading="lazy">
                    <figcaption class="figure-caption text-muted py-3 o_default_snippet_text">Add a caption</figcaption>
                </figure>
            </div>
        </div>
    </div>
</section>
```

### C. 多欄與功能 (Columns & Features)

#### Three Columns (`s_three_columns`)
三欄卡片式佈局。
```xml
<section class="s_three_columns o_cc o_cc2 pt32 pb32 o_colored_level" data-snippet="s_three_columns" data-name="Columns">
    <div class="container">
        <div class="row d-flex align-items-stretch">
            <div class="col-lg-4 s_col_no_bgcolor pt16 pb16">
                <div class="card bg-white h-100">
                    <img class="card-img-top" src="/web/image/website.s_three_columns_default_image_1" alt="" loading="lazy">
                    <div class="card-body">
                        <h3 class="card-title o_default_snippet_text">Feature One</h3>
                        <p class="card-text o_default_snippet_text">Adapt these three columns to fit your design need.</p>
                    </div>
                </div>
            </div>
            <!-- 重複 col-lg-4 結構 -->
        </div>
    </div>
</section>
```

#### Features (`s_features`)
三欄圖標+標題。
```xml
<section class="s_features pt32 pb32 o_colored_level" data-snippet="s_features" data-name="Features">
    <div class="container">
        <div class="row">
            <div class="col-lg-4 pt32 pb32 text-center o_colored_level">
                <i class="fa fa-3x fa-gear rounded bg-primary m-3"></i>
                <h3 class="o_default_snippet_text">First Feature</h3>
                <p class="o_default_snippet_text">Tell what's the value for the customer.</p>
            </div>
            <!-- 重複 col-lg-4 結構 -->
        </div>
    </div>
</section>
```

#### Big Boxes (`s_color_blocks_2`)
雙欄大色塊。
```xml
<section class="s_color_blocks_2" data-snippet="s_color_blocks_2" data-name="Big Boxes">
    <div class="container-fluid">
        <div class="row">
            <div class="col-lg-6 o_cc o_cc3 text-center o_colored_level">
                <i class="fa fa-shield fa-5x m-3"></i>
                <h2 class="o_default_snippet_text">A color block</h2>
                <p class="o_default_snippet_text">Color blocks are a simple and effective way.</p>
                <a href="#" class="btn btn-primary mb-2 o_default_snippet_text">More Details</a>
            </div>
             <!-- 重複 col-lg-6 結構 -->
        </div>
    </div>
</section>
```

#### Media List (`s_media_list`)
媒體列表（左圖右文列表）。
```xml
<section class="s_media_list pt32 pb32 o_cc o_cc2 o_colored_level" data-snippet="s_media_list" data-name="Media List">
    <div class="container">
        <div class="row s_nb_column_fixed s_col_no_bgcolor">
            <div class="col-lg-12 s_media_list_item pt16 pb16" data-name="Media item">
                <div class="row s_col_no_resize s_col_no_bgcolor no-gutters align-items-center o_cc o_cc1 o_colored_level">
                    <div class="col-lg-4 align-self-stretch s_media_list_img_wrapper">
                        <img src="/web/image/website.s_media_list_default_image_1" class="s_media_list_img h-100 w-100" alt="" loading="lazy">
                    </div>
                    <div class="col-lg-8 s_media_list_body">
                        <h3 class="o_default_snippet_text">Media heading</h3>
                        <p class="o_default_snippet_text">Use this snippet to build various types of components.</p>
                        <a href="#" class="btn btn-primary mb-2 o_default_snippet_text">Discover</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

### D. 網格與畫廊 (Grid & Gallery)

#### Masonry (`s_masonry_block`)
*注意：Masonry 有多種 `data-masonry-template`，以下為 Default Reversed 範例。*
```xml
<section class="s_masonry_block" data-snippet="s_masonry_block" data-name="Masonry" data-masonry-template="default_reversed">
    <div class="container-fluid">
        <div class="row">
            <div class="col-lg-6 s_col_no_resize">
                <div class="row h-100">
                    <div class="col-lg-6 pt24 pb8 text-center o_cc o_cc3" data-name="Block">
                        <h3>A great title</h3>
                        <p>And a great subtitle</p>
                    </div>
                    <!-- 其他方塊 -->
                </div>
            </div>
            <div class="col-lg-6 oe_img_bg text-center pb224 pt224" data-name="Block" style="background-image: url(/web/image/website.s_masonry_block_default_image_1);">
            </div>
        </div>
    </div>
</section>
```

#### Numbers (`s_numbers`)
數據統計。
```xml
<section class="s_numbers o_cc o_cc2 pt24 pb24 o_colored_level" data-snippet="s_numbers" data-name="Numbers">
    <div class="container">
        <div class="row">
            <div class="col-lg-3 text-center pt24 pb24 o_colored_level">
                <span class="s_number display-4 o_default_snippet_text">12</span><br>
                <h6 class="o_default_snippet_text">Useful options</h6>
            </div>
            <!-- 重複 -->
        </div>
    </div>
</section>
```

#### Carousel (`s_carousel`)
輪播圖。
```xml
<section class="s_carousel_wrapper" data-snippet="s_carousel" data-name="Carousel">
    <div class="s_carousel s_carousel_default carousel slide" data-interval="10000" data-ride="carousel" id="myCarousel_UniqueID">
        <ol class="carousel-indicators o_we_no_overlay">
            <li data-slide-to="0" class="active" data-target="#myCarousel_UniqueID"></li>
        </ol>
        <div class="carousel-inner">
            <div class="carousel-item pt152 pb152 oe_img_bg o_bg_img_center o_colored_level active" style="background-image: url('/web/image/website.s_carousel_default_image_1');" data-name="Slide">
                <div class="container oe_unremovable">
                    <div class="row content">
                        <div class="carousel-content col-lg-6 o_colored_level">
                            <h2><font class="o_default_snippet_text">Slide Title</font></h2>
                            <p class="lead o_default_snippet_text">Use this snippet to presents your content.</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <a class="carousel-control-prev o_not_editable o_we_no_overlay" data-slide="prev" role="img" href="#myCarousel_UniqueID">
             <span class="carousel-control-prev-icon"></span>
        </a>
        <a class="carousel-control-next o_not_editable o_we_no_overlay" data-slide="next" role="img" href="#myCarousel_UniqueID">
             <span class="carousel-control-next-icon"></span>
        </a>
    </div>
</section>
```

---

### E. 內容結構與導航 (Content Structure & Navigation)

#### Tabs (`s_tabs`)
此區塊包含多個標籤頁，可切換內容。支援多種樣式 (Default, Justified 等)，結構相同。
```xml
<section class="s_tabs pt48 pb48 o_colored_level" data-vcss="001" data-snippet="s_tabs" data-name="Tabs">
    <div class="container">
        <div class="s_tabs_main">
            <div class="s_tabs_nav mb-3">
                <ul class="nav nav-pills" role="tablist">
                    <li class="nav-item">
                        <a class="nav-link active o_default_snippet_text" id="nav_tabs_link_1" data-toggle="tab" href="#nav_tabs_content_1" role="tab" aria-controls="nav_tabs_content_1" aria-selected="true">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link o_default_snippet_text" id="nav_tabs_link_2" data-toggle="tab" href="#nav_tabs_content_2" role="tab" aria-controls="nav_tabs_content_2" aria-selected="false">Profile</a>
                    </li>
                </ul>
            </div>
            <div class="s_tabs_content tab-content">
                <div class="tab-pane fade show active" id="nav_tabs_content_1" role="tabpanel" aria-labelledby="nav_tabs_link_1">
                    <div class="oe_structure oe_empty">
                        <section class="s_text_block" data-name="Text">
                            <div class="container s_allow_columns">
                                <p class="o_default_snippet_text">Content 1</p>
                            </div>
                        </section>
                    </div>
                </div>
                <div class="tab-pane fade" id="nav_tabs_content_2" role="tabpanel" aria-labelledby="nav_tabs_link_2">
                    <div class="oe_structure oe_empty">
                         <!-- Content 2 -->
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

#### Table of Content (`s_table_of_content`)
左側/上方導航，右側/下方內容。內容區塊中的 `h2` 會自動生成錨點連結。
*垂直樣式 (Vertical)*:
```xml
<section class="s_table_of_content pt24 pb24 o_cc o_cc1 o_colored_level" data-snippet="s_table_of_content" data-name="Table of Content">
    <div class="container">
        <div class="row s_nb_column_fixed">
            <div class="col-lg-3 s_table_of_content_navbar_wrap s_table_of_content_navbar_sticky s_table_of_content_vertical_navbar d-print-none d-none d-lg-block o_not_editable o_cc o_cc1 o_colored_level" data-name="Navbar">
                <div class="s_table_of_content_navbar list-group o_no_link_popover">
                    <a href="#toc_link_1" class="table_of_content_link list-group-item list-group-item-action py-2 border-0 rounded-0">Chapter 1</a>
                </div>
            </div>
            <div class="col-lg-9 s_table_of_content_main oe_structure oe_empty o_colored_level" data-name="Content">
                <section class="s_text_block" data-name="Text">
                    <div class="s_allow_columns container">
                        <h2 id="toc_link_1" data-anchor="true">Chapter 1</h2>
                        <p>Content...</p>
                    </div>
                </section>
            </div>
        </div>
    </div>
</section>
```

#### Accordion (`s_faq_collapse`)
QA 問答或收合內容。支援多種樣式 (Default, Light, Boxed 等)。
```xml
<section class="s_faq_collapse pt32 pb32 o_colored_level" data-snippet="s_faq_collapse" data-name="Accordion">
    <div class="container">
        <div id="myCollapse_Unique" class="accordion" role="tablist">
            <div class="card bg-white" data-name="Item">
                <a href="#" role="tab" data-toggle="collapse" aria-expanded="true" class="card-header o_default_snippet_text" data-target="#myCollapseTab_1">Question 1</a>
                <div class="collapse show" data-parent="#myCollapse_Unique" role="tabpanel" id="myCollapseTab_1">
                    <div class="card-body">
                        <p class="card-text o_default_snippet_text">Answer 1</p>
                    </div>
                </div>
            </div>
            <!-- More Items -->
        </div>
    </div>
</section>
```

### F. 行銷與商務 (Marketing & Commerce)

#### Call to Action (`s_call_to_action`)
行動呼籲區塊。
```xml
<section class="s_call_to_action o_cc o_cc3 pt48 pb24 o_colored_level" data-snippet="s_call_to_action" data-name="Call to Action">
    <div class="container">
        <div class="row">
            <div class="col-lg-9 pb16 o_colored_level">
                <h3 class="o_default_snippet_text">Join us and make your company a better place.</h3>
            </div>
            <div class="col-lg-3 pt8 o_colored_level">
                <p style="text-align: right;">
                    <a class="btn btn-primary btn-lg mb-2 o_default_snippet_text" href="#">Contact us</a>
                </p>
            </div>
        </div>
    </div>
</section>
```

#### Comparisons (`s_comparisons`)
價格或功能比較表。
```xml
<section class="s_comparisons pt32 pb32 o_colored_level" data-snippet="s_comparisons" data-name="Comparisons">
    <div class="container">
        <div class="row">
            <div class="col-lg-4 s_col_no_bgcolor text-center pt32 pb16" data-name="Box">
                <div class="card bg-200">
                    <h4 class="card-header o_default_snippet_text">Beginner</h4>
                    <div class="card-body text-center">
                        <h2 class="card-title"><span class="s_comparisons_currency">$</span><span class="s_comparisons_price">35</span><small>/ month</small></h2>
                    </div>
                    <ul class="list-group list-group-flush">
                        <li class="list-group-item">Feature 1</li>
                    </ul>
                    <div class="card-footer">
                        <a href="#" class="btn btn-primary mb-2">Order now</a>
                    </div>
                </div>
            </div>
            <!-- More Columns -->
        </div>
    </div>
</section>
```

#### Company Team (`s_company_team`)
團隊成員介紹。
```xml
<section class="s_company_team pt48 pb48 o_colored_level" data-snippet="s_company_team" data-name="Team">
    <div class="container">
        <div class="row s_nb_column_fixed">
            <div class="col-lg-6 pt24 pb24 o_colored_level">
                <div class="row s_col_no_resize s_col_no_bgcolor">
                    <div class="col-lg-4 pb16 o_not_editable">
                        <img src="/web/image/website.s_company_team_image_1" class="img-fluid rounded-circle mx-auto" alt="">
                    </div>
                    <div class="col-lg-8">
                        <h4>Tony Fred, CEO</h4>
                        <p>Founder and chief visionary.</p>
                    </div>
                </div>
            </div>
            <!-- More Members -->
        </div>
    </div>
</section>
```

#### References (`s_references`)
客戶/合作夥伴 Logo 列表。
```xml
<section class="s_references pt48 pb32 o_colored_level" data-snippet="s_references" data-name="References">
    <div class="container">
        <h2 style="text-align: center;">Our References</h2>
        <div class="row">
            <div class="col-lg-2 pt16 pb16 o_colored_level">
                <img src="/web/image/website.s_reference_demo_image_1" class="img img-fluid mx-auto" alt="">
            </div>
            <!-- More Logos -->
        </div>
    </div>
</section>
```

#### Features Grid (`s_features_grid`)
網格狀功能特點。
```xml
<section class="s_features_grid pt48 pb24 o_colored_level" data-snippet="s_features_grid" data-name="Features Grid">
    <div class="container">
        <div class="row">
            <div class="col-lg-6 s_col_no_bgcolor pb24">
                <div class="row">
                    <div class="col-lg-12 pb24 o_colored_level" data-name="Box">
                        <h2>List of Features</h2>
                    </div>
                    <div class="col-lg-12 pt16 pb16 o_colored_level" data-name="Box">
                        <i class="fa fa-2x fa-font-awesome rounded-circle bg-primary s_features_grid_icon"></i>
                        <div class="s_features_grid_content">
                            <h4>Change Icons</h4>
                            <p>Description</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

#### Process Steps (`s_process_steps`)
步驟流程圖。
```xml
<section class="s_process_steps pt24 pb24 o_colored_level" data-snippet="s_process_steps" data-name="Steps">
    <div class="container">
        <div class="row no-gutters">
            <div class="col-lg-3 s_process_step pt24 pb24 o_colored_level">
                <div class="s_process_step_icon">
                    <i class="fa fa-shopping-basket fa-2x mx-auto rounded-circle bg-primary"></i>
                </div>
                <div class="s_process_step_content text-center">
                    <h2>Add to cart</h2>
                    <p>Description</p>
                </div>
            </div>
            <!-- More Steps -->
        </div>
    </div>
</section>
```

#### Timeline (`s_timeline`)
時間軸。
```xml
<section class="s_timeline pt24 pb48 o_colored_level" data-snippet="s_timeline" data-name="Timeline">
    <div class="container s_timeline_line">
        <div class="s_timeline_row d-block d-md-flex flex-row" data-name="Row">
            <div class="s_timeline_date"><span class="bg-white"><b>2019</b></span></div>
            <div class="s_timeline_content d-flex">
                <div class="s_timeline_card s_card card bg-white w-100" data-name="Card">
                    <div class="card-body">
                        <h5 class="card-title">Title</h5>
                        <p class="card-text">Content</p>
                    </div>
                </div>
                <i class="fa fa-1x fa-child bg-secondary rounded-circle s_timeline_icon"></i>
            </div>
        </div>
        <!-- More Rows -->
    </div>
</section>
```

#### Product Catalog (`s_product_catalog`)
產品目錄/菜單。
```xml
<section class="s_product_catalog oe_custom_bg pt48 pb32 oe_img_bg o_bg_img_center o_colored_level" data-snippet="s_product_catalog" data-name="Pricelist">
    <div class="container">
        <h2>Menu</h2>
        <div class="row">
            <div class="col-lg-6 pt16 pb16 o_colored_level">
                <h3>Starter</h3>
                <ul class="list-unstyled my-3">
                    <li class="s_product_catalog_dish">
                        <p class="s_product_catalog_dish_title d-flex align-items-baseline pb-2">
                            <span class="s_product_catalog_dish_name">Dish Name</span>
                            <span class="s_product_catalog_dish_price ml-auto pl-2">$9.00</span>
                        </p>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</section>
```

#### Product List (`s_product_list`)
產品列表。
```xml
<section class="s_product_list o_colored_level" data-snippet="s_product_list" data-name="Items">
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-4 col-6 o_colored_level col-lg-3">
                <a href="#"><img src="/web/image/website.s_product_list_default_image_1" class="img img-fluid"></a>
                <div class="s_product_list_item_link">
                    <a class="btn btn-primary" href="#">Furniture</a>
                </div>
            </div>
        </div>
    </div>
</section>
```

---

## 4. 功能性區塊 (Functional Snippets)

### 聯絡表單 (Contact Form) - STRICT
*   **規範**: 必須嚴格遵守以下 class 命名與結構，否則後台無法抓取資料。
*   **檔案來源**: `addons2/addon_gtmc_website/views/ui_components.xml`

```xml
<form>
    <!-- Text Input Example -->
    <div class="form-group">
        <label for="unique_id_1">Label Text</label>
        <input type="text" class="form-control s_website_form_input" id="unique_id_1" placeholder="Placeholder"/>
        <div class="s_website_form_field_description small form-text text-muted">Field Description</div>
    </div>
    <!-- Select Example -->
    <div class="form-group">
        <label for="unique_id_2">Select Label</label>
        <select class="form-control s_website_form_input" id="unique_id_2">
            <option>Option 1</option>
            <option>Option 2</option>
        </select>
    </div>
    <!-- Checkbox Example -->
    <div class="form-group">
        <label>Checkbox Group</label>
        <div class="row s_col_no_resize s_col_no_bgcolor s_website_form_multiple" data-display="horizontal">
            <div class="checkbox col-12 col-lg-4 col-md-6">
                <div class="form-check">
                    <input class="form-check-input" type="checkbox" value="" id="check1"/>
                    <label class="form-check-label" for="check1">Option 1</label>
                </div>
            </div>
            <div class="checkbox col-12 col-lg-4 col-md-6">
                <div class="form-check">
                   <input class="form-check-input" type="checkbox" value="" id="check2"/>
                   <label class="form-check-label" for="check2">Option 2</label>
                </div>
            </div>
        </div>
    </div>
    <!-- Radio Button Example -->
    <div class="form-group">
         <label>Radio Group</label>
         <div class="row s_col_no_resize s_col_no_bgcolor s_website_form_multiple" data-display="horizontal">
            <div class="checkbox col-12 col-lg-4 col-md-6">
                <div class="form-check">
                    <input class="form-check-input" type="radio" name="radioGroup" id="radio1" value="r1"/>
                    <label class="form-check-label" for="radio1">Radio 1</label>
                </div>
            </div>
            <!-- More options -->
         </div>
    </div>
</form>
```

### G. Static Features & Cards

#### 1. Static Snippet (`s_static_snippet`)
A versatile container for static content layouts.

**Base Structure:**
```xml
<section data-snippet="s_static_snippet" class="s_static_snippet" data-name="Static Snippet">
    <div class="container">
        <div class="static_snippet_template [TEMPLATE_CLASS]">
            <div class="row">
                <!-- Content -->
            </div>
        </div>
    </div>
</section>
```

#### 2. Static Carousel (`s_static_carousel`)
A carousel version of the static content layouts.

**Base Structure:**
```xml
<section data-snippet="s_static_carousel" class="s_static_carousel" data-name="Static Carousel">
    <div class="container">
        <div class="s_carousel carousel slide static_snippet_template [TEMPLATE_CLASS]" data-ride="carousel">
            <div class="carousel-inner">
                <div class="carousel-item active">
                    <div class="static_snippet_template [TEMPLATE_CLASS]">
                        <!-- Content -->
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

#### 3. Available Templates
The following classes function as `[TEMPLATE_CLASS]` in the structures above.

*   **Borderless Card 1** (`s_product_product_borderless_1`): Simple card with image, title, text, and button. No default border.
*   **Borderless Card 2** (`s_product_product_borderless_2`): Similar to n°1 but typically used with different image/text ratios.
*   **Centered Card** (`s_product_product_centered`): Content aligned to the center.
*   **Classic Card** (`s_product_product_classic`): Standard card with border and footer actions.
*   **Horizontal Card** (`s_product_product_horizontal`): Image on the left (or right), text on the side.
*   **Large Banner** (`s_product_product_large_banner`): Full-width or large banner style with background image capabilities.
*   **Big Picture** (`s_blog_post_big_picture`): Large image background with text overlay.
*   **Card Layout** (`s_blog_post_card` / `s_product_product_card`): Standard blog/product card with shadow and footer info.
*   **List Layout** (`s_blog_post_list` / `s_product_product_list`): Small thumbnail on left, text on right, list style.

---

## 5. 設計與變數規範 (Design Guidelines)

### A. CSS Framework
*   **框架**: **Bootstrap 4.5**
*   **常用 Class**: `d-flex`, `align-items-center`, `justify-content-center`, `p-4`, `m-3`, `text-center`, `img-fluid`.

### B. Icons
*   **版本**: **FontAwesome v4**
*   **語法**: `<i class="fa fa-user"></i>`
*   **禁止**: 禁止使用 v5/v6 的 `<i class="fas fa-user"></i>` (除非確定該環境已升級 FA)。

### C. Button Styles
> [!NOTE]
> **Advanced Styles**: For custom button styles (Skew, Arrows, Flash, etc.), strictly refer to [ODOO_BUTTON_STYLES.md](file:///d:/gemini/odoo/docs/ODOO_BUTTON_STYLES.md). You MUST use the SCSS provided there.

*   **Standard**: `btn-primary`, `btn-secondary`
*   **Outline**: `btn-outline-primary`, `btn-outline-secondary`
*   **Fill**: `btn-fill-primary`, `btn-fill-secondary` (Solid fill with specific hover effects)
*   **Shapes**:
    *   `rounded-circle`: Fully rounded buttons.
    *   `flat`: Sharp corners (no border-radius).
*   **Sizes**: `btn-sm`, `btn-lg`

### D. Utility Classes (Colors)
*   User-defined theme colors: `bg-o-color-1` to `bg-o-color-5`, `text-o-color-1` to `text-o-color-5`.
*   Bootstrap defaults: `bg-primary`, `bg-light`, `text-muted`, etc.

### E. 圖片來源
*   **Placeholder**: `https://picsum.photos/` (e.g., `<img src="https://picsum.photos/800/600" class="img-fluid">`)
*   **Odoo Defaults**: `/web/image/website.s_banner_default_image` 等。

### D. SCSS 生成
*   SCSS 應包含在回應中，並建議使用者貼入 Odoo 的 CSS/SCSS 編輯器。

### E. Theme Color Presets (o_cc classes)
Odoo uses a color class system to apply predefined color palettes/combinations to sections. These are crucial for maintaining consistent theming across the site. Use these classes on the `<section>` or container elements.

| Class | Description | Appearance (Typical) |
| :--- | :--- | :--- |
| **o_cc1** | **Default / Light** | White background, Dark text. |
| **o_cc2** | **Secondary / Light Gray** | Light gray background, Dark text. |
| **o_cc3** | **Primary Color Focus** | Primary color background (e.g., Green), White text. |
| **o_cc4** | **Secondary Color Focus** | Secondary color background (e.g., Dark Green), White text. |
| **o_cc5** | **Dark Mode / Footer** | Dark/Black background, White text. |

**Usage Example:**
```xml
<section class="s_text_block pt40 pb40 o_colored_level o_cc o_cc3" data-snippet="s_text_block">
    <!-- This section will have a primary color background (e.g. Green) and white text -->
</section>
```

### F. Custom Styling & Attributes
To allow the Odoo frontend editor to properly recognize and apply custom styles, a strict naming convention must be followed.

1.  **CSS Class**: Custom styles are applied as classes prefixed with `s_custom_`.
2.  **Data Attribute**: The key part of the class name must be mirrored in the `data-custom-name` attribute.

**Example:**
To apply a large scaling effect:
-   Class: `s_custom_scaleL`
-   Attribute: `data-custom-name="scaleL"`

**Combined Usage:**
```xml
<section class="... s_custom_scaleL s_custom_borderHover" data-custom-name="scaleL borderHover">
```

**Common Custom Classes:**
*   `s_custom_scaleL`: Large scale effect on hover
*   `s_custom_scaleS`: Small scale effect on hover
*   `s_custom_borderHover`: Border appears on hover
*   `s_custom_imgNoPadding`: Removes padding from images
*   `s_custom_imgNoMargin`: Removes margin from images
*   `s_custom_GapM`: Medium gap between elements
*   `s_custom_nameHoverUnderLine`: Underline text on hover

---

## 6. Dynamic & Interactive Snippets

### 6.1 Website Form (`s_website_form`)
A fully functional contact form integrated with Odoo's backend (e.g., creating CRM leads or sending emails).

**Attributes:**
*   `data-model_name`: The Odoo model to create (e.g., `mail.mail`, `crm.lead`).
*   `data-success-mode`: Action after submission (e.g., `redirect`).
*   `data-success-page`: URL to redirect to.

**Field Types:**
*   **Hidden**: `<input type="hidden" name="email_to" value="...">` (e.g., for recipients).
*   **Char**: `data-type="char"` for single line text.
*   **Email**: `data-type="email"`.
*   **Tel**: `data-type="tel"`.
*   **Text (Textarea)**: `data-type="text"` for multi-line content.
*   **Boolean/Checkbox**: `data-type="boolean"`.

**Structure:**
```xml
<section class="s_website_form pt16 pb16" data-snippet="s_website_form" data-name="Form">
  <div class="container">
    <form action="/website/form/" method="post" enctype="multipart/form-data" class="o_mark_required" data-model_name="mail.mail">
      <div class="s_website_form_rows row s_col_no_bgcolor">
        
        <!-- Hidden Field (e.g. Recipient) -->
        <div class="form-group s_website_form_field col-12 s_website_form_dnone" data-name="Field">
          <div class="row s_col_no_resize s_col_no_bgcolor">
             <div class="col-sm">
               <input type="hidden" class="form-control s_website_form_input" name="email_to" value="info@example.com"/>
             </div>
          </div>
        </div>

        <!-- Visible Field -->
        <div class="form-group s_website_form_field col-12 s_website_form_custom s_website_form_required" data-type="char" data-name="Field">
          <div class="row s_col_no_resize s_col_no_bgcolor">
            <label class="col-form-label col-sm-auto s_website_form_label" style="width: 200px">
              <span class="s_website_form_label_content">Name</span>
              <span class="s_website_form_mark"> *</span>
            </label>
            <div class="col-sm">
              <input type="text" class="form-control s_website_form_input" name="name" required="1"/>
            </div>
          </div>
        </div>

        <!-- Submit Button -->
        <div class="form-group col-12 s_website_form_submit" data-name="Submit Button">
          <div style="width: 200px;" class="s_website_form_label"/>
          <a href="#" role="button" class="btn btn-primary btn-lg s_website_form_send">Submit</a>
          <span id="s_website_form_result"/>
        </div>

      </div>
    </form>
  </div>
</section>
```

### 6.2 Breadcrumb (`s_breadcrumb`)
Displays the current page path.

**Structure:**
```xml
<section class="s_text_block" data-snippet="s_text_block">
  <div class="container">
    <div data-snippet="s_breadcrumb" class="s_breadcrumb d-flex" data-name="Breadcrumb">
      <ol class="breadcrumb my-2 bg-transparent">
        <li class="breadcrumb-item"><a href="/">Home</a></li>
        <li class="breadcrumb-item active">Current Page</li>
      </ol>
    </div>
  </div>
</section>
```

### 6.3 Google Map (`s_map`)
Embeds a Google Map based on an address.

**Attributes:**
*   `data-map-address`: The address to display (e.g., "Taiwan").
*   `data-map-zoom`: Zoom level (e.g., "12").
*   `data-map-type`: Map type (e.g., "m" for roadmap).

**Structure:**
```xml
<section class="s_map pb168 pt152" data-map-address="Taiwan" data-map-zoom="12" data-snippet="s_map" data-name="Map">
  <div class="map_container">
    <iframe class="s_map_embedded" width="100%" height="100%" src="https://maps.google.com/maps?q=Taiwan&amp;t=m&amp;z=12&amp;output=embed" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe>
    <div class="s_map_color_filter"></div>
  </div>
</section>
```

### 6.4 Search Bar (`s_searchbar`)
A search input that redirects to the search results page.

**Attributes:**
*   `data-limit`: Max results to show in autocomplete.
*   `data-display-image`: Show product images in results.
*   `data-search-type`: Scope of search (e.g., "all", "products").

**Structure:**
```xml
<section class="s_searchbar o_colored_level" data-snippet="s_searchbar" data-name="Search">
  <div class="container">
    <form method="get" action="/website/search" class="o_searchbar_form s_searchbar_input">
      <div role="search" class="input-group">
        <input type="search" name="search" class="search-query form-control" placeholder="Search..." data-limit="5"/>
        <div class="input-group-append">
          <button type="submit" class="btn btn-primary"><i class="fa fa-search"></i></button>
        </div>
      </div>
    </form>
  </div>
</section>
```

### 6.5 Counter / Stats (`s_counting`)
Animated number counter.

**Attributes:**
*   `data-count-from`: Start number.
*   `data-count-to`: End number.
*   `data-count-before-text`: Prefix symbol (e.g., "$").
*   `data-count-after-text`: Suffix symbol (e.g., "%", "Years").

**Structure:**
```xml
<div class="s_counting c_counting" data-count-from="0" data-count-to="100" data-snippet="s_counting" data-name="Counting">
  <span class="counter-before"></span>
  <span class="counter display-4 font-weight-normal">0</span>
  <span class="counter-after">%</span>
</div>
<p>Description Text</p>
```

### 6.6 Countdown Timer (`s_countdown`)
Counts down to a specific date/time.

**Attributes:**
*   `data-end-time`: Timestamp (Unix epoch).
*   `data-display`: Format (e.g., "dhms" for Days/Hours/Min/Sec).
*   `data-layout`: Visual style (e.g., "circle").

**Structure:**
```xml
<section class="s_countdown pt48 pb48" data-snippet="s_countdown" data-end-time="1770329359" data-display="dhms" data-layout="circle" data-name="Countdown">
  <div class="container">
    <div class="s_countdown_canvas_wrapper text-center d-flex justify-content-center"></div>
  </div>
</section>
```

### 6.7 Embed Code (`s_embed_code`)
Container for raw HTML, JS, or iframe content.

**Structure:**
```xml
<section class="s_embed_code text-center pt64 pb64" data-snippet="s_embed_code" data-name="Embed Code">
  <div class="s_embed_code_embedded container">
    <div class="text-monospace pt8 bg-light">
       <!-- Your Custom HTML Code Here -->
    </div>
  </div>
</section>
```

### 6.8 Dynamic Products (`s_dynamic_snippet_products`)
Displays a grid of products based on a filter or category. The internal content is rendered by the server.

**Key Attributes:**
*   `data-snippet="s_dynamic_snippet_products"`
*   `data-template-key`: Specifies the layout template (see below).
*   `data-product-category-id`: Filter by category ID (e.g., "all", "1").
*   `data-number-of-elements`: Items to show.
*   `data-carousel-interval`: Auto-rotate interval (if applicable).

**Structure:**
```xml
<section data-snippet="s_dynamic_snippet_products" class="s_dynamic_snippet_products s_dynamic o_colored_level [TEMPLATE_CLASS]" data-name="Products" data-template-key="[TEMPLATE_KEY]" data-product-category-id="all">
  <div class="o_not_editable container">
    <div class="dynamic_snippet_template">
       <!-- Server Rendered Content -->
    </div>
  </div>
</section>
```

### 6.9 Dynamic Carousel (`s_dynamic_snippet_carousel`)
Displays products in a carousel format.

**Key Attributes:**
*   `data-snippet="s_dynamic_snippet_carousel"`
*   `data-template-key`: Specifies the layout template.
*   `data-custom-name`: Syncs with `s_custom_` classes.

**Structure:**
```xml
<section data-snippet="s_dynamic_snippet_carousel" class="s_dynamic_snippet_carousel s_dynamic o_colored_level [TEMPLATE_CLASS]" data-name="Dynamic Carousel" data-template-key="[TEMPLATE_KEY]">
  <div class="o_not_editable container">
    <div class="dynamic_snippet_template">
       <!-- Server Rendered Content -->
    </div>
  </div>
</section>
```

### 6.10 Available Dynamic Templates
> [!IMPORTANT]
> Please refer to `docs/ODOO_DYNAMIC_RULES.md` for the complete and strict list of valid Product Templates and their associated custom classes.



### 6.11 Dynamic Blogs (`s_dynamic_snippet`)
Displays a list of blog posts based on a filter or category.

**Key Attributes:**
*   `data-snippet="s_dynamic_snippet"`
*   `data-template-key`: Specifies the layout template (see below).
*   `data-filter-id`: Blog filter ID (e.g., "1").
*   `data-number-of-elements`: Items to show.

**Structure:**
```xml
<section data-snippet="s_dynamic_snippet" class="s_dynamic_snippet s_dynamic o_colored_level [TEMPLATE_CLASS]" data-name="Dynamic Snippet" data-template-key="[TEMPLATE_KEY]" data-filter-id="1">
  <div class="o_not_editable container">
    <div class="dynamic_snippet_template">
       <!-- Server Rendered Content -->
    </div>
  </div>
</section>
```

### 6.12 Dynamic Blog Carousel (`s_dynamic_snippet_carousel`)
Displays blog posts in a carousel format. Note: This shares the same snippet name as products but uses different template keys.

**Key Attributes:**
*   `data-snippet="s_dynamic_snippet_carousel"`
*   `data-template-key`: Specifies the layout template.

**Structure:**
```xml
<section data-snippet="s_dynamic_snippet_carousel" class="s_dynamic_snippet_carousel s_dynamic o_colored_level [TEMPLATE_CLASS]" data-name="Dynamic Carousel" data-template-key="[TEMPLATE_KEY]">
  <div class="o_not_editable container">
    <div class="dynamic_snippet_template">
       <!-- Server Rendered Content -->
    </div>
  </div>
</section>
```

### 6.13 Available Blog Templates
> [!IMPORTANT]
> Please refer to `docs/ODOO_DYNAMIC_RULES.md` for the complete and strict list of valid Blog Templates and their associated custom classes.


**Common Custom Attributes (Sync with `s_custom_` classes):**
*   `cardRadius`: 20px Border Radius.
*   `cardRadiusS`: 10px Border Radius.
*   `picTop`: Picture on top.
*   `picBottom`: Picture on bottom.
*   `dateTop`: Date above text.
*   `borderHover`: Border color change on hover.
*   `hoverUnderLine`: Underline appears on hover.
*   `titleLine`: Underline on title (static).
*   `blockLine`: Separator line between blocks.

### 6.14 Separator (`s_hr`)
A horizontal rule to separate content.

**Structure:**
```xml
<div class="s_hr text-left pt16 pb16" data-snippet="s_hr" data-name="Separator">
  <hr class="w-100 mx-auto" style="border-top-width: 1px; border-top-style: solid;"/>
</div>
```

### 6.15 Table of Feature (`s_table_of_feature`)
Displays feature comparison tables.

**Key Attributes:**
*   `data-tablefeature-template`: Template style (e.g., `default_with_multi_value`, `flexible_content`, `flexible_vertical_content`, `flexible_horizontal_content`, `flexible_full_text`).

**Structure:**
```xml
<div class="s_table_of_feature table-responsive" data-vcss="001" data-snippet="s_table_of_feature" data-name="Table of Feature" data-tablefeature-template="flexible_content">
  <table class="table mb-3">
    <!-- Table Content -->
  </table>
</div>
```

### 6.16 Alert (`s_alert`)
Contextual feedback messages for typical user actions.

**Key Attributes:**
*   `class="s_alert s_alert_[size] alert-[type]"` (e.g. `s_alert_md alert-info`).

**Structure:**
```xml
<div class="s_alert s_alert_md alert-info w-100 clearfix" data-snippet="s_alert" data-name="Alert">
  <i class="fa fa-2x fa-info-circle s_alert_icon"/>
  <div class="s_alert_content">
    <p>Message content...</p>
  </div>
</div>
```

### 6.17 Popup (`s_click_popup`)
A modal popup triggered by a click or event. Note: Usually hidden by default.

**Structure:**
```xml
<div class="s_click_popup o_snippet_invisible" data-snippet="s_click_popup" data-name="Click Popup" id="P123">
  <div class="modal s_popup_middle fade" data-backdrop="false">
    <div class="modal-dialog d-flex">
      <div class="modal-content oe_structure">
         <div class="s_popup_close js_close_popup">×</div>
         <!-- Popup Content (e.g. s_banner) -->
      </div>
    </div>
  </div>
</div>
```

### 6.18 Chart (`s_chart`)
Displays a visual chart (bar, pie, etc.). Note: `canvas` is rendered by JS.

**Key Attributes:**
*   `data-type`: Chart type (e.g., "bar").
*   `data-data`: JSON string of chart data.

**Structure:**
```xml
<div class="s_chart" data-type="bar" data-snippet="s_chart" data-name="Chart" data-data="{...}">
  <h2>Chart Title</h2>
  <canvas width="300"/>
</div>
```

### 6.19 Blockquote (`s_blockquote`)
Styled quote block.

**Structure:**
```xml
<blockquote class="s_blockquote s_blockquote_classic w-100 mx-auto o_animable blockquote" data-snippet="s_blockquote" data-name="Blockquote">
  <i class="s_blockquote_icon fa fa-1x fa-quote-left bg-o-color-2 rounded"/>
  <div class="s_blockquote_content bg-100">
     <p>Quote text...</p>
     <footer class="blockquote-footer">
        <img class="s_blockquote_avatar ..." src="..."/>
        <span class="s_blockquote_author">Author Name</span>
     </footer>
  </div>
</blockquote>
```

### 6.20 Progress Bar (`s_progress_bar`)
Displays a progress bar.

**Structure:**
```xml
<div class="s_progress_bar" data-display="inline" data-snippet="s_progress_bar" data-name="Progress Bar">
  <h4>Title</h4>
  <div class="progress">
    <div class="progress-bar progress-bar-striped" role="progressbar" aria-valuenow="50" style="width: 50%;">
      <span class="s_progress_bar_text">50% Content</span>
    </div>
  </div>
</div>
```

### 6.21 Image Gallery (`s_image_gallery`)
Displays a collection of images in a slideshow or grid format.

**Key Attributes:**
*   `data-columns`: Number of columns (e.g., "3").
*   `class="s_image_gallery o_slideshow ..."`: Determines mode (slideshow vs grid).

**Structure (Slideshow Mode):**
```xml
<section class="s_image_gallery o_slideshow s_image_gallery_show_indicators s_image_gallery_indicators_rounded pt24 pb24" data-vcss="001" data-columns="3" style="height: 500px; overflow: hidden;" data-snippet="s_image_gallery" data-name="Image Gallery">
  <div class="container">
    <div id="slideshow_sample" class="carousel slide" data-ride="carousel" data-interval="0" style="margin: 0 12px;">
       <div class="carousel-inner" style="padding: 0;">
         <div class="carousel-item active">
           <img class="img img-fluid d-block" src="/web/image/website.library_image_08" data-name="Image" data-index="0" loading="lazy"/>
         </div>
         <!-- More items -->
       </div>
       <ul class="carousel-indicators">
          <!-- Indicators -->
       </ul>
       <!-- Controls -->
    </div>
  </div>
</section>
```

### 6.22 Images Wall (`s_images_wall`)
Displays images in a masonry-style wall layout.

**Structure:**
```xml
<section class="s_image_gallery o_spc-small o_masonry pt24 pb24" data-vcss="001" data-columns="3" style="overflow: hidden;" data-snippet="s_images_wall" data-name="Images Wall">
  <div class="container">
    <div class="row s_nb_column_fixed">
      <div class="o_masonry_col o_snippet_not_selectable col-lg-4">
        <img class="img img-fluid d-block" src="..." data-index="0" data-name="Image" loading="lazy"/>
      </div>
      <!-- More columns -->
    </div>
  </div>
</section>
```

### 6.23 Showcase (`s_showcase`)
A split layout to showcase features, typically with a central divider or image.

**Structure:**
```xml
<section class="s_showcase pt48 pb48" data-vcss="002" data-snippet="s_showcase" data-name="Showcase">
  <div class="container">
    <div class="row no-gutters s_col_no_resize s_col_no_bgcolor s_nb_column_fixed">
      <div class="col-lg text-lg-right">
         <!-- Left Side Content -->
         <div class="s_showcase_title d-flex flex-lg-row-reverse mb-2">
            <i class="s_showcase_icon fa fa-2x fa-desktop text-secondary mr-3 mr-lg-0 ml-lg-3"/>
            <h3>First feature</h3>
         </div>
         <p>Description...</p>
      </div>
      <div class="col-1">
         <div class="w-50 h-100 border-right"/>
      </div>
      <div class="col-lg">
         <!-- Right Side Content -->
      </div>
    </div>
  </div>
</section>
```

### 6.24 Parallax (`s_parallax`)
A standalone parallax section (often used as a background wrapper).

**Structure:**
```xml
<section class="s_parallax parallax s_parallax_is_fixed bg-black-50 o_half_screen_height" data-scroll-background-ratio="1" data-fade-method="none" data-snippet="s_parallax" data-name="Parallax">
  <span class="s_parallax_bg oe_img_bg" style="background-image: url('/web/image/website.s_parallax_default_image'); background-position: 50% 75%;"/>
  <div class="o_we_bg_filter bg-black-50"/>
  <div class="oe_structure oe_empty"/>
</section>
```

### 6.25 Quotes Carousel (`s_quotes_carousel`)
A carousel specifically designed for testimonials.

**Structure:**
```xml
<section class="s_quotes_carousel_wrapper" data-vxml="001" data-vcss="001" data-snippet="s_quotes_carousel" data-name="Quotes">
  <div class="s_quotes_carousel s_carousel_default carousel slide o_cc o_cc2" data-interval="10000" id="myQuoteCarousel">
     <div class="carousel-inner">
       <div class="carousel-item active oe_img_bg o_bg_img_center pt80 pb80">
         <div class="container">
            <blockquote class="s_blockquote s_blockquote_classic blockquote mx-auto w-50" data-name="Blockquote">
               <!-- Quote Content -->
            </blockquote>
         </div>
       </div>
       <!-- More slides -->
     </div>
  </div>
</section>
```

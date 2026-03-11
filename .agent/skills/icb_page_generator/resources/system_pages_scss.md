# Odoo 14 系統頁面 (Blog / Shop) 開發規範

在 Odoo 中，像「部落格文章列表 (Blog List)」、「部落格內頁 (Blog Post)」、「商品列表 (Shop)」這類頁面，其 HTML 結構是由系統的核心模組與 Controller 在後端動態生成的。

> [!WARNING]
> **絕對禁止修改系統頁面的底層 XML 結構！**
> 如果我們硬改系統底層的 XML，在 Odoo 版本升級或安裝其他相依模組時，非常容易造成系統崩潰（View Conflict）。因此，針對這些系統頁面，**我們唯一且最強大的武器就是 SCSS。**

---

## 1. 核心觀念：純 SCSS 攔截與覆寫

設計師在 Figma 等工具中畫出的漂亮 Blog 卡片或 Shop 網格，我們必須透過 SCSS，強行套用在 Odoo 原本生硬的 HTML 結構上。

在 Odoo 的前台編輯器中，通常會有原生選項允許客戶切換版面（例如「清單視圖」或「網格視圖」）。我們的 SCSS 必須能夠**聰明地偵測並相容這些原生選項**。

---

## 2. Blog 列表頁 (Blog List) 樣式覆寫策略

當我們在 Odoo 建立 Blog 頁面時，通常會有一層 `.o_wblog_page` 或類似的 wrapper。我們需要針對系統預設的文章區塊 `.o_wblog_post` 進行整型。

### 2-1. 卡片化與陰影設計 (Card Design)
Odoo 預設的部落格列表通常只是白底文字。我們把它變成具有現代感的浮雕卡片：

```scss
/* 限定作用範圍，避免污染其他使用到相同 class 的元件 */
.website_blog {
    
    // 針對單篇文章的容器
    article.o_wblog_post {
        border-radius: 15px; // 圓角特效
        overflow: hidden; // 確保圖片圓角不溢出
        background-color: #ffffff;
        box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05); // 質感陰影
        transition: transform 0.3s ease, box-shadow 0.3s ease;

        &:hover {
            transform: translateY(-5px); // 懸停上浮
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); 
        }

        /* 攔截封面圖片 */
        .o_record_cover_container {
            height: 250px !important; // 固定圖片高度
            width: 100%;
            
            // 如果 Odoo 原生不是用 img 而是 background-image
            background-size: cover;
            background-position: center;
        }

        /* 攔截內文區域 */
        .o_wblog_post_content {
            padding: 20px;

            // 標題樣式
            h2 {
                font-size: 1.25rem;
                font-weight: 600;
                margin-bottom: 15px;
                color: var(--o-cc5-text);
            }
            
            // 隱藏不必要的系統資訊 (如某些不需要的標籤或預設日期格式)
            .te_blog_tag_list {
                display: none; // 如果設計不採用
            }
        }
    }
}
```

### 2-2. 網格列表排版 (CSS Grid)
如果 Odoo 吐出來的結構是一個平坦的 `.row` 裡面塞滿 `article`，但我們想要改變它在桌機與手機上的排列方式，可以利用 CSS Grid：

```scss
.website_blog {
    // 假設系統預設吐出一個包著所有文章的容器
    .o_wblog_post_list {
        display: grid;
        gap: 30px; // 卡片間距
        
        // 手機單欄 -> 平板雙欄 -> 桌布三欄
        grid-template-columns: 1fr;
        
        @include media-breakpoint-up(md) {
            grid-template-columns: repeat(2, 1fr);
        }
        
        @include media-breakpoint-up(lg) {
            grid-template-columns: repeat(3, 1fr);
        }

        // 強制取消 Odoo 原本可能加在子元素上的 Bootstrap col 控制
        > article {
            max-width: none !important;
            flex: none !important;
            width: 100% !important;
            margin: 0 !important;
        }
    }
}
```

### 2-3. 部落格內頁 (Blog Post Detail)
當使用者點擊進入單篇文章時，系統會載入詳細頁面。此頁面的主體通常被包覆在 `#o_wblog_post_main` 或 `.o_wblog_post_content` 中。

對於內頁，我們最常需要使用 SCSS 覆寫的項目包含：
- **文章大標題排版**：Odoo 有時會將標題放在背景圖上 (`.o_record_cover_container`)，需調整遮罩或文字顏色。
- **內容寬度限制**：預設的文字寬度可能太寬不利閱讀，我們可限制最大寬度並置中。
- **隱藏多餘資訊**：如果不想顯示發表時間或作者，可將其 `display: none`。

```scss
#o_wblog_post_main {
    // 限制閱讀區塊最大寬度，提升網頁可讀性 (Readability)
    .o_wblog_post_content {
        max-width: 800px;
        margin: 0 auto;
        font-size: 1.1rem;
        line-height: 1.8;
    }
    
    // 隱藏作者頭像或標籤區域 (若設計不要求)
    .o_wblog_author_avatar {
        display: none !important;
    }
}
```

---

## 3. 電商商品列表 (Shop List) 樣式覆寫策略

電商頁面 (`/shop`) 是 Odoo 中最複雜的模組之一，包含了左側過濾器 (Sidebar)、頂部排序搜尋、以及商品網格。

### 3-1. 佈局重構 (過濾器與商品區的比例)
Odoo 原生的左側 Sidebar (`#wrapwrap .o_wsale_products_main_row`) 佔據比例有時不符合現代電商。我們可以調整比例：

```scss
.o_wsale_products_main_row {
    // 確保 RWD 時 Sidebar 不會壞掉
    @include media-breakpoint-up(lg) {
        display: grid;
        grid-template-columns: 280px 1fr; // 固定 Sidebar 寬度，其餘給商品
        gap: 40px;

        // 移除 Bootstrap 的彈性盒子干擾
        > #products_grid_before { // 左側 Filter
            flex: none;
            max-width: none;
            width: 100%;
        }
        > #products_grid {        // 右側商品清單
            flex: none;
            max-width: none;
            width: 100%;
        }
    }
}
```

### 3-2. 商品卡片重塑 (Product Card)
Odoo 的商品卡片通常包在 `.oe_product` 或 `.o_wsale_product_grid_wrapper` 內。

```scss
#products_grid {
    .oe_product {
        border: none !important;
        border-radius: 8px;
        background: #fafafa;
        overflow: hidden;
        transition: 0.3s;

        &:hover {
            box-shadow: 0 10px 20px rgba(0,0,0,0.08);
            
            // Hover 時讓加入購物車按鈕浮現
            .o_wsale_product_btn {
                opacity: 1;
                transform: translateY(0);
            }
        }

        // 隱藏預設的商品圖片外框
        .oe_product_card {
            border: none;
        }

        // 修改價格顏色
        .product_price {
            color: var(--o-cc3-bg); // 使用主色系強調價格
            font-size: 1.2rem;
            font-weight: bold;
        }
    }
}
```

### 3-3. 分類清單樣式 (Category List)
在 Shop 頁面的左方側邊欄，Odoo 會產生一個樹狀的分類清單 (`.o_wsale_category_list` 或 `#wsale_products_categories_collapse`)。通常系統預設的 `<ul> <li>` 樣式非常單調。

```scss
#wsale_products_categories_collapse {
    .nav-item {
        border-bottom: 1px solid #eee;
        
        .nav-link {
            color: var(--o-cc1-text);
            padding: 10px 15px;
            font-weight: 500;
            transition: all 0.2s;
            
            &:hover, &.active {
                background-color: var(--o-cc3-bg);
                color: #fff;
                padding-left: 20px; // Hover 時文字微推特效
            }
        }
        
        // 針對有子分類的展開樣式
        i.fa-chevron-right, i.fa-chevron-down {
            float: right;
            margin-top: 5px;
            font-size: 0.8em;
        }
    }
}
```

### 3-4. 單一商品內頁 (Product Detail)
使用者點進單個商品後，頁面主體是 `#product_detail`。最常見的覆寫需求是將「圖片畫廊 (Gallery)」與「右側結帳/參數區塊」的比例與位置變得更現代化。

```scss
#product_detail {
    // 拉寬主要容器的間距
    .row {
        @include media-breakpoint-up(lg) {
            gap: 50px;
        }
    }

    // 處理商品標題與售價
    h1[itemprop="name"] {
        font-size: 2.5rem;
        font-weight: bold;
        color: var(--o-cc4-text);
        margin-bottom: 20px;
    }

    .oe_price_h4 {
        font-size: 1.8rem;
        color: var(--o-cc3-bg);
    }

    // 將加入購物車按鈕放大並圓角化
    #add_to_cart {
        border-radius: 50px;
        padding: 15px 40px;
        font-size: 1.2rem;
        text-transform: uppercase;
        letter-spacing: 1px;
    }
    
    // 如果 Odoo 原生規格表格 (Attributes table) 太醜，覆寫表單樣式
    table.o_wsale_product_attribute {
        td, th {
            border-top: none;
            border-bottom: 1px solid #ddd;
            padding: 15px 10px;
        }
    }
}
```

### 3-5. 加入購物車 (詢價車) 飛行動畫定位修復
在 Shop 點擊「加入購物車」時，Odoo 原生的 JavaScript 會有一個商品圖片飛向導覽列購物車圖示 (`.o_wsale_quote_cart`) 的動畫。

當網站在手機版 (Mobile) 時，如果主導覽選單被收合 (`#top_menu_collapse`)，或者我們有設計獨立的手機版 Header 雙排選單，這個飛行動畫會找不到目標，或是飛向奇怪的隱藏區塊。

**解法**：我們無法去改 Odoo 的 JS，但我們可以透過 SCSS，**把隱藏在 Collapse 裡面的原生 `.o_wsale_quote_cart` 強制設為 `display: block !important` 與 `position: absolute`，然後定位到「視覺上」的手機版購物車圖示正上方**，並將透明度設為 0 (或 `pointer-events: none`) 讓它作為純粹的「動畫降落靶心」。

```scss
// 針對未展開的標頭 (Mobile 漢堡選單未點開時)
header:not(.o_top_menu_collapse_shown) {
    @include media-breakpoint-down(md) {
        height: unset !important;

        // 我們不希望整個下拉選單跑出來，所以強行設定高度為 0
        &#top #top_menu_collapse {
            height: 0 !important;
            padding: 0 !important;
            display: block !important;
            margin: 0 !important;
            opacity: 0 !important;
            pointer-events: none;
        }

        // 唯獨把這個「飛行動畫的靶心 (cart)」叫出來，放到畫面上正確的購物車圖示上
        &#top #top_menu_collapse #top_menu .o_wsale_quote_cart {
            display: block !important;
            position: absolute;
            // 以下數值必須根據實際的手機版 Header 高度與按鈕位置自行調整
            top: 15px; 
            right: 140px; 
            width: auto !important;
            pointer-events: none; // 不要阻擋點擊可以考慮加 opacity: 0
        }
    }
}
```

### 3-6. 購物車/詢價車 Checkout 頁面 (`/shop/cart` 或 `/quote/cart`)
當使用者點擊購物車進入結帳或詢價清單頁面時，系統會載入一個包含商品表格 (`#cart_products`) 與右側/下方結帳流程卡片的版面。

這個頁面的預設 Bootstrap 表格通常在手機版會發生嚴重跑版（水平卷軸或破版）。我們必須透過 SCSS 強制讓表格變成 RWD 卡片，或是隱藏不必要的欄位（例如稅金、折扣）。

```scss
// 針對結帳清單主區塊
.oe_cart {
    // 預設的購物車表格
    #cart_products {
        border-collapse: separate;
        border-spacing: 0 15px; // 讓每一列表格之間有空隙，呈現卡片感
        
        thead {
            // 在某些設計中，我們會隱藏標題列，讓畫面更乾淨
            display: none; 
        }

        tbody tr {
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            border-radius: 10px;
            
            td {
                border-top: none; // 移除預設 Bootstrap 表格線
                vertical-align: middle;
            }
            
            // 手機版：強制將表格轉為 Block 堆疊，避免水平卷軸
            @include media-breakpoint-down(sm) {
                display: flex;
                flex-wrap: wrap;
                position: relative;
                padding: 15px;
                
                td {
                    display: block;
                    width: 100%;
                    text-align: left !important;
                    
                    // 針對刪除按鈕 (Trash icon) 絕對定位到右上角
                    &.td-action {
                        position: absolute;
                        top: 10px;
                        right: 10px;
                        width: auto;
                    }
                }
            }
        }
    }
    
    // 下一步/結帳 按鈕美化
    .btn-primary.float-right {
        border-radius: 30px;
        padding: 12px 35px;
        font-weight: bold;
        letter-spacing: 1px;
    }
}
```

---

## 4. 開發流程摘要
1. **進入開發者模式 (Inspector)**：打開 Odoo 前台頁面，按 F12 觀察系統吐出了哪些 ID 與 Class（例如 `.o_wblog_page`, `#products_grid`）。
2. **尋找最外層容器**：在 SCSS 檔案中，先寫下最外層的 Wrapper，確保樣式只影響該系統頁面。
3. **取消預設結構 (Unset/Override)**：把不想要的 Bootstrap grid 屬性（如 `flex`, `max-width`, `margin`）設為 `none` 或 `0`。
4. **注入現代化 CSS**：改用 CSS Grid 或 Flexbox 重新排版，並套用陰影、圓角、懸停動畫等設計。

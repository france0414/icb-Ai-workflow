---
description: Odoo 客製化 SCSS 變數與工具類別 (Utility Classes) 速查表
---

# Odoo SCSS 客製化工具庫 (user_custom_rules.scss)

這份文件記錄了專案核心設計檔 `user_custom_rules.scss` 內所提供的強大自訂 Class 與排版工具。
AI 在生成 Odoo 程式碼時，**絕對不要手寫獨立的 Inline CSS 或另起獨特的 Class**，而應優先使用以下預設好的輔助類別，這能確保網頁在不同專案的 RWD 行為、斷點與主題色皆能保持一致。

---

## 1. 響應式內距系統 (Responsive Padding System)
這套系統會自動隨螢幕大小（電腦、平板、手機）等比例縮小間距，**請取代 Bootstrap 原生的 `pt-*`, `pb-*`**。
*   **使用方式**：`.pt{數值}` 或 `.pb{數值}`，數值為 8 的倍數 (0 到 256)
*   **範例**：`.pt32`, `.pb64`, `.pt128`, `.pb256`
*   **RWD 保護**：在電腦版 `pt64` 代表 `64px`，但到了手機板會自動等比例縮小 (例如變成 `40px`)，確保手機版不會留白過多。

## 2. 獨立推擠空白區塊 (Space Blocks)
當區塊間需要嚴格的間隙，且不依賴 Padding 時使用：
*   `.inner-Space` / `.s_custom_Space`：大間隔（預設 100px，手機 40px）
*   `.inner-SpaceM` / `.s_custom_SpaceM`：中間隔（預設 70px，手機 25px）
*   `.inner-SpaceS` / `.s_custom_SpaceS`：小間隔（預設 50px，手機 20px）

## 3. 容器與網格輔助 (Grid & Container Utilities)
*   `.s_custom_fullContainer`：取消容器兩側的安全距離（Padding），讓內容完全貼邊（適用於滿版 Banner 或橫跨畫面的設計）。
*   `.s_custom_noGap`：套用於含有 `.row` 的外層，使所有欄（col）之間的間隙歸零。
*   `.s_custom_GapM`：套用於含有 `.row` 的外層，欄間隙改為中等（10px padding）。
*   `.s_custom_GapS`：套用於含有 `.row` 的外層，欄間隙改為極小（5px padding）。
*   `.s_custom_reverse`：套用後會使 `.row` 降級變為 `flex-direction: row-reverse;` (左右反轉排列)。

## 4. 動態區塊與卡片美化 (Cards & Dynamic Snippets)
在 `.s_dynamic_snippet_products`, `.s_blog_post_card` 或標準卡片中，組合以下 Class 創造豐富變化：
*   **.s_custom_cardRadius**：卡片變為大圓角 (20px)。
*   **.s_custom_cardRadiusS**：卡片變為小圓角 (10px)。
*   **.s_custom_imgNoMargin**：取消卡片內圖片（Image/Cover）周圍的 Padding 配置，讓圖片長寬撐滿。
*   **.s_custom_borderHover**：滑鼠移過（Hover）時顯示 1px 主題色框線。
*   **.s_custom_scaleL**：設定圖片在滑鼠 Hover 時「放大 1.05 倍」。
*   **.s_custom_scaleS**：設定圖片在滑鼠 Hover 時「縮小 0.95 倍」。
*   **.s_custom_nameHoverUnderLine**：滑鼠 Hover 卡片時，卡片標題下方長出主題色動態底線。
*   **.s_custom_picTop** / **.s_custom_picBottom** / **.s_custom_dateTop**：直接以 Flex 改變原本圖、文、日期的上下排列順序。
*   **.s_custom_noPic**：強制隱藏卡片文章縮圖。

## 5. 輪播箭頭徹底魔改 (Carousel Arrows)
不須寫死 CSS，只需在 `.s_dynamic` 或 `.s_carousel` 加上這些類別即可改變箭頭造型：
*   **位置控制**：`.s_custom_arrowTop` (放右上/左上), `.s_custom_arrowBottom`, `.s_custom_arrowLeft`, `.s_custom_arrowRight`。
*   **排列與留白**：`.s_custom_arrowNoGap` (兩箭頭相黏), `.s_custom_arrowNoSeparate` (箭頭與內容不留間隙)。
*   **外觀造型**：`.s_custom_arrowL` (箭頭變大), `.s_custom_arrowRadius` (按鈕變圓形), `.s_custom_arrowNoLine` (隱藏按鈕邊線與底色，純箭頭)。

## 6. 排版與標題裝飾 (Typography & Decoration)
*   **流體字體 (Fluid Type)**：`h1` 到 `h6` 皆使用基於 viewport (`vw`) 的 `clamp()`，自動縮放，絕不破版。
*   **斷行縮略 (Line Clamp)**：利用預設 mixin (`@include clamp-2`, `@include clamp-3`) 控制行數。
*   **.s_custom_titleUnderLine**：在標題（H1~H4）加上左側下方 / 置中下方的短色塊底線（裝飾線）。

## 7. 色彩主題繼承 (Color Themes Hooks)
給予外層區塊 `.o_cc1` 到 `.o_cc5`：
*   所有 `.btn-primary`, `.btn-secondary` 會自動換成該主題的主配色與副配色。
*   麵包屑 (Breadcrumbs)、Active 分頁標籤底線、卡片文字色彩、標題底線等，皆會自動跟隨所選的色系變數。

## 8. 斷點與 RWD 輔助 (Breakpoints & RWD)

> [!WARNING]
> **絕對禁止寫死 Media Queries (CRITICAL)**：
> AI 產出 SCSS 時，**絕對不允許**使用 `@media (max-width: 767.98px)` 這種寫死的原生語法。
> 必須 **完全強制** 使用 Bootstrap 4.5 的內建 Mixin：
> - `@include media-breakpoint-down(sm)` 代替 手機版斷點 (<768px)
> - `@include media-breakpoint-down(md)` 代替 平板版斷點 (<992px)
> - `@include media-breakpoint-up(lg)` 代替 電腦版斷點 (>=992px)

Odoo 預設使用 Bootstrap 4.5 的斷點。但專案額外提供了以下自訂斷點 mixin，處理極端尺寸：
*   **筆電與大螢幕**：
    *   `@include laptop { ... }` 針對 max-width: 1439px
    *   `@include laptop-m { ... }` 針對 max-width: 1365px
*   **極小螢幕手機**：
    *   `@include mobile-s { ... }` 針對 max-width: 450px
    *   `@include mobile-xs { ... }` 針對 max-width: 375px
若一般 Bootstrap 斷點無法滿足排版細節，請優先使用此四組自訂 Mixin。

---

> [!TIP]
> **堆疊與錯位設計指南 (Overlapping Design)**
> 這份 SCSS 提供極大組合彈性。若是設計需要「圖文錯位」、「卡片懸浮重疊」：
> 1. 以 `.container` 或 `.container-fluid` 確保 RWD 安全骨架。
> 2. 以 Bootstrap 負邊距（如 `mt-lg-n5`）讓內容局部上移重疊。
> 3. 🚨 **編輯器保護機制**：所有這種覆蓋、負位移、絕對定位的 CSS，**必須**使用 `#wrapwrap:not(.odoo-editor-editable)` 包覆！例如：
>    ```scss
>    #wrapwrap:not(.odoo-editor-editable) {
>        .my-overlap-box { margin-top: -50px; position: absolute; }
>    }
>    ```
>    這樣在 Odoo 網頁編輯模式下，元素才會回到正常的 document flow，讓客戶點得到、改得到文字或圖片！
> 4. 追加 `.s_custom_cardRadius .shadow` 增加卡片浮力。
> 5. 以自訂 Padding 系統 `.pb64` 來墊高底部空間，防止負高度吃掉下一個 Section。

## 9. 禁止濫用 #wrapwrap 提升權重 (CSS Specificity Rule)
> [!WARNING]
> **嚴禁將整個元件的 SCSS 都包在 `#wrapwrap` 內！**
> `#wrapwrap` 的 ID 選擇器權重為 `1,0,0`，非常高。如果 AI 開發一般區塊時習慣將所有 CSS 都放入 `#wrapwrap { ... }`，會導致：
> 1. 該區塊的 CSS 變得難以覆寫與擴充。
> 2. 破壞 Odoo 區塊的模組化與複用性。
> 
> **正確做法：**
> - 一般區塊的樣式應直接繼承或使用自訂的頂層 class（例如：`.s_custom_news_hanaria { ... }`）來做 Scoping。
> - **只有在**進行上一點 (Overlapping Design) 所提到的「絕對定位、負邊距等會干擾編輯器的特殊排版魔法」時，才可以使用 `#wrapwrap:not(.odoo-editor-editable)` 來鎖定特定情境。

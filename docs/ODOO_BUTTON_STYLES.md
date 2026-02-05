# Odoo Custom Button Styles

此文件記錄了專案中可用的客製化按鈕樣式及其對應的 SCSS 代碼。
這些按鈕樣式通常不包含在預設主題中，需要透過 `s_embed_code` 或主題 SCSS 檔案手動加入。

## 1. 右下角斜切 (Right Skew) - `.s_custom_btn01`

**描述**: 按鈕右下角呈現斜切效果。

```scss
//------------------ 按鈕，右下角斜切 開始 ------------------//
.oe_website_sale>section .btn i {
    display: none;
}
.btn-fill-primary,
.btn-primary {
    background-color: var(--o-cc1-btn-primary);
}
.btn-fill-primary:hover,
.btn-primary:hover {
    background-color: var(--o-cc1-btn-primary-border);
}
.btn-fill-secondary,
.btn-secondary {
    background-color: var(--o-cc1-btn-secondary);
}
.btn-fill-secondary:hover,
.btn-secondary:hover {
    background-color: var(--o-cc1-btn-secondary-border);
}
.o_cc2 {
    .btn-fill-primary,
    .btn-primary {
        background-color: var(--o-cc2-btn-primary);
        &:hover {
            background-color: color-mix(in srgb, var(--o-cc2-btn-primary) 90%, #222 35%);
        }
    }
    .btn-fill-secondary,
    .btn-secondary {
        background-color: var(--o-cc2-btn-secondary);
        &:hover {
            background-color: var(--o-cc2-btn-secondary);
            background-color: color-mix(in srgb, var(--o-cc2-btn-secondary) 90%, #222 35%);
        }
    }
}
.o_cc3 {
    .btn-fill-primary,
    .btn-primary {
        background-color: var(--o-cc3-btn-primary);
        &:hover {
            background-color: var(--o-cc3-btn-primary-border);
            background-color: color-mix(in srgb, var(--o-cc3-btn-primary) 90%, #222 35%);
        }
    }
    .btn-fill-secondary,
    .btn-secondary {
        background-color: var(--o-cc3-btn-secondary);
        &:hover {
            background-color: color-mix(in srgb, var(--o-cc3-btn-secondary) 90%, #222 10%);
        }
    }
}
.o_cc4 {
    .btn-fill-primary,
    .btn-primary {
        background-color: var(--o-cc4-btn-primary);
        &:hover {
            background-color: var(--o-cc4-btn-primary-border);
        }
    }
    .btn-fill-secondary,
    .btn-secondary {
        background-color: var(--o-cc4-btn-secondary);
        &:hover {
            background-color: var(--o-cc4-btn-secondary-border);
        }
    }
}
.o_cc5 {
    .btn-fill-primary,
    .btn-primary {
        background-color: var(--o-cc5-btn-primary);
        &:hover {
            background-color: color-mix(in srgb, var(--o-cc5-btn-primary) 90%, #222 35%);
        }
    }
    .btn-fill-secondary,
    .btn-secondary {
        color: var(--o-cc5-link);
        background-color: var(--o-cc5-btn-secondary);
        border-color: var(--o-cc5-btn-secondary-border);
        &:hover {
            color: var(--o-cc5-link);
            background-color: color-mix(in srgb, var(--o-cc5-btn-secondary) 90%, #222 5%);
        }
    }
    .btn-outline-secondary {
        color: var(--o-cc5-btn-secondary);
        border-color: var(--o-cc5-btn-secondary);
        &:hover {
            color: var(--o-cc5-btn-secondary-link);
            background-color: var(--o-cc5-btn-secondary-border);
            border-color: var(--o-cc5-btn-secondary-border);
        }
    }
}
#wrapwrap .btn:not(.dropdown-toggle):not(.oe_search_button):not([class*=outline]):not(.o_add_compare_dyn):not(.o_add_compare):not(.o_add_quote):not(.js_add_cart_json) {
    padding-right: 30px;
    mask: linear-gradient(314deg, rgba(167, 11, 11, 0) 16px, black 0%);
    border: none;
}
//------------------ 按鈕，右下角斜切 結束 ------------------//
```

## 2. 一般箭頭 (Normal Arrow) - `.s_custom_btn021`

**描述**: 按鈕右側帶有 FontAwesome 箭頭，Hover 時有晃動動畫。

```scss
//------------------ 按鈕，普通箭頭 開始 ------------------//
.oe_website_sale>section .btn i {
    display: none;
}
.btn-fill-primary,
.btn-primary {
    background-color: var(--o-cc1-btn-primary);
    border-color: var(--o-cc1-btn-primary);
    &:hover {
        background-color: var(--o-cc1-btn-primary-border);
        border-color: var(--o-cc1-btn-primary-border);
    }
}
.btn-fill-secondary,
.btn-secondary {
    background-color: var(--o-cc1-btn-secondary);
    border-color: var(--o-cc1-btn-secondary);
    &:hover {
        background-color: var(--o-cc1-btn-secondary-border);
        border-color: var(--o-cc1-btn-secondary-border);
    }
}
.o_cc2 {
    .btn-fill-primary,
    .btn-primary {
        background-color: var(--o-cc2-btn-primary);
        border-color: var(--o-cc2-btn-primary-border);
        &:hover {
            background-color: color-mix(in srgb, var(--o-cc2-btn-primary) 90%, #222 35%);
            border-color: var(--o-cc2-btn-primary-border);
        }
    }
    .btn-fill-secondary,
    .btn-secondary {
        background-color: var(--o-cc2-btn-secondary);
        border-color: var(--o-cc2-btn-secondary-border);
        &:hover {
            background-color: color-mix(in srgb, var(--o-cc2-btn-secondary) 90%, #222 35%);
            border-color: var(--o-cc2-btn-secondary-border);
        }
    }
}
.o_cc3 {
    .btn-fill-primary,
    .btn-primary {
        background-color: var(--o-cc3-btn-primary);
        border-color: var(--o-cc3-btn-primary-border);
        &:hover {
            background-color: color-mix(in srgb, var(--o-cc3-btn-primary) 90%, #222 35%);
            border-color: var(--o-cc3-btn-primary-border);
        }
    }
    .btn-fill-secondary,
    .btn-secondary {
        background-color: var(--o-cc3-btn-secondary);
        border-color: var(--o-cc3-btn-secondary-border);
        &:hover {
            background-color: color-mix(in srgb, var(--o-cc3-btn-secondary) 90%, #222 35%);
            border-color: var(--o-cc3-btn-secondary-border);
        }
    }
}
.o_cc4 {
    .btn-fill-primary,
    .btn-primary {
        background-color: var(--o-cc4-btn-primary);
        border-color: var(--o-cc4-btn-primary-border);
        &:hover {
            background-color: var(--o-cc4-btn-secondary);
            border-color: var(--o-cc4-btn-secondary);
        }
    }
    .btn-fill-secondary,
    .btn-secondary {
        background-color: var(--o-cc4-btn-secondary);
        border-color: var(--o-cc4-btn-secondary);
        &:hover {
            color: var(--o-cc4-btn-link);
            background-color: var(--o-cc4-btn-secondary-border);
            border-color: var(--o-cc4-btn-secondary-border);
        }
    }
}
.o_cc5 {
    .btn-fill-primary,
    .btn-primary {
        background-color: var(--o-cc5-btn-primary);
        &:hover {
            color: var(--o-cc5-btn-link);
            background-color: color-mix(in srgb, var(--o-cc5-btn-primary) 90%, #222 35%);
            border-color: var(--o-cc5-btn-primary-border);
        }
    }
    .btn-fill-secondary,
    .btn-secondary {
        background-color: var(--o-cc5-btn-secondary);
        border-color: var(--o-cc5-btn-secondary-border);
        &:hover {
            background-color: color-mix(in srgb, var(--o-cc5-btn-secondary) 90%, #222 35%);
            border-color: var(--o-cc5-btn-secondary-border);
        }
    }
    .btn-outline-secondary {
        color: var(--o-cc5-link);
        border-color: var(--o-cc5-link);
        &:hover {
            background-color: var(--o-cc5-btn-secondary);
            border-color: var(--o-cc5-btn-secondary);
        }
    }
}
header,
section {
    .btn:not(.dropdown-toggle):not(.oe_search_button):not(.o_add_compare_dyn):not(.o_add_compare):not(.o_add_quote) {
        display: inline-flex !important;
        gap: 6px;
        align-items: center;
        justify-content: center;
        overflow: hidden;
        &:hover::after {
            animation: btn-bounce-left2 .8s ease 0s 1 normal none;
        }
        &::after {
            content: "\f105";
            font-family: "FontAwesome" !important;
            display: block;
            padding-left: 4px;
            z-index: 2;
            pointer-events: none;
        }
        i {
            display: none;
        }
    }
}
#wrapwrap.odoo-editor-editable {
    header,
    section {
        .btn:not(.dropdown-toggle):not(.oe_search_button):not(.o_add_compare_dyn):not(.o_add_compare):not(.o_add_quote):hover::after {
            animation: none;
        }
    }
}
@keyframes btn-bounce-left2 {
    20% {
        transform: translateX(0);
    }
    50% {
        transform: translateX(4px);
    }
    100% {
        transform: translateX(0);
    }
}
//------------------ 按鈕，普通箭頭 結束 ------------------//
```

## 3. 細箭頭 (Thin Arrow) - `.s_custom_btn02`

**描述**: 使用 CSS 繪製的細長箭頭，Hover 時有拉伸效果。

```scss
//------------------ 按鈕，細箭頭 開始 ------------------//
.oe_website_sale>section .btn i {
    display: none;
}
.btn-fill-primary,
.btn-primary {
    background-color: var(--o-cc1-btn-primary);
    border-color: var(--o-cc1-btn-primary);
    &:hover {
        background-color: var(--o-cc1-btn-primary-border);
        border-color: var(--o-cc1-btn-primary-border);
    }
}
// ... (類似的 o_cc1 - o_cc5 變數設定，略以簡化文件，實作時請參考完整代碼)

header,
section {
    .btn:not(.dropdown-toggle):not(.oe_search_button):not(.o_add_compare_dyn):not(.o_add_compare):not(.o_add_quote) {
        display: inline-flex !important;
        gap: 6px;
        align-items: center;
        justify-content: center;
        overflow: hidden;
        &:hover::after {
            animation: btn-bounce-left 1s ease 0s 1 normal none;
        }
        &::after {
            content: "";
            width: 22px;
            height: 8px;
            border-right: 1px solid currentColor;
            border-bottom: 1px solid currentColor;
            display: block;
            z-index: 2;
            transform: skewX(45deg);
            pointer-events: none;
        }
        &.btn-lg::after {
            height: 10px;
            width: 30px;
        }
        &.btn-sm::after {
            height: 5px;
            width: 14px;
        }
        i {
            display: none;
        }
    }
}
#wrapwrap.odoo-editor-editable {
    header,
    section {
        .btn:not(.dropdown-toggle):not(.oe_search_button):not(.o_add_compare_dyn):not(.o_add_compare):not(.o_add_quote):hover::after {
            animation: none;
        }
    }
}
@keyframes btn-bounce-left {
    20% {
        transform: translateX(-2px) skewX(45deg);
    }
    80% {
        transform: translateX(6px) skewX(45deg);
    }
    90% {
        transform: translateX(-2px) skewX(45deg);
    }
    100% {
        transform: skewX(45deg);
    }
}
//------------------ 按鈕，細箭頭 結束 ------------------//
```

## 4. 實心圓點放大 (Dot Zoom) - `.s_custom_btn03`

**描述**: 按鈕左側有一圓點，Hover 時圓點放大填滿整個按鈕背景。
**特別注意**: 左右 Padding 至少需要 36px (小: 20px, 大: 40px)。

```scss
//------------------ 按鈕，實心圓點放大 開始 ------------------//
$dot-bg: #fff;   // 預設: 白色，其餘有接上色彩球變數者不使用此變數
.oe_website_sale>section .btn i {
    display: none;
}
#wrapwrap {
    .btn[class*='primary'],
    .btn[class*='secondary'] {
        &:not(.dropdown-toggle):not(.oe_search_button):not(.o_add_compare_dyn):not(.o_add_compare):not(.o_add_quote) {
            position: relative;
            z-index: 2;
            overflow: hidden;
            &::before {
                content: "";
                width: 6px;
                height: 6px;
                border-radius: 50%;
                position: absolute;
                top: 50%;
                left: 12px;
                z-index: -1;
                transform: translateY(-50%);
                pointer-events: none;
            }
            &:hover {
                // background-color: unset !important;
                &::before {
                    transform: scale(100);
                }
            }
            &.btn-lg::before {
                left: 16px;
            }
            &.btn-sm::before {
                width: 4px;
                height: 4px;
                left: 8px;
            }
        }
    }
    .oe_edited_link::before {
        border: none;
    }
    // ... 其他變數配置
}
//------------------ 按鈕，實心圓點放大 結束 ------------------//
```

## 5. 閃光效果 (Flash) - `.s_custom_btn04`

**描述**: Hover 時有一道光影快速掃過按鈕。

```scss
//------------------ 按鈕，閃光效果 開始 ------------------//
.oe_website_sale>section .btn i {
    display: none;
}
// ... 顏色變數設定
#wrapwrap:not(.odoo-editor-editable) {
    header,
    section {
        .btn:not(.dropdown-toggle) {
            position: relative;
            overflow: hidden;
            &:hover::before {
                animation: shine .75s;
            }
            &::before {
                position: absolute;
                top: 0;
                left: -75%;
                z-index: 2;
                display: block;
                content: '';
                width: 50%;
                height: 100%;
                background: linear-gradient(to right, rgba(255, 255, 255, 0) 0%, rgba(255, 255, 255, .3) 100%);
                transform: skewX(-25deg);
            }
        }
    }
}
@keyframes shine {
    100% {
        left: 125%;
    }
}
//------------------ 按鈕，閃光效果 結束 ------------------//
```

## 6. Hover 傾斜填滿 (Hover Skew) - `.s_custom_btn06`

**描述**: Hover 時背景色以斜切方式填滿按鈕。

```scss
@mixin transition {
    transition: all 0.5s ease-in-out;
}
//------------------ 按鈕，hover 傾斜填滿 開始 ------------------//
.oe_website_sale>section .btn i {
    display: none;
}
#wrapwrap {
    // ... 顏色變數配置
    .btn[class*='primary'],
    .btn[class*='secondary'] {
        &:not(.dropdown-toggle):not(.oe_search_button):not(.o_add_compare_dyn):not(.o_add_compare):not(.o_add_quote) {
            position: relative;
            z-index: 2;
            overflow: hidden;
            &:hover {
                background-color: unset;
                &::after {
                    height: 350%;
                    width: 130%;
                }
            }
            &::after {
                content: '';
                width: 100%;
                height: 0;
                position: absolute;
                top: 50%;
                left: 50%;
                z-index: -1;
                transform: translate(-50%, -50%) rotate(-30deg);
                pointer-events: none;
                @include transition;
            }
        }
    }
}
#wrapwrap.odoo-editor-editable {
    .btn[class*='primary'],
    .btn[class*='secondary'] {
        &:not(.dropdown-toggle):not(.oe_search_button):not(.o_add_compare_dyn):not(.o_add_compare):not(.o_add_quote)::after {
            transition: none;
        }
    }
}
//------------------ 按鈕，hover 傾斜填滿 結束 ------------------//
```

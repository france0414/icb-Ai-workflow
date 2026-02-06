Rules:

- 你正在做 Odoo WebBuilder 的頁面
- 記得所有 xml ，必須由以下母節點包覆:
<t t-name="website.xxxxxx">
    <t t-call="website.layout">
        <div id="wrap" class="oe_structure oe_empty"/>
    </t>
</t>
- 輸入目錄在 outputs，所有檔案輸出時的黨名必須有當天的日期與時間。
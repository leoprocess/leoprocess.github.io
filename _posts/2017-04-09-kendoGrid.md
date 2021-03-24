---
layout:     post
title:      kendoGrid Template 
subtitle:   kendoGrid Template 格式問題 
date:       2017-04-09
author:     BY
header-img: 
catalog: true
tags:
    - 筆記
---

# 筆記

最近工作上遇到，kendoGrid需要將Grid內的值做字串分割還要加入連結

卡關了一陣子...

主要問題在於切割出來的字串 莫名的會無法放入在參數內

即便畫面顯示正常 但onclick事件就是無法成功

錯誤訊息:Uncaught SyntaxError: Invalid or unexpected token

後來看了參數 發現和 空白 有相當大的關係

原來在createTemplateFor事件內 

onclick test內放置的參數 習慣上都會寫成這樣 參數1, 參數2, 參數3 ....

就是中間的空白導致onclick事件無法成功 

後來改為 參數1,參數2,參數3,....

就這樣好了  呵呵....

範例如下:

<pre>
  function test(name, age){
  alert("name:" + name + "  age:" + age);
  };
  
  function createTemplateFor(columnName) {
    var template = "# var address = data['" + columnName + "']; #" +
        "# var splitaddr = address.split(','); #" +
        "# for (var i = 0; i < splitaddr.length; i++) { #" +
        "<div onclick=test('#= splitaddr[i] #','#=age#')>#=splitaddr[i]#</div><br /> " +
        "# } #";

    return template;
}
$("#grid").kendoGrid({
  sortable: true,
        columnMenu: true,
        filterable: true,
        dataSource: [{
            name: "",
            age: 30
        }, {
            name: "123456789,23456789,3456789",
            age: 32
        },{
            name: "3456789,23456789,123456789",
            age: 33
        }],
        columns: [{
            field: "name",
            template: createTemplateFor("name"),
          attributes: { "class": "table-cell", style: "font-size: 14px;color:blue;" }
        }, {
            field: "age"
        }, {
            field: "rowNumber",
            title: "Row number",
            template: "<span class='row-number'></span>"
        }],
        dataBound: function () {
            var rows = this.items();
            $(rows).each(function () {
                var index = $(this).index() + 1;
                var rowLabel = $(this).find(".row-number");
                $(rowLabel).html(index);
            });
        }
});
</pre>

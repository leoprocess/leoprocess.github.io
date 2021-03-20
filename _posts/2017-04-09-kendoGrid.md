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

後來看了參數 發現和 空白 & "符號 有相當大的關係

所以就另外寫了方法將空白特別去除掉

範例如下:

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <title>Kendo UI Snippet</title>

    <link rel="stylesheet" href="https://kendo.cdn.telerik.com/2021.1.224/styles/kendo.default-v2.min.css"/>

    <script src="https://code.jquery.com/jquery-1.12.4.min.js"></script>
    <script src="https://kendo.cdn.telerik.com/2021.1.224/js/kendo.all.min.js"></script>
</head>
<body>
  
<div id="grid"></div>
<script>
  function test(name, age){
  alert("name:" + name + "  age:" + age);
  };
  function replaceString(value) {
    console.log(value.split(" ").join(""));
        return value.split(" ").join("");
    }
  function createTemplateFor(columnName) {
    var template = "# var address = data['" + columnName + "']; #" +
        "# var splitaddr = address.split(','); #" +
        "# for (var i = 0; i < splitaddr.length; i++) { #" +
        "<div onclick=test('#= replaceString(splitaddr[i]) #','#=age#')>#=splitaddr[i]#</div><br /> " +
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
</script>
</body>
</html>

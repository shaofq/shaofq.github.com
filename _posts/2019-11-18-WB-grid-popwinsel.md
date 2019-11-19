---
layout: post
title:  "Wb表格弹出框选择数据给表格赋值"
categories: WB
tags: 效率 vscode markdown
author: runningshao
---

* content
{:toc}

# 弹出窗口（winsfhr）ok按钮事件

```
var sel = app.gridTrust.getSelection()[0];
if (!sel) {
  Wb.warn("请选择货物");
  return;
}
console.log(sel);
var detail = app.gridDetail.getSelection()[0];
detail.set("WEIGHT_WGT_REMAIN_OUT", sel.data.WEIGHT_NUM);
detail.set("TRUST_NO", sel.data.TRUST_NO);
detail.set("CARGO_NAM_COD", sel.data.CARGO_NAM_COD);
detail.set("CARGO_NAM", sel.data.CARGO_NAM);
detail.set("CARGO_KIND_COD", sel.data.CARGO_KIND_COD);
detail.set("CARGO_KIND_NAM", sel.data.CARGO_KIND_NAM);
app.winTrust.hide();
```

# grid单击事件
*  grid的itemclick事件

```
Wb.columnClick(view, e, "GETOWNER_NAM", function() {
  Wb.reset(app.winsfhr);
  app.winsfhr.show();
  app.isGet=true;

  app.gridsfhr.store.load();
});
```


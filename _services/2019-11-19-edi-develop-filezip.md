---
title:  "EDI报文多个文件生成并压缩"
categories: EDI
tags: 效率 vscode markdown
author: runningshao
---

* content
{:toc}

# edi生成文件添加进filelist

```
fileList.add(new ByteArrayInputStream(HhUtil.byteArrayToStr(bean.getDataout())));
```

# fileList文件输出并压缩

```
 OutputStream fos2 = new ByteArrayOutputStream();
        ZipUtils.toZip(fileList, "CN1101." + DateUtil.getSystime(), fos2);
        InputStream in = new ByteArrayInputStream(((ByteArrayOutputStream) fos2).toByteArray());
        response.setHeader("Content-type", "aapplication/octet-stream");
        response.setHeader("content-disposition", "attachment;" + "filename=" + DateUtil.getSystime() + "-cn1101.zip");
        WebUtil.send(response, in);
```


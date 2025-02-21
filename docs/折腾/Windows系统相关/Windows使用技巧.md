
## 删除文件不扫描

```BASH
DEL /Q/S 文件
```

/Q 不询问是否删除目录和子目录

/S 删除指定目录中的子目录和文件

## 创建软链接

```BASH
mklink /d 要建立的文件 源文件
mklink /d E:\pysot\testing_dataset\OTB100 E:\dataset\OTB2015\OTB100
mklink /d E:\pysot\testing_dataset\VOT2018 E:\dataset\VOT2018
mklink /d F:\Project\WebBlog\MkDocs\docs F:\Project\WebBlog\Repository\docs
```
# MkDocs 使用日常

[MkDocs中文文档](https://markdown-docs-zh.readthedocs.io/zh_CN/latest/)

## 安装

使用pip安装：

```bash
pip install mkdocs
```

## 使用

新建项目： 略，见中文文档

建立项目后的目录结构：

```bash
|
|--mkdocs.yml
|
|--docs
    |
    |--index.md
    |
    ...
```

其中`mkdocs.yml`文件为站点的配置文件，而站点内容则全部以`.md`文件的形式存放在`docs`文件夹中。

站点内容的组织形式则在配置文件中以目录树的形式确定。

### 本地测试

在配置文件相同目录下使用：

```bash
mkdocs serve
```

在浏览器中打开得到的网址即可查看网站效果，并可以一边配置文件或文档内容修改一边预览效果。

### 建立本地站点

在配置文件目录下执行：

```bash
mkdocs build
```

会建立一个名为`site`的目录，即建立的静态站点，下一步只需将其发布即可。

### 发布



#GitHub pages使用指南
## Github Pages简介
GitHub Pages is a static site hosting service designed to host your personal, organization, or project pages directly from a GitHub repository.  
see more in [What is GitHub Pages?](https://help.github.com/articles/what-is-github-pages/)

To enable github pages, in Github repository's page -> setting -> github pages section
* set github page source branch with one of three options: master branch, master branch docs folder or gh-pages branch
* choose theme using **Theme Chooser** or just add _config.yml in github pages root location

## Create a static website
* html+css+js，push to github repository
* Markdown + Hexo
  * Hexo comiple the makrdown files to web static files and depoly to a github repository's branch
  * 参见[hexo是怎么工作的](http://coderunthings.com/2017/08/20/howhexoworks/)
* Markdown + Jekyll
  * Github Pages support Jekyll, you just write markdown files.
  * Customize Jekyll theme's default, override content in `_layouts, _includes, _sass, assets` folders
  * see more in [Using Jekyll as a static site generator with GitHub Pages](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/)
  
## Markdow语法
* 基本语法
  * 标题 -- #,===,---
  * 段落 -- 空行位于单行或多行文字之间
  * 换行 -- 行尾添加2个以上空格然后回车换行
  * 强调 -- 加粗 **（可用于单词内部）或__；斜体*（可用于单词内）或_；粗斜体 ***或___
  * 块引用 -- >
  * 列表 -- 数字（开始为1，后续可不连续）为有序列表；+，-，*为无序列表；列表内嵌元素需以tab或四个空格开始
  * 代码 -- 至少四个空格或者一个tab缩进的代码块 或·（行内）；···，~~~代码块，代码块首行可附加代码块语言，进行语法高亮
  * 图片 ![标题](url)
  * 链接 [标题](url)
* 扩展语法
  * 分割线 ****或者----或者_____
  * 表格 |--；对齐:-(左对齐），:--:(中对齐），--:（右对齐）
  * 脚注 [^数字或文字标记]
  * 标题ID 标题{#id}
  * 表述列表 关键词，下一行:加一个空格开始
  * 删除线 ~~
  * 任务列表 [x]已完成任务，[ ]待完成任务
  * url自动生成文字链接，`url`禁止自动生成链接
* 详见：  
  * [Markdown Basic Syntax](https://www.markdownguide.org/basic-syntax/)
  * [Markdown Extended Syntax](https://www.markdownguide.org/extended-syntax)
  
## TBD
[ ] Jekyll and Hexo internals
[ ] How to customize Jekyll or Hexo
[ ] Reinventing the wheel like Jekyll or Hexo.

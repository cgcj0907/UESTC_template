# Welcome!

# 目录
- [项目简介](#项目简介)
- [安装与运行](#安装与运行)
- [文件介绍](#文件介绍)
- [个性化设计](#个性化设计)
- [注意事项](#注意事项)
- [特别感谢](#特别感谢)
- [在线 PDF 转 docx 网站](#在线网站)

# 项目简介
本项目基于 `pandoc` 和 `Latex` 构建, 通过该模板可快速排版符合 UESTC 要求的论文,包括课程论文-综设报告-学位论文
> 模板基于王稳师兄改良!

# 安装与运行

## 1. 安装
> 个人推荐 `Windows` 环境, 因为 `Latex` 完整安装包大约 7GB
### [Pandoc](https://pandoc.org/installing.html)

> <font color="red">!</font> For the newest version, you should follow the link beyond.
#### For Windows
* [安装地址](https://github.com/jgm/pandoc/releases/tag/3.8.2.1)(选择符合自己电脑版本的安装包)
#### For Linux
```bash
sudo apt update
wget https://github.com/jgm/pandoc/releases/download/3.8.2.1/pandoc-3.8.2.1-1-amd64.deb
sudo dpkg -i pandoc-3.8.2.1-1-amd64.deb
pandoc --version 
```
If you see similar output below, you have installed the pandoc successfully.
```bash
pandoc 3.8.2.1
Compiled with pandoc-types 1.22, texmath 0.12.0.1, skylighting 0.11.0.1
```
### [Latex](https://www.latex-project.org/get/)
#### For Windos
* [安装包](https://mirror.ctan.org/systems/texlive/tlnet/install-tl-windows.exe) <font color = "yellow">提示: Latex 下载时间较慢,可以根据安装指导选择合适镜像</font>
* 安装完后, 需在 `设置>系统>系统信息>高级系统设置>环境变量` 中配置用户变量, 在 `path` 中添加 `C:\texlive\2025\bin\windows`

#### For Linux
The installation of Latex maybe a llite difficult, you can follow the commands below, or you can click the link beyond and seek help from ChatGpt.
```bash
cd /tmp
wget https://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
zcat < install-tl-unx.tar.gz | tar xf - # note final - on that command line
cd install-tl-2*
perl ./install-tl --no-interaction
```
Finally, prepend /usr/local/texlive/YYYY/bin/PLATFORM to your PATH,
e.g., /usr/local/texlive/2025/bin/x86_64-linux

## 2. 运行
> 在此之前, 我建议你先阅读文件介绍, 有助于你理解命令

### 2.1 对已有 `markdown` | `word` 文件进行格式转换, 得到 `tex` 

> 先转换成 `tex`, <font color = "red">再运行 2.2 相同的命令</font>
#### ! 执行命令前准备
1. 切换到你的工作目录
```bash
cd your_workdirection
```
2. 保证要转换的文件与模板在同一目录下,如下图
```bash
\root
|--\pic #图片资源
|--template.tex
|--yours.md
|--yours.docx
```

3. 删除文档标号

> 模板会自动添加标号,所以原文档无需重复标号


4. 将文献存入bib文件(如果有文献需求),格式参见 `reference.bib`
```
@article{wang1999sanwei,
  title = {三维矢量散射积分方程中奇异性分析},
  author = {王浩刚 and 聂在平},
  journal = {电子学报},
  volume = {27},
  number = {12},
  pages = {68 -- 71},
  year = {1999}
}
```

5. 仔细阅读<a href="#注意事项">注意事项</a>, 仔细阅读<a href="#注意事项">注意事项</a>, 仔细阅读<a href="#注意事项">注意事项</a>!!!
#### Linux
```bash
# markdown 文件
pandoc yours.md \
  --from markdown \
  --to latex \
  --template=md.tex \ # 模板文件
  --output=output.tex \ # 输出文件
  --top-level-division=chapter

# docx 文件
pandoc yours.docx \
  --from docx \
  --to latex \
  --template=docx.tex \ 
  --output=output.tex \
  --top-level-division=chapter 
```

#### Windows
```bash
# markdown 文件
pandoc yours.md --from markdown --to latex --template=md.tex --output=output.tex --top-level-division=chapter 

# docx 文件
pandoc yours.docx --from docx --to latex --template=docx.tex --output=output.tex --top-level-division=chapter
```

### 2.2 原生 `tex` 文件转换成 `pdf`
```bash
# output.tex 只是一个示范, 需替换成你生成的 tex 文件
xelatex output.tex      # 第一次编译生成 aux 文件
bibtex output           # 处理参考文献
xelatex output.tex      # 第二次编译更新引用
xelatex output.tex      # 第三次编译最终完善目录
```

# 文件介绍
## `tex` 文件
### 1. `main.tex`
  示例文件,主要展示了该模板的各种功能,成品效果可以查看 `main.pdf`

### 2. `example.tex`
  模板文件,提供简化后的框架,剩余内容需自己补充

### 3. `md.tex` | `docx.tex`
  转换模板文件,将你之前撰写的 `markdown`, `word`等文件转换成按照该模板风格的 `pdf`文件

## `cls` 文件
  `thesis-uestc.cls` 文件是该模板的核心文件, 里面定义了各模块的格式, 可以根据自己所需添加或者更改格式, 详细操作可见 <a href='#个性化设计'>个性化设计</a>

## `bib` 文件
  文献管理文件, 所有参考文献都要按照格式写在这里

## 个性化设计

### 个性化封面和页眉

* 查看 `thesis-uestc.cls`, 找到 `\DeclareOption` 模块

例如, 可以自己设定信软学院进阶式挑战性项目 III 中期报告
```cls
\DeclareOption{report}{
  \def\chinesedegreename{报告}
  \def\englishdegreename{Report}
  \def\chinesebooktitle{综设报告}
  \def\englishbooktitle{Design Report}
  \def\display@chineseheader{信软学院进阶式挑战性项目 III 中期报告}
  \def\display@englishheader{Design Report of University ofReport
    Electronic Science and Technology of China}
}
```
### 个性化文献引用

* 查看 `thesis-uestc.bst`, 找到特定函数

例如 在`function(article)`加上如下代码, 就能在文献引用显示 url
```diff
FUNCTION {article}
{
    bibitem.begin
    format.authors write$ add.period
    format.title "[J]" * write$ add.period
    format.journal write$ add.comma
    format.year write$
    
    volume missing$ {
      pages missing$ 'skip$
      {
        add.comma
        format.pages write$
      } if$
    }
    { add.comma
      number missing$ { volume write$ }
      {
        volume "(" * number *
        ")" * write$
      } if$
      pages missing$ 'skip$ { ": " format.pages * write$ } if$
    } if$
   
+    url empty$ 'skip$ { add.comma " " url * write$ } if$
    "." write$
    newline$
}
```

# 注意事项

* 仔细查看注解, 留意需替换的地方
* 选择模板要对 `tex` 文件开头进行修改, 例如
```tex
\documentclass[master]{thesis-uestc} % 博士论文
\documentclass[course]{thesis-uestc} % 课程论文
```
* 引用文献需在 `tex` 文件中修改如下代码
```tex
% xxxxx替换成你自己创建的bib文件,无需后缀,无需后缀,无效后缀!!!
\thesisbibliography{xxxxx}
```
* 如果使用 `report` 模板(针对信软的同学), 需自行添加封面 `report-cover.docx` (还在开发中), 页眉的修改可在 `thesis-uestc.cls`
```diff
\DeclareOption{report}{
  \def\chinesedegreename{报告}
  \def\englishdegreename{Report}
  \def\chinesebooktitle{综设报告}
  \def\englishbooktitle{Design Report}
-  \def\display@chineseheader{信软学院进阶式挑战性项目 III 中期报告}
+  \def\display@chineseheader{你本学期的项目报告名称}
  \def\display@englishheader{Design Report of University ofReport
    Electronic Science and Technology of China}
}
```
* 如果论文题目较短, 导致有多余横线可以修改 `thesis-usetclcls`, 找到 `\newcommand{\makecover}`
```diff
\newcommand{\makecover}{

  ...

  \begin{tabular}{lp{4.2in}}
    \bfseries\fontsize{18pt}{18pt}\selectfont 论文题目 & \multirow[t]{2}{4.2in}{
        \centering
          \bfseries\fontsize{18pt}{18pt}\selectfont
          \zh@thetitle
    } \\
    \cline{2-2}
-            & \\
-    \cline{2-2}
  \end{tabular}\hspace*{\fill} \\[\baselineskip]

...

}
```
* 若第一章的页眉不希望是`绪论`可以更改 `thesis-uestc.cls` 以下部分代码
```diff
\apptocmd{\@chapter}{
  \ifdefined\@BODY
  \else
    \ifnum\value{chapter}=1
-      \ifchinesebook{\chaptermark{绪论}}{\chaptermark{Introduction}}
+      \ifchinesebook{\chaptermark{你自己的标题}}{\chaptermark{Introduction}}
      \setcounter{page}{1}
    \fi
  \fi
  \def\@BODY{\null}
}{}{}
```
* `tex` 中 `\begin{document}` 后不需要的内容删掉即可
* ...欢迎补充

# 特别感谢
  王稳师兄以及 `Overleaf` 提供的模板

# 在线网站

* [Smallpdf](https://smallpdf.com/cn/pdf-to-word)
* [ILovepdf](https://www.ilovepdf.com/pdf_to_word)  
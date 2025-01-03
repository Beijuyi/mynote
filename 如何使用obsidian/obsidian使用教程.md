# 官网
https://obsidian.md/
[教程](https://publish.obsidian.md/help-zh/%E7%BC%96%E8%BE%91%E4%B8%8E%E6%A0%BC%E5%BC%8F%E5%8C%96/%E7%BC%96%E8%BE%91%E7%9B%B8%E5%85%B3%E7%9A%84%E5%BF%AB%E6%8D%B7%E9%94%AE)

# 创建仓库

第一次打开 Obsidian 时，Obsidian 会询问你是否要创建一个新仓库。此时你有两个选择，你可以创建一个新仓库，或者将现有文件夹作为仓库打开。

仓库就是一个文件夹,包含一个.obsidian隐藏文件,是关于仓库的一些设置.
![[Pasted image 20241217103133.png]]![[Pasted image 20241217103211.png]]

# 设置附件

首先，应该设置一下附件位置和存放规则——“设置——文件与链接”中有两个附件相关的选项。

推荐使用：

- 指定的附件文件夹：就是设定一个文件夹专门用来保存附件（这里说的附件不单指图片）。这种方法附件可以集中管理。
- 当前文件所在文件夹下指定的子文件夹中：这句话说的比较绕，就是在你某篇笔记的身边建立一个文件夹，用来存放这篇笔记相关的附件。这种方法附件就在笔记身边，方便迁移。

设定好之后，本地的图片就直接粘贴或者拖拽过来就好，文件会自动复制到附件文件夹中。（其他类型附件的处理方法相同）

![[Pasted image 20241217103816.png]]

如果你觉得第二种方式（当前文件所在文件夹下指定的子文件夹中）适合你，可能你要了解一下细节。Obsidian 对于附件文件夹的自动建立不支持使用变量，也就是在某一个文件夹下只能有一个附件文件夹（因为同名），这并不灵活。这时候可以借助 [Custom Attachment location](https://github.com/RainCat1998/obsidian-custom-attachment-location) 插件来设定附件位置，而且支持自动文件重命名，文件名修改同步修改附件文件夹和附件名称。

在笔记中删除图片，并不会同步删除附件。

# 设置文件创建目录


![[Pasted image 20241217104703.png]]

# 内部链接

借助**内部链接**来链接笔记、附件。通过链接，你可以轻松构建起一张知识网络。

出于视觉上的考虑，Obsidian 默认使用 Wiki 式的内部链接。如果你更看**重链接的可迁移性**，则可以禁用 Wiki 式链接语法，将默认设置为 Markdown 式链接语法。

要默认使用 Markdown 式语法：
1. 打开**设置**。
2. 在**文件与链接**下，禁用**使用 Wiki 链接**功能。

![](Pasted%20image%2020241217110251.png)


# 标签

## 给笔记添加标签
要创建标签，在编辑器中输入井号 (`#`)，然后跟上主题词即可。例如，`#会议`。
标签不区分大小写。
标签不能包含空格。如果要分隔两个或多个单词，可以使用以下格式：
- [#camelCase](https://publish.obsidian.md/#camelCase)
- [#PascalCase](https://publish.obsidian.md/#PascalCase)
- [#snake_case](https://publish.obsidian.md/#snake_case)
- [#kebab-case](https://publish.obsidian.md/#kebab-case)
#标签


# 多光标模式

Obsidian允许你同时使用多个光标编辑文本。你可以通过按住`Alt`（或macOS上的`Option`）并在笔记中点击另一个位置来添加额外的光标。

要取消选择的文本以及所有额外的光标，在笔记中单击任意位置即可。你也可以通过按下`Escape`键来取消选择。


# 结合github使用

## .gitignore配置
忽略所有图片
```text
### obsidian ###
**/*.png
```
### 忽略规则

忽略文件

1. 忽略当前路径下的a.md文件： `a.md`
2. 忽略根目录下的e.md文件：`/e.md`
3. 忽略当前路径下的Abox文件夹下的b.md文件：`Abox/b.md`
4. 忽略当前路径下的Abox文件夹下**多级目录**下的的c.md文件：`Abox/**/c.md`
5. 忽略所有文件夹下的d.md文件：`**/d.md`
6. 忽略所有.png文件（某个后缀）：`*.png*`

忽略文件夹

1. 忽略当前路径下的Abox文件夹（整个文件夹都不上传）：`Abox/`
2. 忽略根目录下的Bbox文件夹：`/Bbox`

## 下载git 插件

![](Pasted%20image%2020241217131400.png)
我不需要自动同步,我需要手动控制提交

![](Pasted%20image%2020241217131833.png)


如何解决本地图片无法在github上展示的问题?
那就不在github上展示了

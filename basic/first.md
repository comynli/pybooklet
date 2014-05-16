# 最初的步骤

## 使用交互式解释器

Python是一种解释型语言，他提供了一个交互式的解释器，我们可以在交互式解释器里，交互的执行我们的代码。

第一章里，介绍了如何安装Python， 如果你已经安装了Python解释器，并且正确的设置了环境变量（pyenv会为你搞定这一切），那么，你只需要在你的终端里输入 `python`并按下回车，你将看到类似下面的输出：

![interactive](http://pybooklet.qiniudn.com/c2/interactive.png)

这样，你就进入了一个交互式解释器。

在交互式解释器中键入如下内容：
```python
print "Hello Python"
```

你会看到如下如下的输出：

![hello python](http://pybooklet.qiniudn.com/c2/hello.png)

恭喜你，你完成了你的第一个Python程序。是的，这就是Python的`hello world`，就是这么简单。接下来，你可以在你的解释器里键入一些其他内容了，例如：

```python
1 + 2
3.5 / 4
5 ** 10
```

不错，你可以把Python解释器当做计算机来使用。当你想要退出时，直接按下**control - d**即可退出交互式解释器。


### 使用IPython
在最初的兴奋过后，让我们回到现实，Python的交互式解释器很好用，非常好用，但是他完美吗？没有什么东西是完美的，在这个小节，将向你展示一个Python交互式解释器的增强版本，**IPython** 如果你对交互式解释器不是很感兴趣，而想尽快进入Python的世界，那么你可以跳过这个小节，直接看下面的内容，然而，**IPython**确实是一个非常好的工具，你值得拥有。

IPython不是Python的标准工具，在使用IPython之前，我们需要先安装它，IPython是按照标准的Python包打包的，所以我们可以使用Python的包管理工具安装它，这里我们使用**pip**来安装。pip是另外一个非常重要的Python工具，我们将在[包管理](/package.html)一节详细介绍。 通过pyenv安装的Python，都已经默认安装pip了，所以我们可以直接使用。通过pip安装IPython非常简单:
```bash
pip install ipython
```

以上命令会自动下载并安装ipython，使用过apt-get, yum, brew等包管理工具的同学，会对这样的包管理方式非常亲却。

如果是在pyenv管理的环境下，安装ipython后，你可能需要执行rehash命令，才能在`PATH`中找到正确的ipython。
```bash
pyenv rehash
```

通过以上步骤，你就完成了ipython的安装，只需要在终端里执行 `ipython`就可以启动IPython了。启动后的画面，大致如下：

![IPython](http://pybooklet.qiniudn.com/c2/ipython.png)

相对于Python交互式解释器，它的命令提示符改变了，然而，并非这一点点改变，IPython里支持通过tab键补全，方向键导航历史命令。我想，这是广大linux或者mac用户都迫切需要的特性。

另外，在IPython里可以直接执行系统命令，基于这个特性，完全可以吧IPython当做一个非常强大的shell环境。

IPython还提供了很多工具函数，例如`help`函数，通过`help`函数，你可以查看帮助文档， 你可以试着在IPython中键入`help(list)`， 它将打印出`list`类的帮助文档。


## 使用源文件
我们在上一节，见识了如何在Python交互式解释器中执行代码，然而，交互式解释器，只是用来让我们快速测试少量代码片段的，它无法保存我们的代码，供以后使用。所以，我们需要把我们的源码保存到文件，供以后使用。

使用文件保存Python代码非常简单，使用你喜欢的文本编辑器创建一个文件：*hello.py*，在文件中键入如下内容：
```python
print "Hello Python"
```

保存退出。然后执行：
```bash
python hello.py
```
到这里，你的第一个保存到文件，可以供以后重复使用的Python程序就诞生了。在这里，有几点需要注意：
* Python的源代码通常以**.py**做为后缀，但这不是必要的。
* Python是解释型语言，也无需像java那样编译成中间代码，所以Python无需编译，直接执行源文件。
* Python对文件格式(缩进)非常敏感，所以当你的程序无法运行的时候，可以关注一下缩进的问题。


### 可执行的Python
Python无需编译，执行的就是源文件本身，事实上，Python执行的是Python解释器，由解释器解析执行Python源文件。在unix下，可以类似bash那样通过**Sha-Bang**技术来实现可直接执行的Python源文件。

> 在计算机科学中，Shebang（也称为Hashbang）是一个由井号和叹号构成的字符串行（#!），其出现在文本文件的第一行的前两个字符。 在文件中存在Shebang的情况下，类Unix操作系统的程序载入器会分析Shebang后的内容，将这些内容作为解释器指令，并调用该指令，并将载有Shebang的文件路径作为该解释器的参数。-- [维基百科](http://zh.wikipedia.org/wiki/Shebang)

所以，如果我们在unix或者类unix系统下，只需要在源码的的第一行插入 
```python
#!/your/path/to/python
```

即可，但是，由于我们使用pyenv来管理Python版本，所以python的路径也是由pyenv来管理的，所以我们可以使用env指令来获取环境变量中得Python路径，使用env指令获取Python路径，只需要把上面的内容改为：
```python
#!/usr/bin/env python
```

然后给脚本可执行的权限即可。执行脚本时，首先调用env命令，env命令接受参数`python`,它会启动Python解释器，并解释执行文件接下来的内容。


## unicode支持
Python是一门支持unicode的语言，我们来看下面的例子：
```python
print "你好！世界！"
```

然而，程序并非如我们所愿的执行，而是抛出一个错误：
```
SyntaxError: Non-ASCII character '\xe4' in file unicode.py on line 1, but no encoding declared; see http://www.python.org/peps/pep-0263.html for details
```

这并非Python不支持unicode，而是因为Python默认使用ASCII编码，所以当它遇到中文的时候，抛出了Non-ASCII异常。要解决这个问题，我们只需要显式的告诉解释器，我们的文件是以什么编码保存的即可。Python通过首行注释来告诉解释器文件的编码，所谓首行注释就是除ShaBang之外的第一行非空白行。我们只要在源文件除ShaBang之外的第一个非空白行加入如下代码:
```python
# coding:utf-8
```
即可，当然，你也可以是用emacs的写法，
```python
#  -*- coding: utf-8 -*-
```
如果你使用emacs作为你的编辑器，用后一种方法，还可以告诉emacs，文件使用utf-8编码。

最后，把你的文件保存成UTF-8编码即可。

**接下来，如无特殊说明，我们统一使用UTF-8编码**


## 选择合适的文本编辑器或IDE
工欲善其事，必先利其器。我无意挑起编辑和IDE的圣战，毕竟一个好的工具，能提升我们的效率。有非常多的编辑器适合写Python，当你挑选编辑器的时候，一下几点可以作为你的参考意见：
* 支持缩进，Python用缩进表示代码块，所以缩进非常重要
* 支持UTF-8
* 支持Python代码高亮，如果你也和我一样喜欢看花花绿绿的代码的话
* 支持tab自动转化为四个空格，如果你也和我一样，喜欢直接用tab来缩进代码的话

满足以上条件的编辑器非常多，如果你在mac下使用vim，建议你看看这个项目，[maximum-awesome](https://github.com/square/maximum-awesome)

至于IDE，我只有一个推荐，那就是Jetbrains出品的PyCharm， 谁用谁知道。

另外，建议初学者不要使用IDE，因为IDE强大的功能，容易分散你的注意力（尽管PyCharm已经做的很好了，可以让你集中精力关注代码，但是其强大的智能补全，会对初学者造成一定的困扰）。

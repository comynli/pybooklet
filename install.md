# 安装Python
绝大多数Python入门书籍，都会介绍如何安装Python，然而，本书要介绍一种全新的安装方式。使用pyenv来安装并管理我们的环境。

## pyenv简介

  由于Python的依赖是基于site的，这多与生产环境来说，是一种简单而正确的方式，然后，对于我们的开发环境，基于这样的管理方式，带来了可怕的第三方依赖管理的难题。想象一下，你在开发一个新项目的同时，还在维护一个就项目， 新项目依赖某个包的较新的版本，旧项目却依赖一个相对较旧的版本，而这两个版本是不兼容的，事实上，这种不兼容，在Python世界里是司空见惯的。

  基于以上难题，virtualenv适时出现了，拯救了广大因依赖问题焦头烂额的Python程序员。virtualenv无疑是成功的，他为每个项目创建一个虚拟环境，使得项目的依赖全部在一个虚拟且封闭的环境中，互不干扰。然而，这就够了吗？

  想象下面一种场景：你的新项目在Python 3.3下面开发，而你维护的旧项目，缺工作在Python 2.7之上，为了完成工作，你不得不安装两个Python解释器，并为每个解释器配置virtualenv，在你调试执行的时候，不得不适用冗长的全路径调用解释器，或者不断的修改你的环境变量。天哪，你再次陷入与你的工作无关的，烦人的事情中。

  pyenv的出现，就是来拯救你的。pyenv是一个Python多版本管理工具，他设计精巧，通过巧妙的方法，可以使多版本的Python共存在一个操作系统能，简单的实现切换，或者更具项目使用不同的Python。

  pyenv支持插件，通过插件，可以和virtualenv完美结合，实现多版本，多环境的控制，是你的每个项目，仿佛运行在一个完全隔离的环境种一样。


## 安装pyenv
pyenv是纯python开发的，安装pyenv只需要极少数的依赖，它们是：：
* Python >= 2.5 < 3
* git

因为pyenv是Python开发的，所以需要有一个可运行的python版本，然而，这个不必担忧，绝大多数linux和unix发行版以及mac都已经预装了python，我们唯一需要做的是安装git，并且把他的路径放到`PATH`中。

如果你能访问Github， 那么安装pyenv是一件简单而快乐的事， 因为pyenv的作者，很贴心的给我们准备了一个安装脚本[pyenv-installer](https://github.com/yyuu/pyenv-installer)，只需要简单的执行就可以了。

使用pyenv-installer安装pyenv很简单，只需要在终端执行:
```bash
curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```
等待一段时间就可以了。安装完成，pyenv-installer会提示你配置pyenv的环境变量，你仅仅需要做的就是根据提示，把如下的内容加入到你的profile里。
```bash
export PYENV_ROOT="\${HOME}/.pyenv"

if [ -d "\${PYENV_ROOT}" ]; then
  export PATH="\${PYENV_ROOT}/bin:\${PATH}"
  eval "\$(pyenv init -)"
fi
```

重新载入profile之后，你就可以开始使用你的pyenv了，使用pyenv-installer安装的pyenv，会帮助我们安装几个有用的插件，其中，最常用的有：

* pyenv-virtualenv 用于整合virtualenv
* pyenv-pip-rehash 用于使用pip安装包之后自动执行rehash
* pyenv-update 用于升级pyenv

## 使用pyenv安装Python

使用pyenv安装Python非常简单，但是在由于需要编译Python，所以我们需要有变异Python的依赖，以CentOS 6为例，我们需要如下依赖：

* gcc
* gcc-c++
* make
* patch
* openssl-devel
* zlib-devel
* readline-devel
* sqlite-devel
* bzip2-devel

你可以使用你喜欢的包管理器来安装这些依赖，例如：

```bash
yum -y install gcc gcc-c++ make patch openssl-devel zlib-devel readline-devel sqlite-devel bzip2-devel
```

完成以上步骤之后，你可以使用pyenv来管理你的Python环境了。安装Python只需要使用`install`命令即可。

```bash
pyenv install 2.7.5
```

以上命令将在你的系统上安装 Python-2.7.5, 使用过linux包管理系统的朋友，对这样的安装方式，是相当亲却的。

`install`命令有若干选项可用，可以通过 `pyenv help install`来查看。常用的，我们可以通过`-l` 选项来查看所有可用版本：

```bash
pyenv install -l
```

讲列出所有可用版本。

*tips：由于使用pyenv安装的时候，需要到github下载Python源码包，国内用户可能速度比较慢，这个时候，可以修改`PYTHON_BUILD_MIRROR_URL`环境变量，使用国内镜像。`http://magedu-python.qiniudn.com/pythons`是我制作的一个镜像，托管在七牛云存储上*

安装完成之后，并不能立刻使用你所安装的Python，因为pyenv作为一个Python环境管理工具，安装只是第一步，你还需要切换到你新的Python版本上。pyenv提供两个命令来切换Python版本。`global`命令和`local`命令。故名思议，一个是全局的，一个是本地的。

在介绍两个切换命令之前，我们先来看其他几个pyenv命令。

### versions和version命令
`versions`命令列出你已经安装的Python版本以及当前使用的版本
```bash
pyenv versions
```

执行以上输出，你将会得到如下的输出：
![pyenv versions](http://pybooklet.qiniudn.com/c1/versions.png)

也许你的输出会有所出入，但是大致相同，前面加`*`号的版本是当前版本，后面括号内的内容描述了它是在何处设置的，后面会详细讲解。

`version`命令打印你当前使用版本。`version`命令的输出类似`versions`命令，但是它只包含了当前版本那一行，并且没有前导的`*`.

版本名称`system`代表系统预装Python。

### global和local命令
`global`命令和`local`命令都是用来切换当前Python版本的命令。不同之处在于，`global`的切换是全局的，而`local`的切换是局部的。

```bash
pyenv local 2.7.5
```

以上命令：会在当前目录下创建一个`.pyenv-version`文件，文件内容为`2.7.5`，pyenv通过这种形式，标记当前目录使用Python-2.7.5。如果其子目录下面没有`.pyenv-version`文件，那么此版本讲继承到子目录。

```bash
pyenv global 2.7.5
```

以上命令：会修改`$PYENV_HOME/version`文件的内容，标记全局Python版本，如何理解全局Python版本，可以认为全局版本是一个默认值，当一个目录及其父目录下面都没有`.python-version`文件的时候，会使用全局版本。


**一般的，我们不修改全局版本，而使用期默认值`system`，因为在unix系统上，很多系统工具依赖于Python，如果我们修改了Python的版本，会造成绝大多数的依赖Python的系统工具无法使用，如果你不小心修改了，也不要紧张，使用`global`命令修改回来就可以了，有时候，你发现部分系统工具无法使用，你也可以看看你当前的Python版本**。



*到这里，你或许已经迫不及待的想要开始了，那么你可以跳过本章之后的部分，直接看第二章，开始你的Python之旅，但是我还是强烈建议你以后回过头来看看本章剩下的部分，他演示了如何使用pyenv结合virtualenv的强大功能。*

### uninstall 命令
顾名思义，uninstall命令是用来卸载Python版本的，一个已经安装的版本，如果我们不在使用，可以简单的执行`uninstall`命令把它从硬盘上卸载。

```bash
pyenv uninstall 2.7.5
```

以上命令将卸载Python-2.7.5


### pyenv与virtualenv
pyenv通过插件，可以很好的和virtualenv一起工作，通过整合virtualenv，pyenv实现了真正意义上的环境隔离，每个项目都相当于使用一个单独的解释器。

通过pyenv-installer安装的pyenv，已经安装好virtualenv插件了，如果不是通过pyenv-installer安装的pyenv，你可能需要自己安装virtualenv插件，安装方法也很简单：
```bash
cd $PYENV_ROOT/plugins
git clone https://github.com/yyuu/pyenv-virtualenv.git
```

直接把插件clone下来就安装完成了。

安装完成之后，我们可以通过virtualenv命令即可创建虚拟环境，virtualenv的一般用法如下：
```bash
pyenv virtualenv [-f|--force] [-u|--upgrade] [VIRTUALENV_OPTIONS] <version> <virtualenv-name>
```

选项`-f`表示强制的，也就是如果已经存在这个虚拟环境，那么将会覆盖这个虚拟环境
选项`-u`表示upgrade，用于修改已经存在的虚拟环境的Python版本
`VIRTUALENV_OPTIONS` 是传递给virtualenv的选项，可以通过virtualenv的帮助获取选项的含义
`version` 表示Python版本
`virtualenv-name` 是我们给虚拟环境指定的名字

例如:
```bash
pyenv virtualenv 2.7.5 my_project
```

以上命令就创建了一个基于Python-2.7.5,名为my_project的虚拟环境。创建好的虚拟环境犹如一个单独Python版本一样，我们可以通过`local`或者`global`命令切换过去。

由于每个解释器间是完全隔离的，所以强烈建议你的每个项目，都放置在单独的虚拟环境中。

virtualenv插件还提供了`virtualenvs`命令，用于列出所有已经创建的虚拟环境，

```bash
pyenv virtualenvs
```

以上命令列出我们所有已经创建的虚拟环境，已经虚拟环境基于那个Python版本。

当我们的一个项目生命周期结束的时候，我们或许会想要删除虚拟环境以释放我们的硬盘空间，删除虚拟环境非常简单，直接用`uninstall`命令像删除正常的Python版本一样就可以了。

事实上，虚拟环境一旦创建，你就可以把他当成一个独立的版本来使用和维护了。
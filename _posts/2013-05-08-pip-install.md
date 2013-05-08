---
layout: post
title: "安装Pip时遇到的几个问题"
description: "学习tornado笔记"
category: python
tags: [python, pip]
---
{% include JB/setup %}

最近下了一个tornado学习，昨天想看看一些现有的基于tornado的开源程序，于是在[http://www.v2ex.com/](http:// "http://www.v2ex.com/")上面找到[几个](http:// "http://www.v2ex.com/t/62732#reply9")，如下：

	F2E.im: https://github.com/PaulGuo/F2E.im
	@paulguo 写的访 v2ex 论坛，虽然没有用 ORM，但数据库的处理方面很干净利落，也非常值得借鉴。

	PoweredSites: https://bitbucket.org/felinx/poweredsites
	@felinx 写的一个收集网站架构信息的站。作为国内使用 Tornado 的先驱，这个项目曾使包括我和 lepture 在内的很多人受益匪浅，不容错过。

	June: https://github.com/pythoncn/june/tree/tornado-master
	@lepture 写的论坛，代码结构严谨优美，非常适合初学者借鉴，其中也用到了他自己写的框架 July

	book.42qu.com：https://bitbucket.org/zuroc/zpage/wiki/Home
	还有很多。。。。与 Tornado 有关的项目: https://github.com/facebook/tornado/wiki/Links

于是我clone了F2E.im源码下来，参考readme开始进行尝试。

里面提到了pip这个包管理工具，用它来安装所需的依赖项，这有些类似于npm或gem。很简单嘛，于是乎,下载源码，安装`python setup.py install` ...呃...貌似在x64下python3,3有些问题....好吧,google `windows安装pip`，[如下](http:// "http://blog.sina.com.cn/s/blog_4e1354b101017tl7.html")
	

	 http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows这里给出了方案
	Download the last pip version from here: http://pypi.python.org/pypi/pip#downloads
	Uncompress it
	Download the last easy installer for Windows: (download the .exe at the bottom ofhttp://pypi.python.org/pypi/setuptools ). Install it.
	go to the uncompressed pip directory and: python setup.py install
	Add your python c:\Python2x\Scripts to the path

好的，那我滚去先安装[setuptools](http:// "https://pypi.python.org/pypi/setuptools")好了...

嗯，看到了...作为我x64党，需要一个[ez_setup.py](http:// "http://peak.telecommunity.com/dist/ez_setup.py")文件，好的，果断`python ez_setup.py`

貌似...嗯...还是有问题，看提示...查源码...哦，泥煤，原来是2.x的代码，作为不pythonic的python，在我3.x是行不通滴...原来我没注意上面写的`install
	Add your python c:\Python2x\Scripts to the path`

好吧，怎么办，继续google吧，经历了各种无解之后，找到了答案，来自一个[国际友人](http:// "http://www.khattam.info/howto-install-easy_install-and-pip-in-python-3-windows-2011-09-27.html")：

I am just starting with Python 3 on Windows and I wanted to install easy_install and/or pip for installing other available packages easily. However, I found that setuptools setup for Python 3.3.2 (the version I am using) is not available.


I discovered distribute, a fork of setuptools, which provides easy_install. I downloaded source from Python Package page for distribute and extracted it. In the elevated command prompt (cmd->Run as Administrator), I changed to extracted directory and then ran distribute_setup.py. Then, easy_install was successfully installed in Python_Directory\Scripts. Then, I could install pip by changing directory to Scripts and running the following:

	easy_install pip

Hope this helps.

**YES,This rally helps!!**

至此，真是喜大普奔啊，总结一下：

## 1. 在windowsx64 python3.x环境下安装pip的方法

	1、安装distribute-0.6.38.tar.gz (md5)，
	   	  地址：https://pypi.python.org/pypi/distribute#downloads
	2、把C:\Python33\Scripts加入环境变量path
    3、执行easy_install pip

那么，为什么要用这种方式来安装呢，或者说为什么一定要安装pip才行呢？easy_install又是个什么东西？他们哪一个更屌一些？带着这些疑惑， 我们敲开了苍老师的门...喂喂，醒醒，工头喊你去搬砖了。


这些问题归结起来就是，第二个话题：

## 2.Python的包管理工具

在上面我们看到了easy_install、setuptools、distribute、pip，这几个工具可以简单的参考下图（不知道谁是作者...）
![](http://microacup.github.io/assets/images/python-pip-distribute.png)

distribute是setuptools的取代，pip是easy_install的取代。实际上，还不止这几种：



- distutils：Python 自带的基本安装工具, 适用于非常简单的应用场景; 使用:

		为项目创建 setup.py 脚本,
		执行 setup.py install 可进行安装

- setuptools：针对 distutils 做了大量扩展, 尤其是加入了包依赖机制. 在部分 Python 子社区已然是事实上的标准;**不支持python3.x**

- distribute：由于 setuptools 开发进度缓慢, 不支持 Python 3, 代码混乱, 一帮程序员另起炉灶, 重构代码, 增加功能, 希望能够取代 setuptools 并被接纳为官方标准库, 他们非常努力, 在很短的时间便让社区接受了 distribute;


- easy_install：setuptools 和 distribute 自带的安装脚本, 也就是一旦 setuptools 或 distribute 安装完毕, easy_install 也便可用. 最大的特点是自动查找 Python 官方维护的包源 PyPI , 安装第三方 Python 包非常方便; 使用:


		setuptools / distribute 都只是扩展了 distutils;

		easy_install [PACKAGE_NAME] 自动从 PyPI 查找/下载/安装指定的包;



- pip :pip 的目标非常明确 – 取代 easy_install. easy_install 有很多不足: 安装事务是非原子操作, 只支持 svn, 没有提供卸载命令, 安装一系列包时需要写脚本; pip 解决了以上问题, 已俨然成为新的事实标准, virtualenv 与它已经成为一对好搭档; 使用:

		安装: pip install [PACKAGE_NAME]
		卸载: pip uninstall [PACKAGE_NAME]
		支持从任意能够通过 VCS 或浏览器访问到的地址安装 Python 包

distutils2 : setuptools 和 distribute 的诞生是因为 distutils 的不济, 进而导致目前分化的状况. 而 Guido 并未接纳 distribute 为官方标准, 并说明了原因. 开发者在失落之余明确了新的方向和任务 – distutils2, 它将成为 Python 3.3 的标准库 packaging , 并在其它版本中以 distutils2 的身份出现; 换句话说, 它和 pip 将联手结束目前混乱的状况;

参考[http://blog.yangyubo.com/2012/07/27/python-packaging/](http://blog.yangyubo.com/2012/07/27/python-packaging/)

###总而言之， bref， 我们成功安装了pip！！！

## 3. 其他 ##

扩展阅读：



1. [Why use pip over easy_install?](http://stackoverflow.com/questions/3220404/why-use-pip-over-easy-install)
2. [Differences between distribute, distutils, setuptools and distutils2?](http://stackoverflow.com/questions/6344076/differences-between-distribute-distutils-setuptools-and-distutils2)

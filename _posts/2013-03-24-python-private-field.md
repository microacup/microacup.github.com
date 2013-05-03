---
layout: post
title: "python学习笔记之私有变量"
description: ""
category: 
tags: [python]
---
{% include JB/setup %}

> 最近看《Python 参考手册》，看到了7.9 数据封装和私有属性，查看了网上几篇文章，记录下来。

#### 首先请看一个例子，并猜测程序输出：

	class Base(object):
		def __init__(self):
			self.__private()
			self.public()
	
		def __private(self):
			print('Base.__private')
		
		def public(self):
			print('Base.public')
	
---
	class Child(Base):
		def __private(self):
			print('Child.__private')
		
		def public(self):
			print('Child.public')

	# wait! I'm 饿死了……
	c = Child()

*话说Markdown写python简直是绝配！*

OK,程序输出是：

	Base.__private
	Child.public

如果你已经清楚这是为什么，`return`

在Python中，类的所有属性和方法默认都是“共有的”。而把一个变量或方法（下统称变量）变为私有，只需要在变量名前加**双下划线**`（如：__private）`

#### name mangling :

什么是name mangling 呢，为了区分子类和父类私有变量，python对私有变量自动名称自动边形（name mangling ）为`_类__变量`的形式。例如：

`print(dir(Child))`

输出：

	['_Base__private', '_Child__private', （略……）, 'public']

这样就避免了同名私有变量冲突的问题。
这个时候可以使用变形之后的名称来调用相应的方法：

	Child._Child__private()

另外这里需要注意

1. 因为名称变形会使标识符变长，当超过255的时候，Python会切断，要注意因此引起的命名冲突。
2. 当类名全部以下划线命名的时候(`class __:pass`)，Python就不再变形。
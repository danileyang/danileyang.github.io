---
layout: post
title: groovy coding new line
date: 2016-06-27 10:13:26
disqus: y
---
# groovy潜规则之换行
		String test = 'the girl is ' + user.age + 'years old'
针对上面一行代码，期待输出结果是the girl is 18 years old //user.age是18

####但是有时候变量太多，我们习惯性的把代码进行换行
    String test = 'the girl is ' + user.age + 'years old'
    + 'name is '+ user.name
    + 'salary is '+ user.salary

######或是这样
    String test = 'the girl is ' + user.age + 'years old' +
    'name is '+ user.name +
    'salary is '+ user.salary

######那么上面两段代码是否都能正常输出我们期望的结果呢

* <font color=Orange size=2>
  +号放语句结尾则会认为语句未结束，进行语句拼接

  +号放换行后的最前面，则上面语句结束，下面为表达式，所以不能输出期望结果</font>

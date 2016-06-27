---
layout: post
title: MYSQL ORDER BY FIELD
date: 2016-06-26 10:13:26
disqus: y
---
# mysql自定义排序

#### mysql按照某个字段升降序排序的时候，我们可以用ORDER BY field ASC || ORDER BY field DESC 以实现多个字段的升序降序排列

#### 但有时候，我们需要安装某个字段指定排序规则进行排序，ORDER BY FIELD就是实现这个功能的
    List<Item> itemList = Item.executeQuery("select o from Item as o where " + "o.status not in (:excludeStatus) order by FIELD(o.status,11,5,21,31,17),o.dateCreated desc", [excludeStatus: excludeStatus], [max: max, offset: offset])

####上例中我们针对status字段按照11，5，21，31，17的顺序进行排序，而后按照dateCreated字段进行降序排序
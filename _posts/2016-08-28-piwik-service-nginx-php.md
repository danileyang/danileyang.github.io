---
layout: post
title: neo4j cypher
date: 2016-07-17 10:13:26
disqus: y
---
#关于Cypher
Cypher使用户可以撇开neo4jDB底层的具体操作，而使用其简单的类SQL语言即可进行复杂的增删改查<br>
![Mou icon](https://s3.amazonaws.com/dev.assets.neo4j.com/wp-content/uploads/cypher_pattern_simple.png)
##Node
Cypher使用'()'包括节点以表示node，通常node相关的labels会同时使用已做节点的区分和执行的优化. eg.（p:Person）
##Relationships
为了充分发挥数据库的最大能力，node之间可以建立复杂的关系。relationship就是两个节点间的-->. 箭头上可以添加额外信息.<br/>
####eg.<br/>
* 关系类型  -[:KNOWS|:LIKE]->
* 关系限定变量  -[rel:KNOWS]->
* 额外属性 -[{since:2010}]->
* 关系深度 -[:KNOWS*..4]->

##example
######创建一个node (label:Person, name:you)<br/>
 CREATE (me:Person {name:"you"})
######添加几个朋友
MATCH (you:Person {name:"You"})
FOREACH (name in ["Johan","Rajesh","Anna","Julia","Andrew"] |
  CREATE (you)-[:FRIEND]->(:Person {name:name}))

######给朋友添加朋友
match(iori{name:"iori"}) create (jams:Person{name:"jams"}) create (iori)-[:FRIEND]->(jams)

……

经过朋友的添加和朋友关系的添加，我们得到如下的关系网络
![Mou icon](http://7fvcu1.com1.z0.glb.clouddn.com/1.pic.jpg)
<br/>
如上图关系，我们不但为你添加了好友，还为你的好友Julia添加了好友...并且还让你的朋友WORKED_WITH Neo4j

######那么接下来我们看看谁能帮助你学习Neo4j
当然朋友的朋友是朋友，但是层数太多好像就不太愿意帮助了，那么我们找3级以内的吧<br/>
MATCH (you {name:"You"})
MATCH (expert)-[:WORKED_WITH]->(db:Database {name:"Neo4j"})
MATCH path = shortestPath( (you)-[:FRIEND*..3]-(expert) )
RETURN db,expert,path
<br/>
![Mou icon](http://7fvcu1.com1.z0.glb.clouddn.com/6.pic.jpg)
<br/>
呐，现在已经为你找出可以帮助你学习Neo4j的朋友啦~

######附其他一些操作
删除anna所有朋友同事删除其关系<br/>
Match(ana {name:"Anna"})-[r:FRIEND]->(foa) delete r,foa

为现有node添加label<br/>
match(dan{name:"daniel"}) set dan:expert return dan

添加daniel worked_with neo关系<br/>
MATCH (neo:Database {name:"Neo4j"})
MATCH (daniel:Person {name:"daniel"})
create (daniel)-[:WORKED_WITH]->(neo)

新建一个node同时设置多个label标签<br/>
CREATE (me:Person:expert {name:"iori"})

新建一个node并添作为一个现有用户的朋友<br/>
match(iori{name:"iori"}) create (jams:Person{name:"jams"}) create (iori)-[:FRIEND]->(jams)
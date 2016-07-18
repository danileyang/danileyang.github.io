---
layout: post
title: grails multi DB
date: 2016-07-18 15:13:26
disqus: y
---
#grails连接多数据库

    **由于数据安全的原因，需要把一部分数据做加密，或者物理隔绝，这个时候阿里云提供了很好的功能，数据库的TDE。其对数据库的透明加密解密，解放了程序员的生产力，提高了工作效率，用户无需在应用代码层面做相关的操作**

    ####需要做的是使工程连接多个数据库即可(一个是正常数据库，一个为配置了TDE的数据库)
    针对grails工程，我们需要做一下配置：



    ## dataSource:
    		s1:
            pooled: true
            jmxExport: true
            driverClassName: com.mysql.jdbc.Driver
            dbCreate : "validate" # one of 'create', 'create-drop', 'update', 'validate', ''
            loggingSql : false
        s2:
            pooled: true
            jmxExport: true
            driverClassName: com.mysql.jdbc.Driver
            dbCreate : "validate" # one of 'create', 'create-drop', 'update', 'validate', ''
            loggingSql : false


    ##enviroments:
    		s1:
    			username : "***"
        	    password : "***"
            	url : "jdbc:mysql://***\"
            s2:
            	username : "***"
                password : "***"
                url : "jdbc:mysql://***"
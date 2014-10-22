---
layout: post
title: python读取oracle数据库中文乱码问题解决
description: python读取oracle数据库中文乱码问题解决
category: blog
---

##1、cx_Oracle中文乱码问题

oracle的数据集是SIMPLIFIED CHINESE_CHINA.WE8ISO8859P1
编写python脚本时，需要加入如下几句，保证模块的编码与数据库的编码一致

    import os
    os.environ['NLS_LANG']='SIMPLIFIED CHINESE_CHINA.WE8ISO8859P1'

##2、python中的str和unicode

str是一个字节数组，表示对unicode编码（可以是utf-8、gbk、cp936、GB2312）后的存储格式。这里它仅仅是一个字节流，没有其它的含义，如果你想使这个字节流显示的内容有意义，就必须用正确的编码格式，使用decode解码成unicode
例子如下：

    #-*-coding:ISO-8859-1-*-
    import os
    os.environ['NLS_LANG']='SIMPLIFIED CHINESE_CHINA.WE8ISO8859P1'
    import cx_Oracle
    tns_name=cx_Oracle.makedsn('10.233.85.205','1521','CBMS1')
    print tns_name
    conn=cx_Oracle.connect('cbms','c3cbms678',tns_name)
    cursor=conn.cursor()
    cursor.execute('select * from tmp_hhh')
    filename='list.txt'
    fi=open(filename,'w')
    for row in cursor:
        tmp_row=''
        for i in xrange(len(row)):
            if(i==len(row)-1):
                tmp_row=tmp_row+str(row[i])+'!|\n'
            else:
                tmp_row=tmp_row+str(row[i]).decode('GBK','ignore').encode('utf8','ignore')+'|!'
    fi.write(tmp_row)
    fi.close()
    cursor.close()
    conn.close()
思路：

1.将cx_Oracle模块的编码与数据库的编码保持一致
2.将数据库中读出的中文（str类型）使用GBK进行解码
3.再将解码后的中文使用utf8进行编码，编码成了txt文件可以表达的格式




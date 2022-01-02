> 大量SYN_RECV，80端号占死解决方法 ：http://blog.sina.com.cn/s/blog_6915bcb101013st7.html



**Linux增加开放端口号 :**https://www.cnblogs.com/jjxhp/p/11111898.html

```shell
#查看开放的端口列表 
firewall-cmd --list-ports 
firewall-cmd --zone=public --add-port=5121/tcp --permanent  
firewall-cmd --reload
```

--zone 作用域

--add-port=5121/tcp 添加端口，格式为：端口/通讯协议 

--permanent 永久生效，没有此参数重启后失效 



**方法一：命令行方式**

  \1. 开放端口命令： /sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
  2.保存：/etc/rc.d/init.d/iptables save
  3.重启服务：/etc/init.d/iptables restart
  4.查看端口是否开放：/sbin/iptables -L -n**
**

**方法二：直接编辑/etc/sysconfig/iptables文件**

  1.编辑/etc/sysconfig/iptables文件：vi /etc/sysconfig/iptables
  加入内容并保存：-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
  2.重启服务：/etc/init.d/iptables restart
  3.查看端口是否开放：/sbin/iptables -L -n
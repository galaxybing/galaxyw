# 备份 confluence 服务：服务器环境安装步骤 + confluence以及相关插件版本 + application_data + confluence.sql

## 一、CentOS 服务器环境

```bash
# /etc/redhat-release
# /etc/centos-release
# /etc/issue
[root@iZ2332uqfdcZ ~]# cat /etc/issue
CentOS release 6.7 (Final)
Kernel \r on an \m

# 安装jdk8环境（注意confluence和jira环境，最好安装oracle的java,默认的openjdk是不行的。）
# vim /etc/profile
# source /etc/profile
$ java -version

```


# 二、mysql 数据库创建，以及连接属性 - galaxyw-confluence

```bash
# 创建 confluence 数据库 + 并将所有权限授予 confluence 用户
#
mysql -uroot -p;
-> 密码
create database confluence default character set utf8 collate utf8_bin;
grant all on confluence.* to 'confluence'@'%' identified by '密码';

# confluence3 + 导入备份 sql文件： confluence-190418.sql
create database confluence3 default character set utf8 collate utf8_bin;
grant all on confluence3.* to 'confluence'@'%' identified by '密码';

#查看用户
>use mysql;
>select host,user from mysql.user;
+--------------+------------+
| host         | user       |
+--------------+------------+
| %            | confluence |
| %            | galaxyw    |
| %            | ghost      |
| 127.0.0.1    | root       |
| ::1          | root       |
| iz2332uqfdcz | root       |
| localhost    | root       |
+--------------+------------+
7 rows in set (0.00 sec)

#创建用户
> create user 'confluence'@'%' identified by 'confluence';

```

# 三、confluence 服务安装 - https://www.cnblogs.com/kevingrace/p/7607442.html

```
1. [JIRA 插件列表](http://www.confluence.cn/pages/viewpage.action?pageId=1671327)
2. [管理插件](https://marketplace.atlassian.com/apps/23915/atlassian-universal-plugin-manager?hosting=server&tab=overview)的查找、安装、升级、授权等，也可用于Confluence，FishEye，Bamboo，Crucible 及Stash


* [个人知识管理的 wiki 系统](https://www.zhihu.com/question/19716095)
* [dokuwiki](https://blog.csdn.net/wszxs1990/article/details/55259058)
* [mediawiki](https://www.mediawiki.org/wiki/Manual:Installation_guide/zh)
  - [git-hub/wikimedia/mediawiki](https://github.com/wikimedia/mediawiki)
  - [中文文档空间](https://www.cwiki.us/#all-updates)
  - [Database Setup For MySQL](https://www.cwiki.us/display/CONFLUENCEWIKI/Database+Setup+For+MySQL)

* Atlassian生态理解
  - [概念 + 常见操作](https://www.cnblogs.com/ZachRobin/p/6962062.html)
  - [由 Atlassian 合流 5.10.8驱动](http://share.317hu.com/index.action#app-switcher)
  - [基于 Atlassian Confluence 6.9.0 技术构建](http://wiki.chenjiehua.me/pages/viewpage.action?pageId=2458216)
  - [团队协同与知识管理](http://www.unlimax.com/confluence.html)
  - [confluence，JAVA，10 人团队，一年$10，机器配置不能太低](https://www.atlassian.com/purchase/product/confluence)
  - [10 人一下 $10 的 License 是永久授权（ Perpetual ）并且包含一年的维护（ maintenance ）](https://www.v2ex.com/t/457010)
  - [Confluence使用java和内置的tomcat，占用内存较多，需要1.5G以上的内存](https://my.oschina.net/nosand/blog/273155)
    * 2 核 4G 的服务器，confluence 6.3 一启动直接占用 1.8G+内存 加上 mysql 500m+
    * 一样的配置，在线人数 60 左右，启动后几分钟内会卡顿，过后就非常流畅了。

[本站点](http://www.confluence.cn/dashboard.action)由Atlassian白金级和企业级专家及合作伙伴--Unlimax维护更新，使用Confluence系统搭建，为用户提供Atlassian产品中文文档和Confluence演示试用平台。

[破解工具只识别这个文件名](http://www.cnblogs.com/mxmbk/p/9347359.html)

```


ls -arlt /opt/atlassian/confluence/confluence/WEB-INF/lib/

```bash
#数据库连接插件 - [安装目录]/atlassian/confluence/lib/
mysql-connector-java-5.0.8-bin.jar
#confluence插件管理 - [安装目录]/atlassian/confluence/confluence/WEB-INF/atlassian-bundled-plugins/
atlassian-universal-plugin-manager-plugin-*

#atlassian-extras-decoder-解码器.jar - [安装目录]/atlassian/confluence/confluence/WEB-INF/lib/
#不需要了；而是直接使用下面的来破解
atlassian-agent.jar

#confluence附件资源目录，备份 - 默认为 /var/atlassian/application-data/，可自定义
/application-data/
#mysql数据库：confluence，备份
confluence-190418.sql

```


### 1）以二进制文件进行安装： atlassian-confluence-X.X.X-x64.bin

[Install Confluence using an installer](https://confluence.atlassian.com/doc/confluence-installation-guide-135681.html)

```bash
$ cd /usr/local
$ wget https://product-downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.10.1-x64.bin
$ wget https://product-downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.15.2-x64.bin
$ chmod +x atlassian-confluence-6.10.1-x64.bin
$ ./atlassian-confluence-6.10.1-x64.bin
#安装目录内，将创建以下目录：
# ls -a /opt/ 将出现： /opt/atlassian/
# /etc/init.d/ 将出现： /etc/init.d/confluence 可执行文件
# ls -a /var/ 将出现： /var/atlassian
# ls -a /home/confluence3 将自动创建： confluence 用户
#
# 使用默认的端口，8090和8000

初次，不要启动！！

```

### 2）使用atlassian-agent.jar破解服务 - https://github.com/pengzhile/atlassian-agent/blob/master/README.md

- 配置Agent

```bash
#找到，或者下载： 
$ find / -name atlassian-agent.jar 2>>/dev/null
-> /usr/local/atlassian-agent/atlassian-agent.jar

#/opt/atlassian/confluence/bin/setenv.sh 
CATALINA_OPTS="-javaagent:/usr/local/atlassian-agent/atlassian-agent.jar ${CATALINA_OPTS}"

```

- 使用KeyGen

- 热心网友教程（感谢原作者，侵删！）
* [confluence 安装及破解 - centos7](https://www.qinjj.tech/2019/01/04/confluence%20install/)
* [【黑科技】Atlassian产品线全破解](https://tech.cuixiangbin.com/?p=1248)
* [通过Docker安装JIRA和Confluence（破解版）](https://www.jianshu.com/p/b95ceabd3e9d)
* [通过Docker安装JIRA和Confluence（破解版）](https://my.oschina.net/wuweixiang/blog/3014644)


### 3）拷贝 MYSQL 驱动文件包，到安装目录

```bash
ls -alrt /mnt/mysql-connector-java-5.1.42-bin.jar
ls -alrt /opt/atlassian/confluence/confluence/WEB-INF/lib/
cp /mnt/mysql-connector-java-5.1.42-bin.jar /opt/atlassian/confluence/confluence/WEB-INF/lib/
```


### 4) 使用 confluence3 用户启动 + 配置

1. 产品安装

```bash
$ su - confluence3 # 切换到 confluence3 账号 （由于，我是第3次执行 ./atlassian-confluence-6.10.1-x64.bin ）
$ /etc/init.d/confluence3 start

$ /etc/init.d/confluence3 stop
# 第二次安装时，将出现的是带 1 的可执行文件？？
# /etc/init.d/confluence1
# 防火墙端口 centos 7
# firewall-cmd --add-port=8090/tcp --permanent
# firewall-cmd --add-port=8000/tco --permanent
# firewall-cmd --reload
$ /etc/init.d/confluence3 start
# 开启
# /etc/init.d/confluence start
# 查看启动状态：
netstat -ntpl | grep 8090
lsof -i:8090


# # 访问confluence：http://112.124.1.70:8090 ，界面
# # 1. Set up Confluence 选择中文
# # 2. 勾选`产品安装`，下一步
# # 3. 获得插件跳过，下一步
# # 4. 授权界面，得到`服务器Id` - serverId
BB8O-TS12-WLRF-QJ2I
 
#tail -f /mnt/atlassian/application-data/confluence/logs/atlassian-confluence.log
/mnt/atlassian/application-data/confluence
/var/atlassian/application-data/confluence

#破解 confluence
java -jar /home/confluence/atlassian/atlassian-agent.jar -p conf -m galaxybing@hotmail.com -n galaxyw -o http://noonteam.com -s BB8O-TS12-WLRF-QJ2I
#破解 团队日程表
java -jar /home/confluence/atlassian/atlassian-agent.jar -p tc -m galaxybing@hotmail.com -n galaxyw -o http://noonteam.com -s BB8O-TS12-WLRF-QJ2I

将上面生成的`license`拷贝出来 ，填入界面输入框。

```

2. 是否选择插件

3. `license`输入序列号

4. 我自己的数据库

5. 配置管理员信息

6. 一般配置> 授权细节

7. 安装及破解confluence插件

- [license](license-product-support.png)


### 5) 更多配置

```bash
# 配置数据目录 - `/opt/atlassian/confluence/confluence/WEB-INF/classes/confluence-init.properties`
 
# - /var/atlassian/application-data/confluence 更新成为： /mnt/atlassian/application-data/confluence
# - confluence-init.properties 文件，在末尾去除注释并修改为上面设置的confluence家目录路径
 
$ /etc/init.d/confluence2 stop
# 使用 root 用户新建 目录：
$ mkdir -p /mnt/atlassian/application-data/
$ mkdir -p /mnt/atlassian/application-data/confluence
$ chown -R confluence2.root /mnt/atlassian/application-data/confluence
 
# 或者
$ cp -af /var/atlassian/application-data/ /mnt/atlassian/
$ \cp -af /var/atlassian/application-data/ /mnt/atlassian/ # 加了反斜杠，可以为强制覆盖
 
# 配置目录
$ confluence.home=/mnt/atlassian/application-data/confluence
$ /etc/init.d/confluence2 start # restart
$ lsof -i:8090
$ ls -al /mnt/atlassian/application-data/confluence # 查看目录及权限是否正确
 
# 所以，两个存传日志的地方：
$ tail -f /mnt/atlassian/application-data/confluence/logs/atlassian-confluence.log
$ tail -f /opt/atlassian/confluence/logs/catalina.out

```

2）配置资源上传目录 - /mnt/atlassian/application-data/confluence/confluence.cfg.xml

- /var/atlassian/application-data/confluence/confluence.cfg.xml

```bash
- du -sh /mnt/atlassian/application-data/confluence
# 配置 附件目录 - https://www.aityp.com/confluence%E8%BF%81%E7%A7%BB%E9%99%84%E4%BB%B6%E7%9B%AE%E5%BD%95/
- cat /mnt/atlassian/application-data/confluence/confluence.cfg.xml
# 迁移附件目录
- cp -rf /mnt/atlassian/application-data/confluence/attachments/* /data/confluence_attachments
# ?? /mnt/atlassian/application-data/confluence
# confluence.cfg.xml
# <property name="attachments.dir">${confluenceHome}/attachments</property>
<property name="attachments.dir">/data/confluence_attachments</property>

```

3）清理插件缓存：How to clear Confluence plugins cache

```bash
<confluence-home>/bundled-plugins
<confluence-home>/plugins-cache
<confluence-home>/plugins-osgi-cache
<confluence-home>/plugins-temp
<confluence-home>/bundled-plugins_language

```

4）处理报错：A system error has occurred — our apologies!

- caused by: java.lang.IllegalStateException: Container is not setup

```bash
# 配置插件管理包
$ scp kevingrace/atlassian-universal-plugin-manager-plugin-2.22.jar root@112.124.1.70:/opt/atlassian/confluence/confluence/WEB-INF/lib/
# 然后，启动(程序可能会其他报错？)
$ /etc/init.d/confluence2 start
# 需要移除该插件包
$ /etc/init.d/confluence2 stop
$ rm -rf /opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-universal-plugin-manager-plugin-2.22.jar
# 再次启动，并清除浏览器缓存
/etc/init.d/confluence2 start

```

5）？？如何，查看报错日志 - https://cloud.tencent.com/developer/article/1025836


6) [在您系统中的线程限制健康检查失败了](https://confluence.atlassian.com/kb/health-check-thread-limit-858578685.html)

```bash
# /etc/security/limits.conf
# 配置 confluence 运行账号的 最大进程数，添加以下配置：
#
confluence2 soft nofile 4096
confluence2 hard nofile 8192
confluence2 soft nproc 4096 
confluence2 hard nproc 8192
```

7) [数据库 检查](https://confluence.atlassian.com/confkb/exceeds-max-allowed-packet-for-mysql-179443425.html)

```bash
$ /etc/init.d/mysqld stop
$ /etc/init.d/confluence2 stop

# /usr/local/mysql/my.cnf
[mysqld]
...
max_allowed_packet = 256M

$ /etc/init.d/mysqld start
# Restart your Confluence instance.
$ /etc/init.d/confluence2 start
```

8) [InnoDB 日志文件大小](https://confluence.atlassian.com/confkb/mysqlsyntaxerrorexception-row-size-too-large-658735905.html)

```bash
$ /etc/init.d/mysqld stop
$ /etc/init.d/confluence2 stop

# /usr/local/mysql/my.cnf
innodb_log_file_size = 2048M

$ find / -name ib_logfile* 2>>/dev/null
$ ls -al /data/mysqldb/
# 查看是不是最新的 数据包，里面含有：
# drwx------ 2 mysql mysql 12288 Aug 5 10:58 confluence

# 删除 ib_logfile
$ rm -rf /data/mysqldb/ib_logfile0
$ rm -rf /data/mysqldb/ib_logfile1

$ /etc/init.d/mysqld start
$ /etc/init.d/confluence2 start
```

9）更改 confluence 登录会话时长

<!-- /opt/atlassian/confluence/conf/web.xml -->
<session-config>
  <!-- 设置 8个小时过期 -->
  <session-timeout>480</session-timeout>
</session-config>
 
 
<!-- /opt/atlassian/confluence/confluence/WEB-INF/web.xml -->
<session-config>
  <!-- 设置 8个小时过期 -->
  <session-timeout>480</session-timeout>
  <tracking-mode>COOKIE</tracking-mode>
</session-config>


10）[每日备份](https://confluence.atlassian.com/conf610/configuring-backups-952623096.html#ConfiguringBackups-EnablingBackupPathConfiguration)
- Disable automatic backups
  - 预定作业
    - 备份系统 【禁用】它

***

11）[使用 docker 搭建](http://arvon.top/2018/04/14/%E4%BD%BF%E7%94%A8Docker%E6%90%AD%E5%BB%BAJira%E5%92%8CConfluence%E7%B3%BB%E7%BB%9F/)
  - http://www.cnblogs.com/mxmbk/p/9347359.html

12）[confluence与jira账号对接](https://cloud.tencent.com/developer/article/1025836)

13）开启游客登录模式？？权限

14）配置 wiki.noonteam.com 二级域名
* [在腾讯云/万网中加一条A类记录](https://www.cnblogs.com/rynxiao/p/9080179.html)


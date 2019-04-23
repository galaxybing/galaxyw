
（由于，我是第3次执行 ./atlassian-confluence-6.10.1-x64.bin）

```bash
#1）执行安装
$ cd /usr/local
$ wget https://product-downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.10.1-x64.bin
$ wget https://product-downloads.atlassian.com/software/confluence/downloads/atlassian-confluence-6.15.2-x64.bin
$ chmod +x atlassian-confluence-6.10.1-x64.bin
$ ./atlassian-confluence-6.10.1-x64.bin

Select the folder where you would like Confluence 6.10.1 to be installed,
then click Next.
Where should Confluence 6.10.1 be installed?
[/opt/atlassian/confluence]

#安装目录内，将创建以下目录：
# ls -a /opt/ 将出现： /opt/atlassian/
# /etc/init.d/ 将出现： /etc/init.d/confluence3 可执行文件
# 
# 注意重装时，设置 Default location for Confluence data 为： 
[/var/atlassian/application-data/confluence]
-> /mnt/atlassian/application-data/confluence
# ls -a /mnt/ 将出现： /mnt/atlassian
# 
# ls -a /home/confluence3 将自动创建： confluence 用户
#
# 使用默认的端口，8090和8000

初次，不要启动！！


#2）找到，或者下载： 
$ find / -name atlassian-agent.jar 2>>/dev/null
-> /usr/local/atlassian-agent/atlassian-agent.jar

#/opt/atlassian/confluence/bin/setenv.sh 
#添加破解程序的包路径
CATALINA_OPTS="-javaagent:/usr/local/atlassian-agent/atlassian-agent.jar ${CATALINA_OPTS}"
#设置 jvm 内存可使用的范围
#-XX:PermSize=128m -XX:MaxPermSize=512m
#
#CATALINA_OPTS="-Xms2048m -Xmx3072m -XX:+UseG1GC ${CATALINA_OPTS}"
#移除：UseG1GC， 新增：PermSize
CATALINA_OPTS="-Xms256m -Xmx2048m -XX:PermSize=128m -XX:MaxPermSize=512m ${CATALINA_OPTS}"


#3）拷贝 MYSQL 驱动文件包，到安装目录
ls -alrt /mnt/mysql-connector-java-5.1.42-bin.jar
ls -alrt /opt/atlassian/confluence/confluence/WEB-INF/lib/
cp /mnt/mysql-connector-java-5.1.42-bin.jar /opt/atlassian/confluence/confluence/WEB-INF/lib/

#4）启动
$ su - confluence3 # 切换到 confluence3 账号 （由于，我是第3次执行 ./atlassian-confluence-6.10.1-x64.bin ）
$ /etc/init.d/confluence3 start

```

## confluence.cfg.xml 内含有 - cat /mnt/atlassian/application-data/confluence/confluence.cfg.xml

- license，包括 confluence服务、各类插件的使用密钥，都不用更新；
- database
- 

tar -zcvf mnt-atlassian-bak-190418.tar.gz mnt-atlassian-bak-190418

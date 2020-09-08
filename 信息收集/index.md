## 一、被动探测



### 搜索互联网公开数据

https://censys.io
https://shodan.io
https://viz.greynoise.io/table
https://zoomeye.org
https://wigle.net
https://publicwww.com
https://hunter.io
https://haveibeenpwned.com
https://pipl.com
https://osintframework.com



### Google Hack

#### 资源泄露

```
site:baidu.com ext:doc | ext:docx | ext:odt | ext:rtf | ext:sxw | ext:psw | ext:ppt | ext:pptx | ext:pps | ext:csv
```

#### 列文件

```
site:baidu.com intitle:index.of
```

#### 配置文件泄露

```
site:baidu.com ext:xml | ext:conf | ext:cnf | ext:reg | ext:inf | ext:rdp | ext:cfg | ext:txt | ext:ora | ext:ini | ext:env
```

#### 数据库文件泄露

```
site:baidu.com ext:sql | ext:dbf | ext:mdb
```

#### 日志文件泄露

```
site:baidu.com ext:log
```

#### 备份文件泄露

```
site:baidu.com ext:bkf | ext:bkp | ext:bak | ext:old | ext:backup
```

#### 后台查找

```
site:baidu.com inurl:login | inurl:signin | intitle:Login | intitle:"sign in" | inurl:auth
```

#### 数据库错误

```
site:baidu.com intext:"sql syntax near" | intext:"syntax error has occurred" | intext:"incorrect syntax near" | intext:"unexpected end of SQL command" | intext:"Warning: mysql_connect()" | intext:"Warning: mysql_query()" | intext:"Warning: pg_connect()"
```

#### php错误

```
site:baidu.com "PHP Parse error" | "PHP Warning" | "PHP Error"
```

#### phpinfo查找

```
site:baidu.com ext:php intitle:phpinfo "published by the PHP Group"
```

#### 其它网站泄露信息

```
site:pastebin.com | site:paste2.org | site:pastehtml.com | site:slexy.org | site:snipplr.com | site:snipt.net | site:textsnip.com | site:bitpaste.app | site:justpaste.it | site:heypasteit.com | site:hastebin.com | site:dpaste.org | site:dpaste.com | site:codepad.org | site:jsitor.com | site:codepen.io | site:jsfiddle.net | site:dotnetfiddle.net | site:phpfiddle.org | site:ide.geeksforgeeks.org | site:repl.it | site:ideone.com | site:paste.debian.net | site:paste.org | site:paste.org.ru | site:codebeautify.org  | site:codeshare.io | site:trello.com "348560"
```

#### 代码网站包含

```
site:github.com | site:gitlab.com "baidu.com"
```

#### stackoverflow包含

```
site:stackoverflow.com "baidu.com"
```

#### 注册页面

```
site:baidu.com inurl:signup | inurl:register | intitle:Signup
```

## 二、主动探测

nmap Ping扫描
```
nmap -sP 192.168.0.0/24
```
nmap 快速扫描开放端口
```
nmap -F --open 192.168.0.0/24
```
nmap 全端口扫描
```
nmap -p 1-65535 -sV -sS -T4 192.168.0.0/24
```
nmap 扫描结果传递给nikto
```
nmap -p80,443 192.168.0.0/24 -oG - | nikto.pl -h -
```

nmap 路由跟踪

```
nmap --traceroute <target ip> 
```

nmap 操作系统类型的探测

```
nmap -O <target ip> 
```


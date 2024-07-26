+++
title = "乱七八糟:Windows单级菜单/开机启动/关闭更新"
date = 2023-08-26

[taxonomies]
tags = ["乱七八糟"]
+++

前言 Windows操作系统作为全球最为普及的桌面操作系统之一，其用户界面的设计非常经典；而win11中的二级菜单令人感到无语，本文教你回到一级菜单。

<!-- more -->

## **使用CMD恢复完整右键菜单**

Win11的`显示更多选项`怎么设置才能将其关闭,并恢复成Win10的状态呢？系统内置的命令提示符（CMD）可以帮助我们完成这一任务，另外请注意，此操作仅适用于CMD，并不适用于Windows PowerShell。

**步骤1.** 按**Win+S**打开搜索框，输入**cmd**并以管理员身份运行命令提示符。

**步骤2.** 输入以下命令并按**Enter**键执行。

```
reg add HKCU\Software\Classes\CLSID{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32 /ve /d “” /f
```
或者
```
reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
taskkill /f /im explorer.exe 
start explorer.exe
```
注意：如果您想要重新打开Win11新样式的右键菜单的话，以同样的方式在命令提示符中执行此命令：
```
reg delete "HKCU\Software\Classes\CLSID{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f
```
## **Win11添加开机自启动项方法**

选择“开始”按钮 ，然后滚动查找你希望在启动时运行的应用。

右键单击该应用，选择“更多”，然后选择“打开文件位置”。此操作会打开保存应用快捷方式的位置。如果没有“打开文件位置”选项，这意味着该应用无法在启动时运行。

文件位置打开后，按win+ R，键入“shell:startup”，然后选择“确定”。这将打开“启动”文件夹。

将该应用的快捷方式从文件位置复制并粘贴到“启动”文件夹中,即添加启动项成功。

## **Win11关闭自动更新**

方案一：使用Windows设置关闭Win11更新
在Windows系统设置中，也有一个设置可以暂时关闭Win11更新，让我们一起来看看吧！

1. 按Win+I打开Windows设置页面。

2. 单击“更新和安全”>“Windows更新”，然后在右侧详情页中选择“暂停更新7天”选项即可在此后7天内关闭Windows更新。

暂停更新7天
 
方案二：使用注册表编辑器关闭Win11更新
Windows注册表实质上是一个庞大的数据库，存储着各种各样的计算机数据与配置，我们可以通过编辑注册表来解决一些很难搞定的计算机问题，比如彻底关闭Win11更新。

1. 按Win+R输入regedit并按Enter键打开注册表编辑器。

2. 导航到此路径：HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows。

3. 右键单击Windows文件夹，选择“新建”>“项”，然后将其命名为“WindowsUpdate”。

新建项WindowsUpdate
 
4. 右键单击新建的WindowsUpdate文件夹，选择“新建”>“项”，然后将其命名为“AU”。

5. 在新建的AU文件夹右侧空白页面中右键单击并选择“新建”>“DWORD（32位）值”，然后将其命名为“NoAutoUpdate”。

新建DWORD（32位）值并将其命名为NoAutoUpdate
 
6. 双击新建的NoAutoUpdate，在弹出窗口中将其数值数据从0更改为1，然后单击“确定”。

更改NoAutoUpdate数值数据为1
 
7. 关闭注册表编辑器，重启计算机即可彻底关闭Windows更新。

方案三：使用任务计划程序关闭Win11更新
任务计划程序可以帮助用户实现对任务进行记录、安排、追踪和完成的功能，我们可以通过它来关闭windows更新。

1. 右键单击“此电脑”，点击“管理”。

2. 导航到此路径：“任务计划程序” > “任务计划程序库” > “Microsoft” > “Windows” > “WindowsUpdate”。

3. 右键单击“Scheduled Start”任务，然后点击“禁用”即可停止Win11更新。

使用任务计划程序关闭Win11更新

方案四：使用组策略编辑器关闭Win11更新
组策略是管理员为计算机和用户定义的，用来控制应用程序、系统设置和管理模板的一种机制，通俗一点说，即为介于控制面板和注册表之间的一种修改系统、设置程序的工具。当然，我们也可以通过本地组策略编辑器来关闭Win11更新。

1. 按Win+R输入gpedit.msc并按Enter键打开本地组策略编辑器。

2. 转到此路径：本地计算机策略>计算机配置>管理模板>Windows组件>Windows更新>适用于企业的Windows更新。

转到适用于企业的Windows更新文件夹
 
3. 双击此文件夹下的“选择目标功能更新版本”设置。

4. 在弹出窗口中将其配置为“已启用”，在左下方长条框中填入“20H1”（或者其他您想停留的Windows10版本），然后单击“应用”>“确定”即可。

配置选择目标功能更新版本设置以关闭Win11更新
 
5. 关闭本地组策略编辑器，重启计算机即可彻底停止Win11更新。

方案五：使用本地服务关闭Win11更新
本地服务是汇集整个计算机上全部服务的一个集合，我们可以在这里对指定的Windows服务项进行启用、停止或禁用配置，关闭Win11更新也不例外。

1. 按Win+R输入services.msc并按Enter键打开服务页面。

2. 在右侧列表中找到“Windows Update”选项，双击进入详细属性页面，将其启动类型配置为“禁用”，然后单击“应用”>“确定”即可关闭Windows自动更新。

方案六：使用第三方软件关闭更新

[Windows Update Blocker](https://www.sordum.org/9470/windows-update-blocker-v1-8/)

[Dism++](https://www.majorgeeks.com/files/details/dism.html)
 
方案七：使用脚本彻底关闭更新
```
::Windows auomatic updates
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU /v AutoInstallMinorUpdates /t REG_DWORD /d 1 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU /v NoAutoUpdate /t REG_DWORD /d 1 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU /v AUOptions /t REG_DWORD /d 4 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update" /v AUOptions /t REG_DWORD /d 4 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate /v DisableWindowsUpdateAccess /t REG_DWORD /d 0 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate /v ElevateNonAdmins /t REG_DWORD /d 0 /f
reg add HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer /v NoWindowsUpdate /t REG_DWORD /d 1 /f
reg add "HKLM\SYSTEM\Internet Communication Management\Internet Communication" /v DisableWindowsUpdateAccess /t REG_DWORD /d 0 /f
reg add HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\WindowsUpdate /v DisableWindowsUpdateAccess /t REG_DWORD /d 0 /f
sc stop wuauserv
sc config wuauserv start=disabled
sc stop WaaSMedicSvc
sc config WaaSMedicSvc start=disabled
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\InstallService\State /v AutoUpdateLastSuccessTime /t REG_SZ /d "2100-01-01T00:00:00+08:00" /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings /v PauseFeatureUpdatesStartTime /t REG_SZ /d "2100-01-01T00:00:00Z" /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings /v PauseQualityUpdatesStartTime /t REG_SZ /d "2100-01-01T00:00:00Z" /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings /v PauseUpdatesExpiryTime /t REG_SZ /d "2100-01-01T00:00:00Z" /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings /v PauseFeatureUpdatesEndTime /t REG_SZ /d "2100-01-01T00:00:00Z" /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings /v PauseQualityUpdatesEndTime /t REG_SZ /d "2100-01-01T00:00:00Z" /f
```
将以上命令保存为.bat文件，运行即可。
```
::Windows auomatic updates
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU /v AutoInstallMinorUpdates /t REG_DWORD /d 0 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU /v NoAutoUpdate /t REG_DWORD /d 0 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU /v AUOptions /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update" /v AUOptions /t REG_DWORD /d 4 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate /v DisableWindowsUpdateAccess /t REG_DWORD /d 1 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate /v ElevateNonAdmins /t REG_DWORD /d 1 /f
reg add HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer /v NoWindowsUpdate /t REG_DWORD /d 0 /f
reg add "HKLM\SYSTEM\Internet Communication Management\Internet Communication" /v DisableWindowsUpdateAccess /t REG_DWORD /d 1 /f
reg add HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\WindowsUpdate /v DisableWindowsUpdateAccess /t REG_DWORD /d 1 /f
sc config wuauserv start=auto
sc start wuauserv
sc config WaaSMedicSvc start=auto
sc start WaaSMedicSvc
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\InstallService\State /v AutoUpdateLastSuccessTime /t REG_SZ /d "2000-01-01T00:00:00+08:00" /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings /v PauseFeatureUpdatesStartTime /t REG_SZ /d "2000-01-01T00:00:00Z" /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings /v PauseQualityUpdatesStartTime /t REG_SZ /d "2000-01-01T00:00:00Z" /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings /v PauseUpdatesExpiryTime /t REG_SZ /d "2000-01-01T00:00:00Z" /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings /v PauseFeatureUpdatesEndTime /t REG_SZ /d "2000-01-01T00:00:00Z" /f
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings /v PauseQualityUpdatesEndTime /t REG_SZ /d "2000-01-01T00:00:00Z" /f
pause
```
以上为恢复更新的脚本。


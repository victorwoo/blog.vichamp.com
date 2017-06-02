---
layout: post
title: 用 PowerShell 查看安装的 .NET 框架
description: Use Powershell to Return Versions of .NET Framework on a Machine
date: 2017-06-02 09:24:09
categories:
- powershell
tags:
- powershell
- .net
---
以下 PowerShell 代码最高支持 .NET 4.7 版本：

```powershell
Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -recurse |
Get-ItemProperty -name Version,Release -EA 0 |
Where { $_.PSChildName -match '^(?!S)\p{L}'} |
Select PSChildName, Version, Release, @{
  name="Product"
  expression={
      switch -regex ($_.Release) {
        "378389" { [Version]"4.5" }
        "378675|378758" { [Version]"4.5.1" }
        "379893" { [Version]"4.5.2" }
        "393295|393297" { [Version]"4.6" }
        "394254|394271" { [Version]"4.6.1" }
        "394802|394806" { [Version]"4.6.2" }
        "460798" { [Version]"4.7" }
        {$_ -gt 460798} { [Version]"Undocumented 4.7 or higher, please update script" }
      }
    }
}
```

参考：

- [How to: Determine Which .NET Framework Versions Are Installed](https://msdn.microsoft.com/en-us/library/hh925568)
- [PowerShell script to return versions of .NET Framework on a machine? - Stack Overflow](https://stackoverflow.com/questions/3487265/powershell-script-to-return-versions-of-net-framework-on-a-machine)

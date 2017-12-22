---
layout: post
title:  "windows server 运维笔记"
date:   2017-12-18 10:00:00
categories: 运维, windows
---


#一 文件权限
## 1.1 使用powershell批量修改文件ower
```powershell
# Define the owner account/group
$Account = New-Object -TypeName System.Security.Principal.NTAccount -ArgumentList 'BUILTIN\Administrators';

# Get a list of folders and files
$ItemList = Get-ChildItem -Path D:\data  -Recurse;

# Iterate over files/folders
foreach ($Item in $ItemList) {
    $Acl = $null; # Reset the $Acl variable to $null
    $Acl = Get-Acl -Path $Item.FullName; # Get the ACL from the item
    $Acl.SetOwner($Account); # Update the in-memory ACL
    Set-Acl -Path $Item.FullName -AclObject $Acl;  # Set the updated ACL on the target item
}
```

# powershell gits

## 修改RDP端口

```powershell
function ChangeRdp {
    param(
    [parameter(Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [int]
    $port
    )
 
    # Set the registry value for the port
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\Termin*Server\WinStations\RDP*CP\ -Name PortNumber -Value $port
     
    # Open the firewall port for Remote Desktop and Remote Desktop FX
    netsh advfirewall firewall add rule name="Custom RDP (in)" protocol=TCP dir=in localport=$port action=allow program="System"
    netsh advfirewall firewall add rule name="Custom RDP Remote FX (in)" protocol=TCP dir=in localport=$port action=allow program='%SystemRoot%\system32\svchost.exe'
     
    # Disable the previous rules on the old port
    netsh advfirewall firewall delete rule name='Remote Desktop (TCP-In)'
    netsh advfirewall firewall delete rule name='Remote Desktop - RemoteFX (TCP-In)'
     
    # Restart the service to finalize the changes
    # Use -Force as it has dependant services
    Restart-Service -Name TermService -Force
}
```

# 杂项

## 1. 映射阿里云NAS NFS挂载点

```bash
mount -o nolock \\xxx.cn-shenzhen.nas.aliyuncs.com\! x:
```
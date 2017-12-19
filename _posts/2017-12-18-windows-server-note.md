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
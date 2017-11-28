---
post_title: Cleanup/Remove old PowerShell modules
author: Christian Gräfe
post_excerpt: "Simple way to remove old module version"
layout: post
published: true
---
# Cleanup/Remove old PowerShell modules

As a big fan of the [dbatools][1] PowerShell module, I often install the latest pre-release for testing purposes.
In October 2017 the project was very active and I installed a lot of different versions on my workstation

So if you often install new version you can show all installed versions of a module this way
```powershell
PS C:\Windows\system32> Get-InstalledModule -Name dbatools -AllVersions

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
0.9.105    dbatools                            PSGallery            The community module that enables SQL Server Pro...
0.9.107    dbatools                            PSGallery            The community module that enables SQL Server Pro...
0.9.96     dbatools                            PSGallery            The community module that enables SQL Server Pro...
0.9.98     dbatools                            PSGallery            The community module that enables SQL Server Pro...
```

With this knowledge we can build a little script, which removes all versions, but leaves the latest version in places.
Keep in mind that uninstall process needs a PowerShell terminal with administrative rights.
```powershell
PS C:\Windows\system32> $module = Get-InstalledModule -Name dbatools
PS C:\Windows\system32> Get-InstalledModule $module.Name -AllVersions | Where-Object {$_.Version -ne $module.Version} |
Uninstall-Module -Verbose
VERBOSE: Performing the operation "Uninstall-Module" on target "Version '0.9.105' of module 'dbatools'".
VERBOSE: Successfully uninstalled the module 'dbatools' from module base 'C:\Program
Files\WindowsPowerShell\Modules\dbatools\0.9.105'.
VERBOSE: Performing the operation "Uninstall-Module" on target "Version '0.9.96' of module 'dbatools'".
VERBOSE: Successfully uninstalled the module 'dbatools' from module base 'C:\Program
Files\WindowsPowerShell\Modules\dbatools\0.9.96'.
VERBOSE: Performing the operation "Uninstall-Module" on target "Version '0.9.98' of module 'dbatools'".
VERBOSE: Successfully uninstalled the module 'dbatools' from module base 'C:\Program
Files\WindowsPowerShell\Modules\dbatools\0.9.98'.
PS C:\Windows\system32>
```

If everything works correct, it looks like above. You can check the module versions with the first command again.
```powershell
PS C:\Windows\system32>  Get-InstalledModule -Name dbatools -AllVersions

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
0.9.107    dbatools                            PSGallery            The community module that enables SQL Server Pro...


PS C:\Windows\system32>
```

I hope this is useful for you.

[1]: https://github.com/sqlcollaborative/dbatools
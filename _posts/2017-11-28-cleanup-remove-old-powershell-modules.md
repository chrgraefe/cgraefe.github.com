---
ID: 70
post_title: Cleanup/Remove old PowerShell modules
author: sharptools_wp
post_excerpt: ""
layout: post
permalink: >
  http://passionindata.de/2017/11/28/cleanup-remove-old-powershell-modules/
published: true
post_date: 2017-11-28 23:03:27
---
# Cleanup/Remove old PowerShell modules

As a big fan of the [dbatools][1] PowerShell module, I often install the latest pre-release for testing purposes. In October 2017 the project was very active and I installed a lot of different versions on my workstation

So if you often install new version you can show all installed versions of a module this way 
<pre><code class="powershell">
Get-InstalledModule -Name dbatools -AllVersions

Version   Name      Repository  Description ------- ---- ---------- ----------- 
0.9.105   dbatools  PSGallery   The community module that enables SQL Server Pro... 
0.9.107   dbatools  PSGallery   The community module that enables SQL Server Pro... 
0.9.96    dbatools  PSGallery   The community module that enables SQL Server Pro... 
0.9.98    dbatools  PSGallery   The community module that enables SQL Server Pro... 
</code></pre>

With this knowledge we can build a little script, which removes all versions, but leaves the latest version in places. Keep in mind that uninstall process needs a PowerShell terminal with administrative rights. 

<pre><code class="powershell">
$module = Get-InstalledModule -Name dbatools
Get-InstalledModule $module.Name -AllVersions | Where-Object {$_.Version -ne $module.Version} | Uninstall-Module -Verbose

VERBOSE: Performing the operation "Uninstall-Module" on target "Version '0.9.105' of module 'dbatools'".
VERBOSE: Successfully uninstalled the module 'dbatools' from module base 'C:Program
FilesWindowsPowerShellModulesdbatools.9.105'.
VERBOSE: Performing the operation "Uninstall-Module" on target "Version '0.9.96' of module 'dbatools'".
VERBOSE: Successfully uninstalled the module 'dbatools' from module base 'C:Program
FilesWindowsPowerShellModulesdbatools.9.96'.
VERBOSE: Performing the operation "Uninstall-Module" on target "Version '0.9.98' of module 'dbatools'".
VERBOSE: Successfully uninstalled the module 'dbatools' from module base 'C:Program
FilesWindowsPowerShellModulesdbatools.9.98'.
</code></pre>

If everything works correct, it looks like above. You can check the module versions with the first command again. 
<pre><code class="powershell">
Get-InstalledModule -Name dbatools -AllVersions

Version Name      Repository  Description ------- ---- ---------- ----------- 
0.9.107 dbatools  PSGallery   The community module that enables SQL Server Pro...
</code></pre>

I hope this is useful for you.

 [1]: https://github.com/sqlcollaborative/dbatools
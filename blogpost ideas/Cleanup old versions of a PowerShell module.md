$module = Get-InstalledModule -Name dbatools 
Get-InstalledModule $module.Name -AllVersions | Where-Object {$_.Version -ne $module.Version} | Uninstall-Module -Verbose 

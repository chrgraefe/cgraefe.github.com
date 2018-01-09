$Username = "myAdminUser"
$Password = "Welcome2sql"
$FullName = "My Admin User"
$Description = "Description of this account (My Admin User)."


Get-LocalUser | Where-Object { $_.Name -eq $Username } | Remove-LocalUser


$encryptedPassword = ConvertTo-SecureString -String $Password -AsPlainText -Force
New-LocalUser -Name $Username -Password $encryptedPassword -FullName $FullName -Description $Description -AccountNeverExpires -PasswordNeverExpires


Add-LocalGroupMember -Group "Administrators" -Member $Username
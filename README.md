# 👨‍💻 Azure Admin Group & Role Management
Demonstrating how to manage users, groups and role assignments in Azure using Azure PowerShell and Azure CLI.

## 🛠️ Tools & Services Used

- Azure Active Directory (Azure AD) – For creating and managing users and groups.
- Azure PowerShell – Scripting user and group creation.
- Azure CLI – Command-line tool for Azure resource and identity management.
- Azure Role-Based Access Control (RBAC) – For role assignment to groups.
- Azure Resource Groups – Logical containers for managing resources.

## ✅ Task
### 🔹 Exercise 1: Create the Senior Admins group via Azure Portal
####   Task 1: Create a user account for Joseph2 Price.

-  Navigate to Azure Active Directory > Users > New user.
-  Click Create

![image](https://github.com/user-attachments/assets/efdc677e-1e67-4ce9-8816-de1665a150a9)

  
####   Task 2: Create the Senior Admins group and add Joseph2 Price to it.
-  Azure Active Directory > Groups > New group
-  Group type: Security
-  Add member: Joseph2 Price  

![image](https://github.com/user-attachments/assets/0985840b-6dc4-45d7-8fde-66e3f5c1888d)


### 🔹 Exercise 2: Create the Junior Admins group via PowerShell
####   Task 1: Use PowerShell to create a user account for Isabel Garcia.

 ```powershell
Connect to Azure AD
Connect-AzureAD

Create user
New-AzureADUser -DisplayName "Isabel Garcia" `
                -UserPrincipalName "isabelgarcia@yourdomain.com" `
                -AccountEnabled $true `
                -MailNickName "isabelgarcia" `
                -PasswordProfile @{Password="YourPassword123!"}
  ```

![image](https://github.com/user-attachments/assets/ab753bfb-0e27-40f4-8cc7-61216b71ea0a)


####   Task 2: Use PowerShell to create the Junior Admins group and add Isabel Garcia to it.

 ```powershell
Create group
$group = New-AzureADGroup -DisplayName "Junior Admins" -SecurityEnabled $true -MailEnabled $false -MailNickName "junioradmins"

Get user object
$user = Get-AzureADUser -Filter "UserPrincipalName eq 'isabelgarcia@yourdomain.com'"

Add user to group
Add-AzureADGroupMember -ObjectId $group.ObjectId -RefObjectId $user.ObjectId
  ```

![image](https://github.com/user-attachments/assets/ea3763a3-6d71-42d8-8f1a-acd9042987bc)



### 🔹 Exercise 3: Create the Service Desk group via Azure CLI
####   Task 1: Use Azure CLI to create a user account for Dylan Williams.

 ```bash
az ad user create --display-name "Dylan Williams" \
                  --user-principal-name dylanwilliams@yourdomain.com \
                  --password "YourPassword123!" \
                  --force-change-password-next-login false
 ```

![image](https://github.com/user-attachments/assets/32a5282d-07a2-4156-9bfa-aa7b007f6d7c)


####   Task 2: Use Azure CLI to create the Service Desk group and add Dylan Williams to it.

```bash
Create group
az ad group create --display-name "Service Desk" --mail-nickname "servicedesk"

Add user to group
az ad group member add --group "Service Desk" --member-id $(az ad user show --id dylanwilliams@yourdomain.com --query objectId -o tsv)

 ```

![image](https://github.com/user-attachments/assets/57a40d97-1f67-420b-a770-a86a060dd8db)


### 🔹 Exercise 4: Assign RBAC Role to Service Desk Group
####   Task 1: Create a resource group for testing.

```bash
az group create --name "MyResourceGroup" --location "eastus"
 ```
####   Task 2: Assign the Virtual Machine Contributor role to the Service Desk group for the created resource group.

```bash
Get group object ID
GROUP_ID=$(az ad group show --group "Service Desk" --query objectId -o tsv)

Assign role to group at resource group scope
az role assignment create --assignee $GROUP_ID --role "Virtual Machine Contributor" --scope /subscriptions/{subscription-id}/resourceGroups/MyResourceGroup
 ```
![image](https://github.com/user-attachments/assets/0103fa63-8d50-40fd-aa9c-236c2f26eca2)



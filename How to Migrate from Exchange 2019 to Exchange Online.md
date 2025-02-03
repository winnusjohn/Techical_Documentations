# How to Migrate from Exchange 2019 to Exchange Online

Migrating from Exchange Server 2019 to Exchange Online involves several steps to ensure a smooth transition. Below is a comprehensive guide on how to migrate your email infrastructure from on-premises Exchange 2019 to Exchange Online. Please note that this is a general guideline, and it's crucial to adapt the steps based on your specific environment and requirements.

## Prerequisites

### 1. Review System Requirements
- Ensure that your existing Exchange Server 2019 is updated to the latest Cumulative Update.
- Verify system requirements for Exchange Online: [Exchange Online System Requirements](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/plan-and-deploy?view=exchserver-2019)
#
### 2. Prepare Your Active Directory

#### Verify Active Directory Schema
- Before migrating to Exchange Online, ensure your Active Directory schema is up-to-date.
- Run the following PowerShell command on a domain controller to check the current schema version:
  ```PowerShell
  Get-ADObject (Get-ADRootDSE).schemaNamingContext -Property objectVersion
- if the schema version is not up-to-date, update it.Ensure you have the necessary permissions, such as being a member of the Schema Admins group.
- Run the following command on a server with the Exchange setup files:
  ```PowerShell
  Setup.exe /PrepareSchema /IAcceptExchangeServerLicenseTerms
- Allow the schema update to complete.

#### Ensure that users have the necessary permissions for the migration
-	Ensure that the account performing the migration has the necessary permissions. It's recommended to use an account with the "Organization Management" role in Exchange. Also add the account to the "Recipient Management" role group if not already a member.
- Delegate the necessary permissions for mailbox moves. Use the following PowerShell commands: 
  ```PowerShell
  New-ManagementRoleAssignment -Role "Mailbox Migration" -User <MigrationUser>

**Note: Replace <MigrationUser> with the actual username**
-	If migrating public folders, ensure that the migration account has the necessary permissions.
- Run the following command using PowerShell
  ```PowerShell
  •	Add-PublicFolderAdministrativePermission -User <MigrationUser> -AccessRights AllExtendedRights -InheritanceType SelfAndChildren
#
### 3. Set Up and Configure Azure AD Connect
setting up and configuring Azure AD Connect is a critical step in ensuring seamless synchronization between your on-premises Active Directory and Azure AD for the migration to Exchange Online. Follow these steps to set up and configure Azure AD Connect


---
## Download and Install Azure AD Connect
### 1. Download Azure AD Connect:
- Visit the [Azure AD Connect download page](https://www.microsoft.com/en-us/download/details.aspx?id=47594)
- Download the latest version of Azure AD Connect
  
### 2. Run the Installation Wizard:
- Execute the downloaded installer on a Windows Server within your on-premises environment.
- Follow the prompts in the installation wizard

### 3. Express Settings or Custom Installation:
- Choose between Express Settings (recommended for most deployments) or Custom Installation based on your organization's requirements
  
### 4. Sign in to Azure AD:
- During installation, you will be prompted to sign in with an account that has the necessary permissions in Azure AD
#
**After signing into Azure AD. The next step is to  configure the Azure AD Connect**

---

# Configure Azure AD Connect
### 1. Configure Connectors:
- Choose the appropriate options for configuring connectors, such as "Sync all domains and OUs" or "Sync selected domains and OUs" based on your organization's structure.
### 2. Select Synchronization Methods:
- Choose the synchronization method that best fits your needs. Options include "Password Hash Synchronization," "Pass-through Authentication," or "Federation with AD FS"
### 3. Configure Optional Features:
- Choose the appropriate options for configuring connectors, such as "Sync all domains and OUs" or "Sync selected domains and OUs" based on your organization's structure
### 4. Configure Filtering:
- Define filtering rules to include or exclude specific objects based on attributes
### 5. Configure Domain and OU Filtering:
- If you chose the custom installation, configure domain and organizational unit (OU) filtering based on your environment
### 6. Configure Optional Features:
- Enable features like Exchange hybrid deployment if you plan to maintain a hybrid environment during migration
### 7. Configure Azure AD Domain Name System (DNS) Settings:
- Confirm the Azure AD domain name system (DNS) settings.
### 8. Start the Synchronization:
- Complete the configuration wizard, and allow the initial synchronization to run.
### 9. Monitor and troubleshoot the Azure AD Connect 
- Verify the synchronization process to ensure it is completed successfully
- Check for the Azure AD Connect health. Consider installing and configuring Azure AD Connect Health for monitoring and troubleshooting purposes

---

# Migration Steps
### 1. Assess Your Environment
- Mailbox Assessment:
  - Identify the number of mailboxes, their sizes, and usage patterns.
  - Identify any special configurations, such as shared mailboxes, distribution lists, and resource mailboxes.

- Client Access Assessment:
  - Identify the email clients and devices in use.
  - Ensure they are compatible with Exchange Online.

### 2. Plan and Prepare
- License Assignment:
   - Purchase and assign Exchange Online licenses to users.

- Network Considerations:
  - Ensure that your network bandwidth is sufficient for the migration.
  - Consider using Microsoft's ExpressRoute for a dedicated connection.

- Mail Flow:
  - Plan for mail flow during the migration period.
  - Decide whether you will use a cutover, staged, or hybrid migration approach.

### 3. Perform a Cutover Migration or Staged/Hybrid Migration
- Cutover Migration:
  - Move all mailboxes at once.
  - Suitable for small to medium-sized organizations.

- Staged Migration:
  - Move mailboxes in batches.
  - Suitable for larger organizations with a phased approach.
    
- Hybrid Migration:
  - Maintain a coexistence between on-premises and Exchange Online.
  - Recommended for large organizations with complex requirements.

### 4. Update DNS Records
- MX Record:
  - Update the MX record to point to Exchange Online.

- Autodiscover Record:
  - Update the Autodiscover record to point to Exchange Online.

### 5. Decommission On-Premises Exchange
- Remove Arbitration Mailboxes:
   - Move arbitration mailboxes to Exchange Online.

- Uninstall Exchange Servers:
  - Once migration is successful, you can decommission on-premises Exchange servers.

### 6. Verify and Monitor
- Verify Mailbox Migration:
  - Confirm that all mailboxes have been successfully migrated.

- Client Access Verification:
  - Ensure that clients can connect to Exchange Online without issues.

- Monitoring:
  - Monitor the Exchange Online environment for any issues.

### 7. Update Documentation and Train Users
- Update Documentation:
  - Update any internal documentation regarding email settings.

- User Training:
  - Provide training to users on new features and access methods.

### 8. Finalize Migration
- Complete the Migration Batch:
  - If using a staged migration, complete the migration batch.

- Verify Mail Flow:
  - Confirm that mail flow is working as expected.

- Remove Legacy Components:
  - Remove any remaining on-premises Exchange components

---

# Post-Migration
### 1. Ongoing Monitoring and Support
- Continuously monitor the Exchange Online environment.
- Provide support and assistance to users as needed.

### 2. Security Considerations
- Implement modern security practices, such as multi-factor authentication.

### 3. Optimize and Scale
- Optimize Exchange Online settings based on usage patterns.
- Consider scaling resources as needed.

**Note: Remember to thoroughly test the migration process in a lab environment before applying it to production.**

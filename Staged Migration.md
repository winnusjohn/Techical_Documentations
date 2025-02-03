# Staged Migration

A staged migration is a method for migrating mailboxes to Exchange Online in batches, making it suitable for larger organizations with a phased approach to the migration.

## Migration Process Overview

### 1. Planning
- Plan the migration in stages, determining which groups of mailboxes will be moved in each phase
- Consider factors such as departmental needs, geographical locations, or any other criteria relevant to your organization

### 2. Verify Domain Ownership
- Ensure that you have verified ownership of your domain in Microsoft 365
- This involves adding DNS records to your domain registrar to prove ownership

### 3. Create Migration Batches
- Navigate to the Microsoft 365 Admin Center and Exchange Admin Center (EAC)
- Create migration batches for each stage, specifying the mailboxes to be moved in each batch

### 4. Start Migration Batches
- Initiate the migration batches
- Exchange Online will start copying mailbox data from on-premises to the cloud for the specified users

### 5. Monitor Migration Progress
- Regularly monitor the migration progress using the EAC or PowerShell commands
- Ensure a smooth transition throughout the process

### 6. Complete and Finalize
- Once a migration batch is complete, finalize it to switch mail routing to Exchange Online
- Repeat the process for each subsequent batch until all mailboxes are migrated

## PowerShell Commands for Staged Migration

### Create a New Migration Batch
```PowerShell
New-MigrationBatch -Name "StagedBatch1" -SourceEndpoint OnPremises -Remote -RemoteHostName "mail.contoso.com" -RemoteCredential (Get-Credential) -AutoStart -NotificationEmails "admin@contoso.com" -CSVData (Get-Content "C:\Path\to\CSV\StagedBatch1.csv" | ConvertFrom-Csv)
```

### Command Explanation:
- **Name**: The name of the migration batch
- **SourceEndpoint**: Specifies the source email system (OnPremises)
- **RemoteHostName**: The hostname of your on-premises Exchange Server
- **RemoteCredential**: The credential object obtained using Get-Credential
- **AutoStart**: The migration batch will automatically start
- **NotificationEmails**: Email address for migration notifications
- **CSVData**: CSV file containing the list of mailboxes to be migrated

 **This output provides information about the new migration batch, including its identity, status, start time, and other relevant details**
 #### Here's a simplified example of the output you might see:
 
 ![Image](https://github.com/user-attachments/assets/fece774e-a77f-4946-9239-610dfbae923d)

 ---

 ### Get Migration Batch Status
 ```PowerShell
Get-MigrationBatch -Identity "StagedBatch1" | Get-MigrationUser -IncludeReport | Format-Table DisplayName, Status, MigrationType, TotalItemsTransferred
```

Example Output:

| DisplayName | Status | MigrationType | TotalItemsTransferred
  | ------ | ------ | ------ | ------ |
| John Doe | Synced | Staged | 500  
| Jane Smith | Syncing | Staged | 200  
| Bob Johnson | Failed | Staged | 0  

---

### Complete a Migration Batch
```PowerShell
Complete-MigrationBatch -Identity "StagedBatch1"
```

### Key Considerations for Staged Migration
### 1. Batch Size
- Adjust migration batch sizes based on:
    - Network bandwidth
    - Server capacity
### 2. Coexistence
- Plan for coexistence between on-premises Exchange and Exchange Online during migration

### 3. Communication
- Keep users informed about:
    - Migration Schedule
    - Required actions
      
### 4. Testing
- Conduct thorough testing in a lab environment before production implementation

### 5. Rollback Plan
- Develop a comprehensive rollback strategy in case of migration issues

**By following these steps and considerations, a staged migration allows larger organizations to migrate mailboxes in a controlled and phased manner, minimizing disruptions to end-users**.

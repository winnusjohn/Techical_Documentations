# Migration Steps for Mailbox Assessment

## Identify the Number of Mailboxes
- Use PowerShell to run the following command on your Exchange Server to get the total number of mailboxes:
  ```PowerShell
  Get-Mailbox -ResultSize Unlimited | Measure-Object
Example Output:

![Image 1](https://github.com/user-attachments/assets/c7d15b4e-51c9-4595-9fbd-fcc7f93d9173)

**Note: This output indicates the total number of mailboxes on your Exchange Server. The value for "Count" represents the total count of mailboxes**.

---

- Another way to view the list of mailboxes and their count is in the Exchange Admin Center (EAC):
  - Navigate to the "Recipients" tab to view the list of mailboxes and their count
#
### Determine Mailbox Sizes
- PowerShell Reporting:
  - Use PowerShell to generate a report on mailbox sizes. For example:
 ```PowerShell
 Get-MailboxStatistics -Server <ServerName> | Select DisplayName, TotalItemSize, ItemCount | Sort-Object TotalItemSize -Descending | Format-Table -AutoSize
```
**Note: Replace <ServerName> with the name of your Exchange Server.**

Example Output:

![Image](https://github.com/user-attachments/assets/dcc9dcfb-9e02-440d-8a4c-ae008e316271)

---
- Exchange Admin Center (EAC):
  - In EAC, under "Recipients," select "Mailboxes" to see a list of mailboxes with their sizes.

## Analyze Message Tracking Logs
- Analyze message tracking logs to understand email usage patterns, peak times, and busiest mailboxes.
- Use the following command to understand usage patterns:
  ```PowerShell
  Get-MessageTrackingLog -ResultSize Unlimited -Start "<StartDate>" -End "<EndDate>" | Group-Object -Property Sender | Sort-Object Count -Descending | Select-Object Name, Count | Format-Table -AutoSize
  ```
**Note: Replace <StartDate> and <EndDate> with the desired date range.**

 Example Output:

![Image](https://github.com/user-attachments/assets/6e3e633b-5163-4e8a-954c-1eb36f4623c8)  

---

- Explanation of Columns:
  - Name: The sender's email address.
  - Count: The number of messages sent by each sender within the specified date range.
#

### Identify Special Configurations
- List all shared mailboxes using PowerShell:
```PowerShell
  Get-Mailbox -RecipientTypeDetails SharedMailbox
```
- Identify and document distribution lists using PowerShell:
```PowerShell
  Get-DistributionGroup
```
- List all resource mailboxes using PowerShell:
```PowerShell
  Get-Mailbox -RecipientTypeDetails RoomMailbox, EquipmentMailbox
```
- Custom Configurations:
  - Identify any custom configurations, such as custom transport rules, mail flow settings, and mailbox permissions.
#
### Client Access Assessment
- Identify the email clients and devices in use.
- Ensure they are compatible with Exchange Online.

## Plan and Prepare
### License Assignment
Assigning Exchange Online licenses to users is a crucial step in the migration process. Before you start assigning licenses, make sure you have purchased the necessary licenses for your organization. Here's a step-by-step guide for planning and preparing a license assignments for Exchange Online

### 1. Purchase Exchange Online Licenses
- Log in to the Microsoft 365 Admin Center using administrative credentials: Microsoft 365 Admin Center.
- In the Admin Center, go to the "Billing" or "Billing > Subscriptions" section.
- Purchase the required number of Exchange Online licenses for your organization. Ensure you select the appropriate plan based on your organization's needs.

### 2. Assign Licenses to Users
- Assign Licenses to Individual Users:
    - In the Microsoft 365 Admin Center, go to "Users" or "Active Users."
    - Select the user(s) to whom you want to assign Exchange Online licenses.
    - Click on "Assign licenses" or a similar option.
    - Check the box next to "Exchange Online" or the specific license plan you purchased.
    - Review the assigned licenses and click "Assign" or "Save" to confirm.
- Bulk License Assignment
    - In the Microsoft 365 Admin Center, go to "Users" or "Active Users."
    - Select multiple users by holding down the "Ctrl" key (or "Cmd" key on Mac) while clicking on user names.
    - Click on "Assign licenses" or a similar option.
    - Check the box next to "Exchange Online" or the specific license plan.
    - Review the assigned licenses and click "Assign" or "Save" to confirm

### Automate License Assignment (Optional):
For large-scale deployments, consider using PowerShell scripts to automate the license assignment process. Example PowerShell command:
```PowerShell
$users = Get-MsolUser -All | Where-Object { $_.Licenses.Count -eq 0 }
$users | Set-MsolUserLicense -AddLicenses "TENANT:EXCHANGE_S_STANDARD"
```

### What this script does:
- Retrieves all users in your Microsoft 365 tenant who currently do not have any assigned licenses.
- Assigns the "EXCHANGE_S_STANDARD" license to the selected users. Adjust the license SKU based on your specific licensing plan.

**Note: Replace "TENANT:EXCHANGE_S_STANDARD" with the actual license SKU you want to assign. Test this script in a controlled environment before deploying it in production.**

#
### Network Considerations
- Ensure that your network bandwidth is sufficient for the migration.
- Consider using Microsoft's ExpressRoute for a dedicated connection.


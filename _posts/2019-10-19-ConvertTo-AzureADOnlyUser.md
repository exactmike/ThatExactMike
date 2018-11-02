---
title: "ConvertTo-AzureADOnlyUser"
excerpt: "converting a user that was connected with on premises Active Directory to be a stand alone cloud / Azure AD user"
date: 2018-10-24
last_modified_at: 2018-11-02
comments: true
tags:
  - AzureAD
  - Office365
  - Directory Synchronization
  - AzureADConnect
---

# background

Unfortunately, there is not really a ConvertTo-AzureADOnlyUser (sometimes called a Cloud Only user) function in the Azure AD module nor the MSOnline module.  But, at least for now, it is possible to do a conversion of an existing synchronized user (from an on premises directory) to a cloud only user that no longer depends on the on premises directory for it's identity and attributes.  

In my situation I had several mailboxes still hosted in an on premises Exchange environment and finally got around to migrating them.  For these mailboxes, I no longer wanted them to have any on premises dependencies so I needed to convert them to cloud only, Azure AD only user objects.  

Warning/Danger: As far as I can tell there is not an 'officially supported' way of doing this conversion, and the process I'm documenting here is not supported as far as I know. The only way that I was able to do it uses the MSOnline module which is headed for retirement at some point. However, the following procedure worked for me as of this posting.  It seems the official way to do this might be to delete the user, then create a new cloud only object and then connect orphaned/disconnected assets (like Exchange Mailbox and Onedrive?) to the newly created user object.  
{: .notice--danger}

# steps

- Move the mailbox(es) to Exhange Online
    - I used a hybrid move and used both standalone move request(s) and a migration batch to move different mailboxes during this operation.  Both worked fine.
- Remove the user object from synchronization with Azure AD.  
    - warning: this will disrupt your ability to access the mailbox and may disrupt (briefly) mail flow to the mailbox
    - options:
        - delete the user from AD
        - modify the sync scope to exclude an OU where the object resides in AD (this is what I did)
- Restore the user object from the Azure AD recycle bin
    - use Restore-Msoluser or Restore-AzureADMSDeletedDirectoryObject
    - use the AzureAD or Microsoft 365 portal
- Re-apply a license to the user object for the workload(s) you intend to keep using in Azure AD / Microsoft 365
- Set the ImmutableID of the object to be a blank string.  Do NOT set it to $null.  That will seem to work, but it will silently fail (no errors, no warnings.) I tried using the Azure AD module to do this but a blank string fails with that module and $null fails silently. 
  
    ```powershell

    Set-MSOLUser -UserPrincipalName [the upn of your object] -ImmutableID ''

    ```

# feedback

Do you know a better or supported way to accomplish conversion of a user to an AzureAD Only user?
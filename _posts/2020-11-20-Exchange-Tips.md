---
layout: post
title:  "Exchange Tips"
date:   2020-11-20 00:00:00 +0300
categories: tips
---

## Smtp Errors

### 504 5.7.4 Unrecognized authentication type

Starting in Exchange 2010, the only authentication mechanism enabled is NTLM.  
2 solutions:  
a) tell your app to use the NTLM authentication scheme.  
b) Enable AuthLogin authenticaton on Exchange:  
In the Exchange console under server configuration:  
Select hub transport.  
Right click the client server and select properties.  
Select the authentication tab.  
Check the Basic Authentication checkbox.  
Uncheck the Offer Basic only after TLS  
May have to restart the Exchange services.  

### 550 5.7.1 Client does not have permission to send as this sender

This error happens because the FROM address, the customer was using, was different than the Exchange mailbox they were relaying through.
Like the error message implies, this is a permissions issue. Go to:
a)From an Exchange Command prompt, run the following command:
Add-AdPermission -Identity “Default Receive Connector” -User “NT AUTHORITY\Authenticated Users” -ExtendedRights ms-Exch-SMTP-Accept-Any-Sender
b) On the user account, in Active Directory, under Security, under the SELF account, select the Manage Send As Permission option.
---
layout: post
title: SSH Artifacts in Windows 11 - Part 2
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

Continuing from yesterday's [post](https://ogmini.github.io/2025/03/26/Windows-SSH-Testing-Part-1.html), we are diving deep into the Windows Event Logs. Please refer back to the main [post](https://ogmini.github.io/2025/03/25/David-Cowen-Sunday-Funday-SSH-Windows.html) for full details as this post will only talk about the tests and results. 


#### Detailed Analysis of Windows Event Logs

##### Logon IDs

In the log analysis below, we will see the following Logon IDs:
- 0x3E7
- 0x44C61B
- 0x44C8A8
- 0x44C907

##### Successful Login

Pay special attention to the Logon IDs which are listed right after the EventID. 

1. EventID 4717 (0x3E7) - WINDOWS-SSH-SER$ is given the [SeServiceLogonRight](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/security-policy-settings/log-on-as-a-service). [https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4717](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4717)       
![20090](/images/ssh-challenge-windows/20090.png)
2. EventID 4648 (0x3E7) - The sshd.exe process now logs on (RUNAS) as the Account "sshd_4568". [https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4648](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4648)  
![20091](/images/ssh-challenge-windows/20091.png)
3. EventID 4624 (0x3E7) - Take note of the Logon Type, Virtual Account, and Elevated Token. [https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4624](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4624)  
![20092](/images/ssh-challenge-windows/20092.png)
4. EventID 4672 (0x44C61B) - "sshd_4568" is given the [SeImpersonatePrivilege](https://github.com/nickvourd/Windows-Local-Privilege-Escalation-Cookbook/blob/master/Notes/SeImpersonatePrivilege.md). [https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4672](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4672)  
![20093](/images/ssh-challenge-windows/20093.png)
5. EventID 4718 (0x3E7) - WINDOWS-SSH-SER$ has the seServiceLogonRight removed. [https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4718](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4718)  
![20094](/images/ssh-challenge-windows/20094.png)
6. EventID 4798 (0x3E7) - Enumerates local groups for the User we'ved logged in as. Again, I chose a bad username of "User" for testing purposes. [https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4798](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4798)  
![20095](/images/ssh-challenge-windows/20095.png)
7. EventID 4648 (0x3E7) - The sshd.exe process now logs on (RUNAS) as the Account "User".    
![20096](/images/ssh-challenge-windows/20096.png)
8. EventID 4624 (0x3E7) - Take note of the Logon Type, Virtual Account, and Elevated Token.  
![20097](/images/ssh-challenge-windows/20097.png)
9. EventID 4672 (0x44C8A8) - "User" is given a bunch of privileges. Note the Logon ID of 0x44C8A8.  
    - [https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4672](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4672)
    - SeSecurityPrivilege - Manage auditing and security log
    - SeTakeOwnershipPrivilege - Take ownership of files or other objects
    - SeLoadDriverPrivilege - Load and unload device drivers
    - SeBackupPrivilege - Back up files and directories
    - SeRestorePrivilege - Restore files and directories
    - SeDebugPrivilege - Debug programs
    - SeSystemEnvironmentPrivilege - Modify firmware environment values 
    - SeImpersonatePrivilege - Impersonate a client after authentication
    - SeDelegateSessionUserImpersonatePrivilege 
![20098](/images/ssh-challenge-windows/20098.png)
10. EventID 4634 (0x44C8A8) - The "User" is now logged out which is interesting because I hadn't closed the SSH Session yet. Note the Logon ID of 0x44C8A8. [https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4634](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4634)    
![20099](/images/ssh-challenge-windows/20099.png)
11. EventID 4798 (0x3E7) - Repeat of Step 6 above.  
![20100](/images/ssh-challenge-windows/20100.png)
12. EventID 4648 (0x3E7) - Repeat of Step 7 above.  
![20101](/images/ssh-challenge-windows/20101.png)
13. EventID 4624 (0x3E7) - Repeat of Step 8 above.  
![20102](/images/ssh-challenge-windows/20102.png)
14. EventID 4672 (0x44C907) - Repeat of Step 9 above.   
![20103](/images/ssh-challenge-windows/20103.png)
15. EventID 4634 (0x44C61B) - The "sshd_4568" is now logged out and this correlates with closing the actual SSH Session.  
![20104](/images/ssh-challenge-windows/20104.png)
16. EventID 4634 (0x44C907) - The "User" is now logged out and this correlates with closing the actual SSH Session.  
![20105](/images/ssh-challenge-windows/20105.png)

##### Failed Login

When a user inputs a bad password, the logs are similar for Steps 1 - 10 above. There would be the obvious differences in Logon IDs and the "sshd_####" account.

11. EventID 4625 - The "User" account fails to login. [https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4625](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4625)   
![badlogin](/images/ssh-challenge-windows/bad_login.png)
12. EventID 4634 - The "sshd_####" is now logged out and this correlates with closing the actual SSH Session.  

##### Connect No Attempt

When a user connects but makes no attempt to enter a password, the logs are similar for Steps 1 - 10 above. There would be the obvious differences in Logon IDs and the "sshd_####" account.

11. EventID 4634 - The "sshd_####" is now logged out and this correlates with closing the actual SSH Session.  

### SSH Private/Public Key

Unlike Debian, authenticating with keys must be explicitly enabled in Windows. I had to modify the sshd_config file located at `C:\ProgramData\ssh\sshd_config` to allow connections using a public key. [https://woshub.com/connect-to-windows-via-ssh/](https://woshub.com/connect-to-windows-via-ssh/). One point to note, you can enable local logging to the sshd.log file. Again, this is not enabled by default and instead logs are stored in the Windows Event Logs. 

By default the Private/Public keys are stored at `%userprofile%\.ssh`.

The Windows 11 Server saves the public key in the authorized_keys file located at `%userprofile%\.ssh` for the relevant user. They could also be stored at `C:\ProgramData\ssh\administrators_authorized_keys` for system-wide management. 

#### Event Viewer

Under Applications and Services Logs -> OpenSSH -> Operational, we see a record for the successful login using a Public Key.

![SSH key login](/images/ssh-challenge-windows/ssh_key_server_1.png)

The Windows Logs -> Security records are no different from the standard username/password authentication.
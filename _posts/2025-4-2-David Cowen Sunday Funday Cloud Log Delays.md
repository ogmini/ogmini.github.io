---
layout: post
title: David Cowen Sunday Funday Challenge - Cloud Log Availability Delays
author: 'ogmini'
tags:
 - sunday-funday
 - challenge
---

David Cowen has posted his weekly Sunday Funday challenge at his [blog](https://www.hecfblog.com/2025/03/daily-blog-793-sunday-funday-33025.html) and it is about the looking at the delays present between action and log availability.

I'm not going to be able to full testing as I don't have access to test instances for all these cloud providers.

## Challenge

For the main cloud providers (AWS, Azure, Google Cloud) determine how long it takes from you performing the action the log being available for the following actions:

1. Logging in successfully
2. Failing to login
3. Changing a users permissions
4. Deleting a user
5. Creating a user

## Test Setup

- I am performing testing on Microsoft Entra as part of a subscription for Microsoft 365 Business Standard.
- I will be checking Sign-in, Audit, and Provisioning Logs under the Microsoft Entra admin center. 
- MFA is configured on this tenant.
- Seconds are enabled on the Task Tray Clock

Initially, I had planned to check the logs using Graph API; but I do not have the required Premium P1 license. The following documenation from Microsoft talks about the process to check logs using the Microsoft Entra admin center and Graph API.

[https://learn.microsoft.com/en-us/entra/identity/monitoring-health/quickstart-analyze-sign-in](https://learn.microsoft.com/en-us/entra/identity/monitoring-health/quickstart-analyze-sign-in)

[https://learn.microsoft.com/en-us/entra/identity/monitoring-health/quickstart-access-log-with-graph-api](https://learn.microsoft.com/en-us/entra/identity/monitoring-health/quickstart-access-log-with-graph-api)
  
One important thing to note from the Microsoft Documentation is they explicitly state to 
> Wait for 5 minutes to ensure that you can find the event in the sign-in log.

We're going to test and verify this statement.

## Tests

### Logging in successfully

![Success Sign In](/images/cloudlogdelays/Success-SignIn.png)

I successfully log in at 4:23:10 PM and start refreshing the search screen.

![Success Refresh](/images/cloudlogdelays/Success-Refresh.png)

After refreshing the page at 4:27:04 the Success Log-in records are seen. 

![Success Found](/images/cloudlogdelays/Success-Found.png)

| RequestID (Censored) | Date | Status |
| --- | --- | --- |
| f1bb9a | 4/1/2025 4:23:09 PM | Interrupted |
| 43e60d | 4/1/2025 4:23:11 PM | Success | 
| 225002 | 4/1/2025 4:23:12 PM | Success |
| 0f4a0f | 4/1/2025 4:23:12 PM | Success |

It is nice to see that the timestamps are not off or show any appreciable delay. I'm going to chalk the second difference up to network delay. 

There is a delay of about 4 minutes. 

### Failing to login

![Failure Sign In](/images/cloudlogdelays/Failure-SignIn.png)

I fail log in at 4:39:05 PM and start refreshing the search screen. I'm going to forgo the screenshot of refreshing the logs.

![Failure Found](/images/cloudlogdelays/Failure-Found.png)

After refreshing the page at 4:42:59 PM the Failure Log-in record is seen.

| RequestID (Censored) | Date | Status |
| --- | --- | --- |
| 6351f6 | 4/2/2025 4:39:06 PM | Failure |

Again the timestamps are not off or show any appreciable delay. I'm going to chalk the second difference up to network delay. 

There is a delay of about 4 minutes.

### Deleting a user

![Delete User](/images/cloudlogdelays/Delete-User.png)

At 4:55:04 PM I delete a test user and start refreshing the search screen for the Audit Logs. I'm going to forgo the screenshot of refreshing the logs.

![Delete User Found](/images/cloudlogdelays/Delete-User-Found.png)

After refreshing the page at 4:47:15 PM the Delete user record is seen.

Again the timestamps are not off or show any appreciable delay.

There is a delay of about 2 minutes.

### Adding a user

![Add User](/images/cloudlogdelays/Add-User.png)

At 5:03:37 PM I add a test user and start refreshing the search screen for the Audit Logs. I'm going to forgo the screenshot of refreshing the logs.

![Add User Found](/images/cloudlogdelays/Add-User-Found.png)

After refreshing the page at 5:04:34 PM the Add user record is seen.

Again the timestamps are not off or show any appreciable delay.

There is a delay of about 1 minute.

### Changing a user's permissions

![Add Permissions](/images/cloudlogdelays/Perms.png)

At 5:12:46 PM, I add the Password Administrator role to my test user and start refreshing the search screen for the Audit Logs. I'm going to forgo the screenshot of refreshing the logs.

![Add Permissions Found](/images/cloudlogdelays/Perms-Found.png)

After refreshing the page at 5:14:01 PM the Add member to role record is seen.

Again the timestamps are not off or show any appreciable delay.

There is a delay of about 1 minute.

## Conclusion

Microsoft Entra Log Delays

| Log | Delay | Notes |
| --- | --- | --- |
| Log-in | ~4 minuttes | Approximately 4 minutes for log in success and failure. |
| Audit | ~2 minutes | Deleting a user showed 2 minutes. Adding a user was faster at 1 minute. Role changes were in the middle. |
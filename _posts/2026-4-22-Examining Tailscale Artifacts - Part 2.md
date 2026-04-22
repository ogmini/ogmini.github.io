---
layout: post
title: Examining Tailscale Artifacts - Part 2
author: 'ogmini'
tags:
 - Tailscale
---
You can have multiple accounts associated with Tailscale and be able to switch between them. I logged in with a Google and Microsoft account. More testing is required for the other account types and options.

I had expected to see another avatar file created for the second account I logged into. This was not the case; but isn't definitive. It is possible that the Microsoft account doesn't provide an avatar to Tailscale. More testing is required. As a reminder the avatars are stored in %localappdata%\Tailscale\Avatars. You can refer to yesterday's post for more details - [https://ogmini.github.io/2026/04/21/Examining-Tailscale-Artifacts.html](https://ogmini.github.io/2026/04/21/Examining-Tailscale-Artifacts.html).

The server-state.conf file located at %ProgramData%\Tailscale had some obvious changes. You can see a snippet/example below with some information redacted:

```text
{
  "_current/S-1-5-21-306847822-[SNIP]-[SNIP]-1000": "cHJ[SNIP]",
  "_machinekey": "cHJpdm[SNIP]",
  "_profiles": "eyIwNTMz[SNIP]",
  "profile-0533": "ewoJIkNv[SNIP]",
  "profile-617d": "ewoJI[SNIP]",
  "server-mode-start-key": nullbox.c
}
```

The redacted information is actually Base64 encoded text. Which when decoded results in the following with some information redacted:

```JSON
{
  "_current/S-1-5-21-306847822-[SNIP]-[SNIP]-1000": "profile-617d",
  "_machinekey": "privkey:[SNIP]",
  "_profiles":
    "{
      "0533": {
          "ID": "0533",
          "Name": "[SNIP]@gmail.com",
          "NetworkProfile": {
              "MagicDNSName": "tail[SNIP].ts.net",
              "DomainName": "[SNIP]@gmail.com",
              "DisplayName": "[SNIP]@gmail.com"
          },
          "Key": "profile-0533",
          "UserProfile": {
              "ID": [SNIP],
              "LoginName": "[SNIP]@gmail.com",
              "DisplayName": "ogmini",
              "ProfilePicURL": "https://lh3.googleusercontent.com/a/[SNIP]"
          },
          "NodeID": "[SNIP]",
          "LocalUserID": "S-1-5-21-306847822-[SNIP]-[SNIP]-1000",
          "ControlURL": "https://controlplane.tailscale.com"
      },
      "617d": {
          "ID": "617d",
          "Name": "[SNIP]@hotmail.com",
          "NetworkProfile": {
              "MagicDNSName": "tail[SNIP].ts.net",
              "DomainName": "[SNIP]@hotmail.com",
              "DisplayName": "[SNIP]@hotmail.com"
          },
          "Key": "profile-617d",
          "UserProfile": {
              "ID": [SNIP],
              "LoginName": "[SNIP]@hotmail.com",
              "DisplayName": "FirstName LastName"
          },
          "NodeID": "[SNIP]",
          "LocalUserID": "S-1-5-21-306847822-[SNIP]-[SNIP]-1000",
          "ControlURL": "https://controlplane.tailscale.com"
      }
    }",
  "profile-0533":
    "{
      "ControlURL": "https://controlplane.tailscale.com",
      "RouteAll": true,
      "ExitNodeID": "",
      "ExitNodeIP": "",
      "InternalExitNodePrior": "",
      "ExitNodeAllowLANAccess": false,
      "CorpDNS": true,
      "RunSSH": false,
      "RunWebClient": false,
      "WantRunning": true,
      "LoggedOut": false,
      "ShieldsUp": false,
      "AdvertiseTags": null,
      "Hostname": "",
      "NotepadURLs": false,
      "AdvertiseRoutes": null,
      "AdvertiseServices": null,
      "Sync": null,
      "NoSNAT": false,
      "NoStatefulFiltering": true,
      "NetfilterMode": 2,
      "AutoUpdate": {
        "Check": true,
        "Apply": true
      },
      "AppConnector": {
        "Advertise": false
      },
      "PostureChecking": false,
      "NetfilterKind": "",
      "DriveShares": null,
      "AllowSingleHosts": true,
      "Config": {
        "PrivateNodeKey": "privkey:[SNIP]",
        "OldPrivateNodeKey": "privkey:0000000000000000000000000000000000000000000000000000000000000000",
        "UserProfile": {
          "ID": [SNIP],
          "LoginName": "[SNIP]@gmail.com",
          "DisplayName": "ogmini",
          "ProfilePicURL": "https://lh3.googleusercontent.com/a/[SNIP]"
        },
        "NetworkLockKey": "nlpriv:[SNIP]",
        "NodeID": "[SNIP]"
      }
    }",
  "profile-617d":
    "{
      "ControlURL": "https://controlplane.tailscale.com",
      "RouteAll": true,
      "ExitNodeID": "",
      "ExitNodeIP": "",
      "InternalExitNodePrior": "",
      "ExitNodeAllowLANAccess": false,
      "CorpDNS": true,
      "RunSSH": false,
      "RunWebClient": false,
      "WantRunning": true,
      "LoggedOut": false,
      "ShieldsUp": false,
      "AdvertiseTags": null,
      "Hostname": "",
      "NotepadURLs": false,
      "AdvertiseRoutes": null,
      "AdvertiseServices": null,
      "Sync": null,
      "NoSNAT": false,
      "NoStatefulFiltering": true,
      "NetfilterMode": 2,
      "AutoUpdate": {
        "Check": true,
        "Apply": true
      },
      "AppConnector": {
        "Advertise": false
      },
      "PostureChecking": false,
      "NetfilterKind": "",
      "DriveShares": null,
      "AllowSingleHosts": true,
      "Config": {
        "PrivateNodeKey": "privkey:[SNIP]",
        "OldPrivateNodeKey": "privkey:0000000000000000000000000000000000000000000000000000000000000000",
        "UserProfile": {
          "ID": [SNIP],
          "LoginName": "[SNIP]@hotmail.com",
          "DisplayName": "FirstName LastName"
        },
        "NetworkLockKey": "nlpriv:[SNIP]",
        "NodeID": "[SNIP]"
      }
    }",
  "server-mode-start-key": null
}
```

Pretty easy to read and we can see the records for the two accounts that I connected. There is a profile 0533 and 617d which correlate to the two accounts I logged in with. The first line with _profile denotes which account is currently active on the computer.

You'll also notice that only one account has a ProfilePicURL which sort of lines up with my theory above about there being no avatar for the Microsoft account. This needs to be tested.

Someething to call out for possible investigations is the DisplayName which pulled the Firstname and Lastname that I specified when creating the test accounts on Google and Microsoft. Potentially useful if you have a sloppy suspect.

A lot more to dive into later. I'm particularly interested in the NodeIDs, _machinekey, and LocalUserID. More than likely that LocalUserID is the SID on the computer for the user account Tailscale is being run under. The pattern matches but I didn't actually verify that yet. This could be useful when someone sets up Tailscale to be run unattended.

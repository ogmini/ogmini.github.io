---
layout: post
title: Examining Tailscale Artifacts
author: 'ogmini'
tags:
 - Tailscale
---

I started looking at [Tailscale](https://tailscale.com/) to see what sort of digital artifacts are left behind. It has clients for Windows/Linux/Android/iOS/macOS and I'm hoping to take a look at all of them except iOS. Tailscale is a networking tool that lets you securely connect your devices (laptop, phone, servers, cloud VMs, etc.) as if they were all on the same private network, no matter where they are. A very popular tool with Homelab people and I use it myself.

Today, I fired up a clean VM running Windows 11 25H2 and installed Tailscale 1.96.3. Authenticated with my account and started to poke around.

Locations of note:

- %localappdata%\Tailscale
  - Avatars folder
- %localappdata%\temp\Tailscale*.log
  - Installation log files
- %ProgramData%\Tailscale
  - Logs folder
  - files folder

The Avatars folder is interesting because it contains files with what appear to be HTTP/2 responses that have the avatar image embedded. I was able to extract them with a hex editor.

Tailscale also has support for secure file transfer between devices on a tailnet. You can read more at [https://tailscale.com/docs/features/taildrop](https://tailscale.com/docs/features/taildrop). In current versions, files are dropped into %userprofile%\Downloads AFTER they are temporarily stored at %ProgramData%\Tailscale\files\[Account-uid-####]\ during transfer. This is to support the ability to resume interrupted tranfers. Possible to find interesting artifacts here...

Additionally, I was able to find records in the local Tailscale logs about these transfers. Below are some examples:

Tiny transfer

```log
2026-04-21T11:10:26.712-04:00: [v1] Accept: TCP{100.113.x.x:54099 > 100.105.x.x:34718} 41 ok out
2026-04-21T11:10:26.713-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 29500/767972
2026-04-21T11:10:26.713-04:00: [v1] Accept: TCP{100.105.x.x:34718 > 100.113.x.x:54099} 40 tcp non-syn
2026-04-21T11:10:26.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 29580/767972
2026-04-21T11:10:28.000-04:00: [v1] Accept: TCP{100.105.x.x:56010 > 100.113.x.x:36843} 52 tcp ok
2026-04-21T11:10:28.033-04:00: peerapi: got put of <=1MB in 0s from 100.105.x.x/0x7ff60151dc40
```

Moderate transfer

```log
2026-04-21T11:22:08.273-04:00: [v1] Accept: TCP{100.105.x.x:56042 > 100.113.x.x:36843} 52 tcp ok
2026-04-21T11:22:08.275-04:00: [v1] Accept: TCP{100.105.x.x:56042 > 100.113.x.x:36843} 40 tcp non-syn
2026-04-21T11:22:08.275-04:00: [v1] Accept: TCP{100.105.x.x:56042 > 100.113.x.x:36843} 172 tcp non-syn
2026-04-21T11:22:08.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 36220492/1877892
2026-04-21T11:22:10.480-04:00: peerapi: got put of ~141MB in 2.2s from 100.105.x.x/0x7ff60151dc40
```

Cancelled transfer

```log
2026-04-21T12:36:44.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 283171412/9316908
2026-04-21T12:36:46.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 434701012/13177516
2026-04-21T12:36:47.346-04:00: netmap: suggested exit node:  ()
2026-04-21T12:36:48.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 563871396/16888732
2026-04-21T12:36:50.347-04:00: netmap: suggested exit node:  ()
2026-04-21T12:36:50.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 704353732/20631252
2026-04-21T12:36:52.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 834675908/24517588
2026-04-21T12:36:53.242-04:00: [v1] Accept: TCP{100.105.x.x:56073 > 100.113.x.x:36843} 1280 tcp non-syn
2026-04-21T12:36:53.347-04:00: netmap: suggested exit node:  ()
2026-04-21T12:36:54.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 985344324/28780612
2026-04-21T12:36:56.347-04:00: netmap: suggested exit node:  ()
2026-04-21T12:36:56.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 1138328004/33788836
2026-04-21T12:36:58.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 1286926196/38278692
2026-04-21T12:36:59.347-04:00: netmap: suggested exit node:  ()
2026-04-21T12:37:00.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 1435116740/42726524
2026-04-21T12:37:02.348-04:00: netmap: suggested exit node:  ()
2026-04-21T12:37:02.735-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 1583067284/46583196
2026-04-21T12:37:03.242-04:00: [v1] Accept: TCP{100.105.x.x:56073 > 100.113.x.x:36843} 1280 tcp non-syn
```

Sent File

```log
2026-04-21T15:02:42.720-04:00: [v1] Accept: TCP{100.105.x.x:53517 > 100.104.x.x:58294} 41 ok out
2026-04-21T15:02:42.721-04:00: [v1] Accept: TCP{100.104.x.x:58294 > 100.105.x.x:53517} 40 tcp non-syn
2026-04-21T15:02:42.721-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 892/1220
2026-04-21T15:02:44.495-04:00: localapi: [PUT] /localapi/v0/file-put/
2026-04-21T15:02:44.496-04:00: [v1] Accept: TCP{100.105.x.x:53517 > 100.104.x.x:58294} 165 ok out
2026-04-21T15:02:44.503-04:00: netmap: suggested exit node:  ()
2026-04-21T15:02:44.505-04:00: netmap: suggested exit node:  ()
2026-04-21T15:02:45.532-04:00: [v1] v1.96.3-t3ffddb134-g460d8764a peers: 1644/8036
```

Zeroing in on a few specific lines:

```log
2026-04-21T11:10:28.033-04:00: peerapi: got put of <=1MB in 0s from 100.105.38.14/0x7ff60151dc40
```

This is logged on the client that recieves a file. We can glean the approximate size of the file, the IP of the Tailscale device that sent the file, and when the file was received.

```log
2026-04-21T15:02:44.495-04:00: localapi: [PUT] /localapi/v0/file-put/
```

This is logged on the client that sent the file. Previous and subsequent log records possibly point to the IP of the Tailscale device the file was sent to. But this requires far more testing. Those log records could just be showing communication between clients.

Many more things I want to test in future posts.

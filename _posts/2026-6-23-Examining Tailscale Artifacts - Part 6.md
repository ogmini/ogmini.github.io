---
layout: post
title: Examining Tailscale Artifacts - Part 6
author: 'ogmini'
tags:
 - Tailscale
 - MacOS
---

This will sort of be a two for one post today as I'm still at training until the end of the week. Always fun how these sorts of things can get the mind churning and give you the motivation to start looking at stuff again. Today was mainly about Log Analytics workspaces and KQL. I had prior experience with what I would call doing "easy" queries. I've not had much need to play with functions like summarize, bin, or even search/find. I was very reliant on using the traditional where clause. So it was nice to be "forced" to use them.

Anyways, going back to Tailscale. Back in [Part 1](https://ogmini.github.io/2026/04/21/Examining-Tailscale-Artifacts.html), I had asked the question if partial downloads are kept forever or cleaned up periodically. I have an answer that is untested/unverified. It comes by way of looking at the Tailscale repository on GitHub specifically the `delete.go` file for Taildrop at [https://github.com/tailscale/tailscale/blob/d4f2917c1bfd27529ec7997d08a8f1aa9ea903f2/feature/taildrop/delete.go](https://github.com/tailscale/tailscale/blob/d4f2917c1bfd27529ec7997d08a8f1aa9ea903f2/feature/taildrop/delete.go). In the file we see the comment and const variable:

```go
// deleteDelay is the amount of time to wait before we delete a file.
// A shorter value ensures timely deletion of deleted and partial files, while
// a longer value provides more opportunity for partial files to be resumed.
const deleteDelay = time.Hour
```

So one hour before the partial file gets deleted.

Another interesting callout from reading the code. In the `ext.go` file for Taildrop at [https://github.com/tailscale/tailscale/blob/d4f2917c1bfd27529ec7997d08a8f1aa9ea903f2/feature/taildrop/ext.go](https://github.com/tailscale/tailscale/blob/d4f2917c1bfd27529ec7997d08a8f1aa9ea903f2/feature/taildrop/ext.go) there is a comment about how MacOS and some other clients handle the location of in-progress downloads differently. 

```go
// directFileRoot, if non-empty, means to write received files
	// directly to this directory, without staging them in an
	// intermediate buffered directory for "pick-up" later. If
	// empty, the files are received in a daemon-owned location
	// and the localapi is used to enumerate, download, and delete
	// them. This is used on macOS where the GUI lifetime is the
	// same as the Network Extension lifetime and we can thus avoid
	// double-copying files by writing them to the right location
	// immediately.
	// It's also used on several NAS platforms (Synology, TrueNAS, etc)
	// but in that case DoFinalRename is also set true, which moves the
	// *.partial file to its final name on completion.
	directFileRoot string
```

Again, I want to test this when I get the chance.

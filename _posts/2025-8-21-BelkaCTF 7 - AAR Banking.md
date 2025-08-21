---
layout: post
title: BelkaCTF 7 - AAR Banking
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task10.png)

We need to the last 4 digits of the owner's bank card and the contact phone number of the bank. Time to go looking in emails again!

Again we use ALEAPP and check the Email section. I find a receipt, sorry not a receipt from McDonald's that has the last 4 digits of the card used as `7076`. This doesn't tell us who his bank is and we need to keep digging. I search the emails for anyhing related to a bank and find nothing.

![McDonalds](/images/BelkaCTF7/Task10-1.png)

Next, I go poking around browser history and find a visit to `https://libertybank.ge/ka/agreements/liberti-angarishi`.

![Bank](/images/BelkaCTF7/Task10-2.png)

Visiting the contact page for this bank at `https://libertybank.ge/ka/chven-shesakheb/kompaniis-shesakheb/kontaqti` we find the phone number of `+995322555500`.

![BankContact](/images/BelkaCTF7/Task10-3.png)

## Thoughts

From reading the official writeup, I learned that this could also have been solved by looking at the Google Pay database. I do not believe that ALEAPP currently parses this. Maybe something to poke around for the future. 

---
layout: post
title: BelkaCTF 6 - Bogus Bill Tasks 1-5
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
---

Did a few of the tasks from a previous BelkaCTF as a warmup for this weekend. Solutions are written using Open Source or Free tools. I like to validate the findings from tools whenever possible. I've always appreciated that iLEAPP provides the location of where it found artifacts so that an analyst can manually parse if needed.

### Task 1

_What is the Apple ID used on the imaged iPhone?_

Using iLEAPP, we can check the Account Data section and locate the "Apple ID" as "billthemegakill@icloud.com".

![iLEAPP](/images/BelkaCTF6/AppleID-ileapp.png)

This information is stored in a SQLite database called Accounts3.sqlite located at \data\private\var\mobile\Library\Accounts. Manually examining the ZACCOUNT and ZACCOUNTTYPE table with DB Browser for SQLite verifies the information reported by iLEAPP. Example query below:

~~~ SQL
SELECT ZUSERNAME, ZACCOUNTTYPE.ZACCOUNTTYPEDESCRIPTION
FROM ZACCOUNT
INNER JOIN ZACCOUNTTYPE 
ON ZACCOUNT.ZACCOUNTTYPE = ZACCOUNTTYPE.Z_PK
~~~

![SQL](/images/BelkaCTF6/AppleID-SQL.png)

### Task 2

_What is the iPhone owner's full name?_

Using ILEAPP, we can check the Address Book under the Contacts section. There is only one record of "William Phorger".

![iLEAPP Owner](/images/BelkaCTF6/Owner-ileapp.png)

This information is stored in a SQLite database called AddressBook.sqlitedb located at \data\private\var\mobile\Library\AddressBook. Manually examining the ABMultiValue and ABPerson table with DB Browser for SQLite verifies the information reported by iLEAPP. Example query below:

~~~ SQL
SELECT *
FROM ABMultiValue
INNER JOIN ABPerson
ON ABMultiValue.record_id = ABPerson.ROWID
~~~

![SQL Owner](/images/BelkaCTF6/Owner-SQL.png)

### Task 3

_Which Telegram accounts did the owner discuss shady stuff with?_

Using ILEAPP, we can check the Telegram - Messages under the Telegram section. The individual conversed with the following:

- @diddyflowers
- @locknload771
- @Sm00thOperat0r
- @JesusStreeton1999

Belkasoft has a very good writeup on how to manually parse this information at [https://belkasoft.com/ios-telegram-forensics-acquisition-and-database-analysis](https://belkasoft.com/ios-telegram-forensics-acquisition-and-database-analysis).

### Task 4

_Where does William live?_

Apps such as Uber like to store your home location and other destinations locally. Using ILEAPP, we can check the Uber - Places under the Uber section. There is a location with the tag of "home".

![iLEAPP Location](/images/BelkaCTF6/Location-ileapp.png)

This information is stored in a SQLite database called database.db located at \data\private\var\mobile\Containers\Data\Application\[GUID]\Documents\. Manually examining the place table with DB Browser for SQLite verifies the information reported by iLEAPP.

![SQL Location](/images/BelkaCTF6/Location-SQL.png)

### Task 5

_What is the username of the laptop user?_

Using Autopsy, we can look under OS Accounts and find a reference to "phorger". We can also cross reference this to the names of the user folders under %SystemDrive%\Users.

![Laptop Account](/images/BelkaCTF6/LaptopAccount-Autopsy.png)

![Laptop Account 2](/images/BelkaCTF6/LaptopAccount-Autopsy2.png)

Examining the SOFTWARE hive with Registry Explorer, we can examine the keys located at HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList.  

![Laptop Account Registry](/images/BelkaCTF6/LaptopAccount-Registry.png)

## Tools Used

- Autopsy - [https://www.autopsy.com/](https://www.autopsy.com/)
- DB Browser for SQLite - [https://sqlitebrowser.org/](https://sqlitebrowser.org/)
- iLEAPP - [https://github.com/abrignoni/iLEAPP](https://github.com/abrignoni/iLEAPP)
- Registry Explorer - [https://ericzimmerman.github.io/#!index.md](https://ericzimmerman.github.io/#!index.md)

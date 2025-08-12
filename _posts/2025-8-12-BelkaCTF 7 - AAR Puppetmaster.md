---
layout: post
title: BelkaCTF 7 - AAR Puppetmaster
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task7.png)

I have a small complaint on this one as it required me to create a Telegram account. I wasn't particulary happy about that requirement for various personal reasons. From our previous reverse engineering of the malware we had found the following:

- API Url - <https://api-telegram-org.ctf.do/>
- API Key - 8169267144:AAFwkhmXh71SMVcvFLantjTlwlRbT9HdJdU
- Telegram ID - 7474460026

![admin_telegram_id](/images/BelkaCTF7/Task7-1.png)

Typically, when malware uses Telegram as a C2 mechanism the controller will have a chat that the malware bot can join. This allows them to send commands and recieve replies. Our next steps are to leverage the Telegram API to see what chats the bot is a member of and who else is in those chats. I used Postman to send the API call and read the response. It can also spit out the equivalent call in curl and other languages.

![Postman](/images/BelkaCTF7/Task7-2.png)

~~~ curl
curl --location 'https://api-telegram-org.ctf.do/bot8169267144:AAFwkhmXh71SMVcvFLantjTlwlRbT9HdJdU/getChat?chat_id=7474460026'
~~~

We get back a Telegram username of "pineapple_press". From here, you open up your Telegram client and search for the user to get their publicly shared telephone number.

![Telephone](/images/BelkaCTF7/Task7-3.png)

## Thoughts

Complaints about needing a Telegram account aside, this was a fun challenge as I got to use some tools that I used as a developer. I make extensive use of Postman to easily test APIs. I guess I also have a Telegram account now. I really tried to see if I could get the telephone number by other means including sending a message to the chat/user. That didn't work obviously as we know the malware operator is a little "busy" to reply back. I don't think I'm the only one to have messaged the chat as the "message_id" in the response steadily increased over time. I'd be interested to see/know what else people messaged that chat with.

![Chatting](/images/BelkaCTF7/Task7-4.png)

~~~ curl
curl --location 'https://api-telegram-org.ctf.do/bot8169267144:AAFwkhmXh71SMVcvFLantjTlwlRbT9HdJdU/sendMessage?Content-Type=application%2Fjson' \
--header 'Content-Type: application/json' \
--data '{
  "chat_id": 7474460026,
  "text": "Please share your phone number",
  "reply_markup": {
    "keyboard": [
      [
        {
          "text": "Share my phone number",
          "request_contact": true
        }
      ]
    ],
    "one_time_keyboard": true,
    "resize_keyboard": true
  }
}
~~~

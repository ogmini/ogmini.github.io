---
layout: post
title: Magnet Virtual Summit 2026 CTF - AAR Cipher Challenges
author: 'ogmini'
tags:
 - CTF
 - Challenges
---

Below are my writeups for the Cipher Pre-Challenges from the recent MVS 2026 CTF.

## Can you Handle this one?

> Hi Jessica! 00110110 00111000 00100000 00110111 00110100 00100000 00110111 00110100 00100000 00110111 00110000 00100000 00110111 00110011 00100000 00110011 01100001 00100000 00110010 01100110 00100000 00110010 01100110 00100000 00110111 00110011 00100000 00110111 00110100 00100000 00110110 00110001 00100000 00110111 00110010 00100000 00110111 00110100 00100000 00110110 01100100 00100000 00110110 00110101 00100000 00110010 01100101 00100000 00110111 00110011 00100000 00110111 00110100 00100000 00110110 00110001 00100000 00110111 00110010 00100000 00110110 01100010 00100000 00110011 00110100 00100000 00110110 01100101 00100000 00110011 00110110 00100000 00110010 01100101 00100000 00110110 00110011 00100000 00110110 01100110 00100000 00110110 01100100 00100000 00110010 01100110

This looks like binary. Some give aways are the 0s and 1s and the grouping of 8. Using CyberChef we can convert from Binary. [CyberChef From Binary](https://cyberchef.org/#recipe=From_Binary('Space',8)From_Hex('Auto'/breakpoint)&input=MDAxMTAxMTAgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMDAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDExMDAwMDEgMDAxMDAwMDAgMDAxMTAwMTAgMDExMDAxMTAgMDAxMDAwMDAgMDAxMTAwMTAgMDExMDAxMTAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAwMDEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMTAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAxMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAxMDEgMDAxMDAwMDAgMDAxMTAwMTAgMDExMDAxMDEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAwMDEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMTAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAxMDEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAwMTAgMDExMDAxMDEgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAxMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAxMDAgMDAxMDAwMDAgMDAxMTAwMTAgMDExMDAxMTA)

>68 74 74 70 73 3a 2f 2f 73 74 61 72 74 6d 65 2e 73 74 61 72 6b 34 6e 36 2e 63 6f 6d 2f

This output looks like Hex. [CyberChef From Hex](https://cyberchef.org/#recipe=From_Binary('Space',8)From_Hex('Auto')&input=MDAxMTAxMTAgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMDAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDExMDAwMDEgMDAxMDAwMDAgMDAxMTAwMTAgMDExMDAxMTAgMDAxMDAwMDAgMDAxMTAwMTAgMDExMDAxMTAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAwMDEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMTAgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAxMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAxMDEgMDAxMDAwMDAgMDAxMTAwMTAgMDExMDAxMDEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAwMDEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAwMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMTAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAxMDEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAwMTAgMDExMDAxMDEgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAxMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAxMDAgMDAxMDAwMDAgMDAxMTAwMTAgMDExMDAxMTA)

Jessica Hyde had also given a very big hint on LinkedIn - [https://www.linkedin.com/posts/hydejessica_dfir-ctf-activity-7417910498744016896-Pdzf?utm_source=share&utm_medium=member_desktop&rcm=ACoAAADfdBEBELHpzRkuF5mAh39sI_EZfTyW4lg](https://www.linkedin.com/posts/hydejessica_dfir-ctf-activity-7417910498744016896-Pdzf?utm_source=share&utm_medium=member_desktop&rcm=ACoAAADfdBEBELHpzRkuF5mAh39sI_EZfTyW4lg). Bin2Hex or Binary to Hex.

Answer: [https://startme.stark4n6.com/](https://startme.stark4n6.com/)

## Peel Back the Layers

> Can you find the hidden link? (Provide the redirected resource URL)
>
> File Download: [ThisIsFine.pxz](/files/MVS2026/ThisIsFine.pxz)

This is a PXZ file which is a zip file of .webp image files and a JSON Manifest used by [https://pixlr.com/](https://pixlr.com/). We can unzip the file and examine the individual files.

![Contents](/images/MVS2026/cipher-2-1.png)

I first checked the `manifest.json` file and saw [http://tiny.cc/7h5v001](http://tiny.cc/7h5v001). The classic RickRoll and this is not the answer.

``` JSON
{
  "id": "b13faf49-01ce-4a21-9739-3e89e56a33c0",
  "name": "Untitled(2)",
  "type": "document",
  "unit": "pixel",
  "width": 1000,
  "height": 1000,
  "stack": [
    {
      "name": "Flag???",
      "type": "image",
      "rect": {
        "x": 0,
        "y": 0,
        "w": 1019,
        "h": 1019,
        "r": 0
      },
      "opacity": 1,
      "locked": false,
      "visible": true,
      "link": "",
      "content": "2403a8354dbb.webp"
    },
    {
      "name": "Flag???",
      "type": "image",
      "rect": {
        "x": 8,
        "y": 84,
        "w": 984,
        "h": 858,
        "r": 0
      },
      "opacity": 1,
      "locked": false,
      "visible": true,
      "link": "",
      "content": "8782b3234be0.webp"
    },
    {
      "name": "Flag??",
      "type": "text",
      "rect": {
        "x": 89,
        "y": 444,
        "w": 833,
        "h": 78,
        "r": 0
      },
      "opacity": 1,
      "locked": false,
      "visible": true,
      "link": "",
      "content": "http://tiny.cc/7h5v001",
      "format": {
        "size": 60,
        "align": "center",
        "bold": false,
        "italic": false,
        "underline": false,
        "uppercase": false,
        "linespace": 0,
        "letterspace": 0,
        "font": {
          "name": "Arial",
          "content": "arial.woff"
        },
        "fill": {
          "type": "color",
          "value": "#ffffff"
        }
      }
    },
    {
      "name": "Flag?",
      "type": "image",
      "rect": {
        "x": -242,
        "y": -23,
        "w": 1485,
        "h": 1047,
        "r": 0
      },
      "opacity": 1,
      "locked": false,
      "visible": true,
      "link": "",
      "content": "d1f3b7ad4efe.webp"
    }
  ]
}
```

I checked the various image files and found some very light gray text in `2403a8354dbb.webp` which points to [http://tiny.cc/nh5v001](http://tiny.cc/nh5v001) which redirects to [https://github.com/abrignoni](https://github.com/abrignoni).

![2403a8354dbb.webp](/images/MVS2026/2403a8354dbb.webp)

Peel back the layers was a reference to the zip file.

Answer: [https://github.com/abrignoni](https://github.com/abrignoni)

## Good Grief!

> The Great Pumpkin’s Inverse Logic
>
> File Download: [VanPelt.png](/images/MVS2026/VanPelt.png)

![VanPelt.png](/images/MVS2026/VanPelt.png)

The image is a [Pigpen Cipher](https://www.dcode.fr/pigpen-cipher) which decodes to WLFYOVYOZP. This is an [Atbash Cipher](https://www.dcode.fr/atbash-cipher). Using [CyberChef Atbash Cipher](https://cyberchef.org/#recipe=Atbash_Cipher()&input=V0xGWU9WWU9aUA) we can decode the answer of DOUBLEBLAK which is a reference to [https://www.doubleblak.com/](https://www.doubleblak.com/)

We had a few hints on this one. Good Grief! was a reference to Peanuts the comic. VanPelt.png is a reference to Linus Van Pelt. Hexordia provided another hint [https://www.linkedin.com/posts/hexordia_mobileforensics-dfir-forensictraining-activity-7422664920581746688-XmZn](https://www.linkedin.com/posts/hexordia_mobileforensics-dfir-forensictraining-activity-7422664920581746688-XmZn) of "Even the messiest members of the gang can play!". This references the character Pig Pen from Peanuts. Pig Pen was a hint towards the pigpen cipher and the "Inverse Logic" in the description hints to the Atbash Cipher.

Answer: DOUBLEBLAK

## Resident Evil

> Can you find the link?
>
> File Download: [Challenge3.vhdx](/files/MVS2026/Challenge3.vhdx)

Mounting the vhdx file and we can see it contains a readme.txt file with an obvious fake answer ("These aren't the droids you're looking for..."). Decoding the binary with [CyberChef](https://cyberchef.org/#recipe=From_Binary('Space',8)&input=MDEwMDExMDEwMTEwMTAwMTAxMTEwMDExMDExMTAwMTEwMTEwMTAwMTAxMTAxMTEwMDExMDAxMTEKMDExMDAwMDEwMDEwMDAwMDAxMTEwMDAwMDExMDEwMDEwMTEwMDEwMTAxMTAwMDExMDExMDAxMDE) has the author taunting us with a possible hint of "Missing a piece".

![cipher-4-1](/images/MVS2026/cipher-4-1.png)

Load up the disk into Autopsy and start poking around. The $MFT file and $MFTMirr file can often times contain remnants of files and other useful information. It can be a way to recover deleted or changed files.

![cipher-4-2](/images/MVS2026/cipher-4-2.png)

We can already sort of see the answer in the "Extracted Text" and dumping the binary into [CyberChef](https://cyberchef.org/#recipe=From_Binary('Space',8)&input=MDExMTAxMDAwMTExMDEwMDAxMTEwMDAwMDExMTAwMTEwMDExMTAxMDAwMTAxMTExMDAxMDExMTEwMTEwMDExMTAxMTAxMDAxMDExMTAxMDAwMTEwMTAwMDAxMTEwMTAxMDExMDAwMTAwMDEwMTExMDAxMTAwMDExMDExMDExMTEwMTEwMTEwMTAwMTAxMTExMDExMDExMDEwMTEwMDAwMTAxMTAxMTEwMDExMDAxMDAwMTEwMTAwMTAxMTAwMDAxMDExMDExMTAwMTExMDEwMDAwMTAxMTExMDExMDExMDEwMTEwMDAwMTAxMTAwMDExMDExMDExMTEwMTExMDAxMTAwMTAxMTAxMDEwMTAxMDEwMTEwMTExMDAxMTAxMDAxMDExMDAxMTAwMTEwMTAwMTAxMTAwMTAxMDExMDAxMDAwMTAwMTEwMDAxMTAxMTExMDExMDAxMTEwMTExMDAxMQ) gives us ttps://github.com/mandiant/macos-UnifiedLogs which should obviously be [https://github.com/mandiant/macos-UnifiedLogs](https://github.com/mandiant/macos-UnifiedLogs)

I extract the $MFT file and examine it with 010 Editor just for some more context.

![cipher-4-3](/images/MVS2026/cipher-4-3.png)

In the slack space for the MFT Entry 33, we observe the following:

> 011010..0111010001110100011100000111001100111010001011110010111101100111011010010111010001101000011101010110001000101110011000110110111101101101001011110110110101100001011011100110010001101001011000010110111001110100001011110110110101100001011000110110111101110011001011010101010101101110011010010110011001101001011001010110010001001100011011110110011101110011

It is pretty clear that we are missing some 0s and 1s. 011010 most likely should be 01101000 which translates to "h" completing our URL.

I'm unsure of the hints on this one if it even had any.

Answer: [https://github.com/mandiant/macos-UnifiedLogs](https://github.com/mandiant/macos-UnifiedLogs)

## Bonsoir Elliot

> It's not that Deep, just Sound it out (shout out to int80)
>
> File Download: [HideNSeek.wav](/files/MVS2026/HideNSeek.wav)

We are given a wav file. There is a BIG hint in the description. "It's not that Deep, just Sound it out (shout out to int80)" Points to DeepSound. int80 is the nerdcore rapper.

Using the DeepSound tool from [https://github.com/Jpinsoft/DeepSound](https://github.com/Jpinsoft/DeepSound) allows us to extract a `secret.txt` file which contains [https://dfir.blog/unfurl/](https://dfir.blog/unfurl/).

![cipher-5-1](/images/MVS2026/cipher-5-1.png)

Answer: [https://dfir.blog/unfurl/](https://dfir.blog/unfurl/)

## Cows come in all shapes and sizes

> Moooo! Get the cow to spill the secret! (zip password: moo)
>
> File Download: [moo.zip](/files/MVS2026/moo.zip)

I went really off track with this challenge! What follows are all the steps I took including the missteps.

Running the program shows us [cowsay](https://en.wikipedia.org/wiki/Cowsay). It obviously wants an argument and providing one results in the cow yelling at us about the wrong key.

![cipher-6-0](/images/MVS2026/cipher-6-0.png)

Using [Detect It Easy](https://github.com/horsicq/Detect-It-Easy), I identify the executable as a Python script packed with pyinstaller.

![cipher-6-1](/images/MVS2026/cipher-6-1.png)

I next use [pyinstxtractor](https://github.com/extremecoders-re/pyinstxtractor) to unpack the Python script and find compiled python bytecode.  

![cipher-6-2](/images/MVS2026/cipher-6-2.png)

I take the moo.pyc and upload it to [https://www.pylingual.io/](https://www.pylingual.io/) to decompile the python bytecode. As a sidenote, there are other tools such as [https://github.com/rocky/python-uncompyle6](https://github.com/rocky/python-uncompyle6). Sadly, I missed the author's presentation at BSides NYC 0x05. I get the following:

```Python
# Decompiled with PyLingual (https://pylingual.io)
# Internal filename: 'moo.py'
# Bytecode version: 3.11a7e (3495)
# Source timestamp: 1970-01-01 00:00:00 UTC (0)

import sys
import base64
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC
from cryptography.hazmat.primitives import hashes
from cryptography.exceptions import InvalidTag
from cryptography.hazmat.backends import default_backend
import cowsay
SALT = 'Wx8ah0DALPJAWMS93dm/QA=='
NONCE = 'Tby9ej9/2rDEtRI1'
TAG = 'dDzVexYRxvP/iaVfXA14sg=='
CIPHERTEXT = '3G/8hjWYbdwMGSRLbbOUADqYVYWnZg=='
ITERATIONS = 600000
KEY_SIZE = 32
def decrypt_message(password_attempt):
    try:
        salt = base64.b64decode(SALT)
        nonce = base64.b64decode(NONCE)
        tag = base64.b64decode(TAG)
        ciphertext = base64.b64decode(CIPHERTEXT)
        password = password_attempt.encode('utf-8')
        kdf = PBKDF2HMAC(algorithm=hashes.SHA256(), length=KEY_SIZE, salt=salt, iterations=ITERATIONS, backend=default_backend())
        derived_key = kdf.derive(password)
        decryptor = Cipher(algorithms.AES(derived_key), modes.GCM(nonce, tag), backend=default_backend()).decryptor()
        plaintext = decryptor.update(ciphertext) + decryptor.finalize()
        cowsay.cow(f"Mooooooo! You found the answer!\n{plaintext.decode('utf-8')}")
    except InvalidTag:
        cowsay.cow('Hmmmmm...\n That doesn\'t look right.')
    except Exception as e:
        cowsay.cow(f'What\'s happening?!?!\n{e}')
def main():
    if len(sys.argv)!= 2:
        if len(sys.argv) > 2:
            cowsay.cow('Moooooooooooo!\n TMI!')
        else:
            cowsay.cow('Moooooooooooo!\n Feed me a key!')
        sys.exit(1)
    password_attempt = sys.argv[1]
    decrypt_message(password_attempt)
if __name__ == '__main__':
    main()
```

After reading the code, there is no tricking this as the program itself is essentially a decryptor. I have to give it the encrypted string. Bruteforcing this is also not a realistic option.

Going back to Detect It Easy, I look a little closer at what it has found within the executable. Under the resources section we find some suspect PNG files.

![cipher-6-3](/images/MVS2026/cipher-6-3.png)

I dump them to files, examine them, and find an interestind barcode.

![moo.exe.0009c950_0764.png](/images/MVS2026/moo.exe.0009c950_0764.png)

To read the barcode, I upload it to [https://online-barcode-reader.inliteresearch.com/](https://online-barcode-reader.inliteresearch.com/). This is a Pdf417 barcode which reads as `7e0d78a5af1cae300cddbce0f9be3232f32cc5f428e45c36e52f8087ee0a` and feeding that key to the cow gives:

![cipher-6-4](/images/MVS2026/cipher-6-4.png)

Hexordia provided a small hint on this one at [https://www.linkedin.com/posts/hexordia_mobileforensics-dfir-forensictraining-activity-7430362826969534464-TcPv](https://www.linkedin.com/posts/hexordia_mobileforensics-dfir-forensictraining-activity-7430362826969534464-TcPv). Saying "You may WREStle with this one." which points to RES or the resource section of the exe.

Answer: [https://evanolevm.com/](https://evanolevm.com/)

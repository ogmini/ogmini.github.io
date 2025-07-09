---
layout: post
title: SANS - Ransomware Summit 2025
author: 'ogmini'
tags:
 - DFIR
 - AI
 - Workshop
---

Had the pleasure of listening to some great talks at the online SANS - Ransomware Summit 2025. I particularly enjoyed the Hands-On Workshop put on by Mari DeGrazia titled "Forensic AI, Your Way: A Local LLM Installation". The workshop we participated in is part of the SANS FOR563 course as a lab and you can find more information at [https://for563.com/](https://for563.com/). My main takeaway is that a local LLM can be powerful when used appropriately.

We utilized [https://lightning.ai/](https://lightning.ai/) to handle the GPU processing for the workshop. I'm sure many of us don't have a GPU with enough memory to run some of the better LLMs. In Lightning.AI, we created a studio and installed [Ollama](https://ollama.com/) on it to easily download various LLMs and run them using [Open WebUI](https://github.com/open-webui/open-webui).

The LLMs that we experimented with were:

- llama3.1:8b
- qwen2.5:14b
- deepseek-r1:14b

After some configuration, we asked the various LLMs to analyze csv data to look for malicious or suspicious activity. One of the more enlightening prompts was having the LLMs look at the csv data below and convert the timestamps, translate the messages to english, and provide a summary.

``` csv
activityKey,activityId,externalNumber,number,activityType,content,dataPath,direction,timeStamp
1,c:mW3sMoUmRWOMPx5JIKFmcw,e:+p:Assistant,n:a:OZ4fHzIPQC2RajBR9ZhBpA,1,"<?xml version='1.0' encoding='utf8' standalone='yes' ?><ChatMessage timeStamp=""1705445560504"" activityId=""c:mW3sMoUmRWOMPx5JIKFmcw"" deleted=""false"" from=""e:+p:Assistant"" to=""n:a:OZ4fHzIPQC2RajBR9ZhBpA"" message=""Welcome to Phoner. You can text and call privately and also choose your own phone numbers. Try texting or calling someone now."" error="""" status=""2"" />",,0,1705445560504
2,c:IgbBoDYwQHaBhIFXsA2Tjw,e:+15714099634,n:a:OZ4fHzIPQC2RajBR9ZhBpA,1,"<?xml version='1.0' encoding='utf8' standalone='yes' ?><ChatMessage timeStamp=""1705445702118"" activityId=""c:IgbBoDYwQHaBhIFXsA2Tjw"" from=""n:a:OZ4fHzIPQC2RajBR9ZhBpA"" to=""e:+15714099634"" message=""¿Tienes alguna droga para vender?"" error="""" status=""0"" />",,1,1705445702118
3,c:2sVwhV6FT9WnTF7GEMQWgA,e:+15714099634,n:a:OZ4fHzIPQC2RajBR9ZhBpA,1,"<?xml version='1.0' encoding='utf8' standalone='yes' ?><ChatMessage timeStamp=""1705445757788"" activityId=""c:2sVwhV6FT9WnTF7GEMQWgA"" deleted=""false"" encryptedTo=""379175872c0fd50f5b7d81d68250e395bfc3e2fe37ad8c3dc256d3600efc66f9"" groupSender=""null"" encryptedFrom=""0ccf5ac1ac5497cbd82265aca6ff2459"" from=""e:+15714099634"" to=""n:a:OZ4fHzIPQC2RajBR9ZhBpA"" message=""¿Qué tipo de droga estás buscando y cuánta cantidad deseas?"" error="""" status=""2"" />",,0,1705445757788
4,nl:DIjjoL3sTuSF0trqRWpHeA,e:+15714099634,n:a:OZ4fHzIPQC2RajBR9ZhBpA,18,"<?xml version='1.0' encoding='utf8' standalone='yes' ?><NumberLookup timeStamp=""1705445758692"" activityId=""nl:DIjjoL3sTuSF0trqRWpHeA"" number=""+15714099634"" deleted=""false"" lineInfo="""" callerName=""Occoquan     VA"" />",,0,1705445758692
5,c:ft5b2WSQSIeOlp2zNGuf9A,e:+15714099634,n:a:OZ4fHzIPQC2RajBR9ZhBpA,1,"<?xml version='1.0' encoding='utf8' standalone='yes' ?><ChatMessage timeStamp=""1705445838480"" activityId=""c:ft5b2WSQSIeOlp2zNGuf9A"" from=""n:a:OZ4fHzIPQC2RajBR9ZhBpA"" to=""e:+15714099634"" message=""Tienes algun oxi"" error="""" status=""0"" />",,1,1705445838480
6,c:X9o7b6DIQcOytnNKppYe1A,e:+15714099634,n:a:OZ4fHzIPQC2RajBR9ZhBpA,1,"<?xml version='1.0' encoding='utf8' standalone='yes' ?><ChatMessage timeStamp=""1705445879197"" activityId=""c:X9o7b6DIQcOytnNKppYe1A"" deleted=""false"" from=""e:+15714099634"" to=""n:a:OZ4fHzIPQC2RajBR9ZhBpA"" message=""si, cuanto quieres"" error="""" status=""2"" />",,0,1705445879197
7,c:MyOZEiYLRGu85Yc1q*s8Ew,e:+15714099634,n:a:OZ4fHzIPQC2RajBR9ZhBpA,1,"<?xml version='1.0' encoding='utf8' standalone='yes' ?><ChatMessage timeStamp=""1705445897068"" activityId=""c:MyOZEiYLRGu85Yc1q*s8Ew"" from=""n:a:OZ4fHzIPQC2RajBR9ZhBpA"" to=""e:+15714099634"" message=""Déjame llamarte y podemos hablar de ello."" error="""" status=""0"" />",,1,1705445897068
8,s:N3*ZbgsBQMWYMJpdOQZhJw,e:+15714099634,n:a:OZ4fHzIPQC2RajBR9ZhBpA,2,"<?xml version='1.0' encoding='utf8' standalone='yes' ?><CallHistory timeStamp=""1705446014940"" activityId=""s:N3*ZbgsBQMWYMJpdOQZhJw"" deleted=""false"" minutes=""1"" from=""n:a:OZ4fHzIPQC2RajBR9ZhBpA"" to=""e:+15714099634"" callType=""0"" status=""1"" />",,1,1705446014940
```

## LLM Answers

I prompted all three LLMs with the same prompt and got different answers from all three.

![Qwen](/images/sansransomeware2025/qwen.PNG)

![CyberLlama](/images/sansransomeware2025/cyberllama.PNG)

![Deepseek](/images/sansransomeware2025/deepseek.PNG)

What really leaps out is that all three got the timestamps wrong. Taking a detailed look at the Unix Timestamp of 1705445560504 for the first record which translates to Tue Jan 16 2024 22:52:40 GMT.

| LLM | Answer |
| --- | --- |
| llama3.1:8b | Fri, 24 Aug 2018 21:06:30 GMT |
| qwen2.5:14b | February 22, 2024 18:39:20 UTC |
| deepseek-r1:14b | January 19, 2023 at approximately 1:39:20 PM UTC |

The translations are pretty spot on. I want to call out one interesting result though. There is a message of "Tienes algun oxi". Which do you think is the more correct translation? I would argue deepseek-r1:14b provided the more straight translation while llama3.1:8b read into the shorthand of oxi and made an assumption. qwen2.5:14b seemed to split the two. I prefer the translation from deepseek-r1:14b.

| LLM | Translation |
| --- | --- |
| llama3.1:8b | Do you have any oxycodone? |
| qwen2.5:14b | Do you have any oxy [Oxycodone?] |
| deepseek-r1:14b | Do you have any oxi? |

One last "funny" thing, it appears that the llama3.1:8b LLM thought that the individual was talking to the Phoner assistant and not another contact. It did not understand or take into account the externalNumber column.

## Thoughts

LLMs used appropriately can be a useful tool to quickly triage and get a sense of the data you are looking at. It does NOT replace the need to actually investigate, verify, and understand the context. Do not implicitly trust the LLM.

---
layout: post
title: Magnet Virtual Summit 2026 CTF - AAR "Welcome Home"
author: 'ogmini'
tags:
 - CTF
 - Challenges
 - Writeups
 - ILEAPP
---

![iphone-12.png](/images/MVS2026/iphone-12.png)

This year I'm going to writeup AARs for the challenges that I solved differently from Hexordia or felt needed to be expanded upon due to my approach or thinking.

## Welcome Home

Title: Welcome Home  
Description: What port was the host directed through for a sign-in?

### Thoughts/Hints/Background

- Wifi Network Portal?
- Possibly home network?

### Solution

Let us check browser history for any interesting URLs with a port specified. We can look both in iLEAPP and Magnet Axiom where we find a hit for `http://127.0.0.1:49354/?state=DlZ5NRaWdsqHUiS21f2V&code=4/0ATX87lPypweMwdXMiCRQiFSHNUkw86nymXg8LV6gv 0Tb5fB9gs5iHzJ0fKHicbKzcaMahg&scope=https://www.googleapis.com/auth/flexible-api`. There is an obvious port of 49354 after the IP of 127.0.0.1.

![iphone-solution-12.png](/images/MVS2026/iphone-solution-12.png)

![iphone-solution-12-1.png](/images/MVS2026/iphone-solution-12-1.png)

Answer: 49354

### Takeaways

Interestingly, the iLEAPP plugin from Kevin Pagano reports the Redirect Source and Redirect Direction from the database while Magnet Axiom doesn't show this or at least not by default. I'd like to get confirmation; but it looks like the individual was opened the Google Drive application on their iPhone which utilized Google OAuth 2.0 authorization. I'm intrigued to know if that is correct...

OAuth 2.0 flow seen in Safari History:

1. https://accounts.google.com/o/oauth2/auth?client_id=947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com&redirect_uri=http%3a%2f%2f127.0.0.1%3a49354&state=DlZ5NRaWdsqHUiS21f2V&scope=https%3a%2f%2fwww.googleapis.com%2fauth%2fflexible-api&response_type=code&prompt=consent&access_type=offline&hl=en
2. https://accounts.google.com/v3/signin/identifier?opparams=%253Fhl%253Den&dsh=S-636249750%3A1765316704781958&access_type=offline&client_id=947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com&hl=en&o2v=1&prompt=consent&redirect_uri=http%3A%2F%2F127.0.0.1%3A49354&response_type=code&scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fflexible-api&service=lso&state=DlZ5NRaWdsqHUiS21f2V&flowName=GeneralOAuthFlow&continue=https%3A%2F%2Faccounts.google.com%2Fsignin%2Foauth%2Fconsent%3Fauthuser%3Dunknown%26part%3DAJi8hAPFdT1NN0wXnYLIoXlSLz6dLMkWdrkCm3uH1A8l-mmugkmKezyBfgzsiE8zXCydLvdBxHwla-yXeNM_16MrjupxLKl38c2z77W_0oVdqPHq9Q02PYByV9knOOeXWe2nuvkFNKI7xo_O8apEopOhMOQWN-5N6H9mw4asXLjhMQ8-_aP822iAH33-yBwmToE39ZHBtqBN603MMaSuWr9EMcSIrCoQ8ecnbc7fgOBjnLAL5T0WEvxyWjA-1pyY7O2YAUkMHhBL7A6gerDfiPGvr9IaLwVb_X9osEr0huovWbuTEyI9FWTQSRlLk3DRemddIgizTXdiVM32FTSbyvU7HpJz1L6UFuWPKrHLkvYXrIhGYFWITmvwbgAeLxuxbN0PEbcvDyaSol7IAuwH4jLnqLrFwNiQOyHeOXVEgGhnu4uhw0VSAO1RiUzW2HoClHF7s4xUBwiV%26flowName%3DGeneralOAuthFlow%26hl%3Den%26as%3DS-636249750%253A1765316704781958%26client_id%3D947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com%26requestPath%3D%252Fsignin%252Foauth%252Fconsent%23&app_domain=http%3A%2F%2F127.0.0.1%3A49354&rart=ANgoxccha1Z0OMQinNG2yWgBz3TdLntfNEb93zJaeD_m8LCF8bwb-O3rGx0Kk38qz70aUdfyhXNf-PdeSm-f0UGAhX5zexwpiR247gvWp9SRz2jAmCrDgUY
3. https://accounts.google.com/v3/signin/identifier?opparams=%253Fhl%253Den&dsh=S-636249750%3A1765316704781958&access_type=offline&client_id=947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com&hl=en&o2v=1&prompt=consent&redirect_uri=http%3A%2F%2F127.0.0.1%3A49354&response_type=code&scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fflexible-api&service=lso&state=DlZ5NRaWdsqHUiS21f2V&flowName=GeneralOAuthFlow&continue=https%3A%2F%2Faccounts.google.com%2Fsignin%2Foauth%2Fconsent%3Fauthuser%3Dunknown%26part%3DAJi8hAPFdT1NN0wXnYLIoXlSLz6dLMkWdrkCm3uH1A8l-mmugkmKezyBfgzsiE8zXCydLvdBxHwla-yXeNM_16MrjupxLKl38c2z77W_0oVdqPHq9Q02PYByV9knOOeXWe2nuvkFNKI7xo_O8apEopOhMOQWN-5N6H9mw4asXLjhMQ8-_aP822iAH33-yBwmToE39ZHBtqBN603MMaSuWr9EMcSIrCoQ8ecnbc7fgOBjnLAL5T0WEvxyWjA-1pyY7O2YAUkMHhBL7A6gerDfiPGvr9IaLwVb_X9osEr0huovWbuTEyI9FWTQSRlLk3DRemddIgizTXdiVM32FTSbyvU7HpJz1L6UFuWPKrHLkvYXrIhGYFWITmvwbgAeLxuxbN0PEbcvDyaSol7IAuwH4jLnqLrFwNiQOyHeOXVEgGhnu4uhw0VSAO1RiUzW2HoClHF7s4xUBwiV%26flowName%3DGeneralOAuthFlow%26hl%3Den%26as%3DS-636249750%253A1765316704781958%26client_id%3D947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com%26requestPath%3D%252Fsignin%252Foauth%252Fconsent%23&app_domain=http%3A%2F%2F127.0.0.1%3A49354&rart=ANgoxccha1Z0OMQinNG2yWgBz3TdLntfNEb93zJaeD_m8LCF8bwb-O3rGx0Kk38qz70aUdfyhXNf-PdeSm-f0UGAhX5zexwpiR247gvWp9SRz2jAmCrDgUY
4. https://accounts.google.com/v3/signin/challenge/pwd?TL=AHE1sGXSrbPs5XK7qczFSN_3A5wSNlzgtTkJMiCx5FYvNHd0P567twHphfOuLcqL&access_type=offline&app_domain=http%3A%2F%2F127.0.0.1%3A49354&checkConnection=youtube%3A440&checkedDomains=youtube&cid=3&client_id=947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com&continue=https%3A%2F%2Faccounts.google.com%2Fsignin%2Foauth%2Fconsent%3Fauthuser%3Dunknown%26part%3DAJi8hAPFdT1NN0wXnYLIoXlSLz6dLMkWdrkCm3uH1A8l-mmugkmKezyBfgzsiE8zXCydLvdBxHwla-yXeNM_16MrjupxLKl38c2z77W_0oVdqPHq9Q02PYByV9knOOeXWe2nuvkFNKI7xo_O8apEopOhMOQWN-5N6H9mw4asXLjhMQ8-_aP822iAH33-yBwmToE39ZHBtqBN603MMaSuWr9EMcSIrCoQ8ecnbc7fgOBjnLAL5T0WEvxyWjA-1pyY7O2YAUkMHhBL7A6gerDfiPGvr9IaLwVb_X9osEr0huovWbuTEyI9FWTQSRlLk3DRemddIgizTXdiVM32FTSbyvU7HpJz1L6UFuWPKrHLkvYXrIhGYFWITmvwbgAeLxuxbN0PEbcvDyaSol7IAuwH4jLnqLrFwNiQOyHeOXVEgGhnu4uhw0VSAO1RiUzW2HoClHF7s4xUBwiV%26flowName%3DGeneralOAuthFlow%26hl%3Den%26as%3DS-636249750%253A1765316704781958%26client_id%3D947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com%26requestPath%3D%252Fsignin%252Foauth%252Fconsent%23&dsh=S-636249750%3A1765316704781958&flowName=GeneralOAuthFlow&hl=en&o2v=1&opparams=%253Fhl%253Den&prompt=consent&pstMsg=1&rart=ANgoxccha1Z0OMQinNG2yWgBz3TdLntfNEb93zJaeD_m8LCF8bwb-O3rGx0Kk38qz70aUdfyhXNf-PdeSm-f0UGAhX5zexwpiR247gvWp9SRz2jAmCrDgUY&redirect_uri=http%3A%2F%2F127.0.0.1%3A49354&response_type=code&scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fflexible-api&service=lso&state=DlZ5NRaWdsqHUiS21f2V
5. https://accounts.google.com/CheckCookie?continue=https://accounts.google.com/signin/oauth/consent?authuser%3Dunknown%26part%3DAJi8hAPFdT1NN0wXnYLIoXlSLz6dLMkWdrkCm3uH1A8l-mmugkmKezyBfgzsiE8zXCydLvdBxHwla-yXeNM_16MrjupxLKl38c2z77W_0oVdqPHq9Q02PYByV9knOOeXWe2nuvkFNKI7xo_O8apEopOhMOQWN-5N6H9mw4asXLjhMQ8-_aP822iAH33-yBwmToE39ZHBtqBN603MMaSuWr9EMcSIrCoQ8ecnbc7fgOBjnLAL5T0WEvxyWjA-1pyY7O2YAUkMHhBL7A6gerDfiPGvr9IaLwVb_X9osEr0huovWbuTEyI9FWTQSRlLk3DRemddIgizTXdiVM32FTSbyvU7HpJz1L6UFuWPKrHLkvYXrIhGYFWITmvwbgAeLxuxbN0PEbcvDyaSol7IAuwH4jLnqLrFwNiQOyHeOXVEgGhnu4uhw0VSAO1RiUzW2HoClHF7s4xUBwiV%26flowName%3DGeneralOAuthFlow%26hl%3Den%26as%3DS-636249750:1765316704781958%26client_id%3D947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com%26requestPath%3D/signin/oauth/consent%26rapt%3DAEjHL4PUyETMhvvqvzkP6ZXHANx1UthuIWKOm0hvLuoE5Okxt8sqmdhw4HKfDOUXG49-3NEexg5Z__xCkT3si7Eu1PDQGtkbx4wy8_yaWQCTY2l8gNb5zEo%23&service=lso&hl=en&checkedDomains=youtube&checkConnection=youtube:440&pstMsg=1&rart=ANgoxccha1Z0OMQinNG2yWgBz3TdLntfNEb93zJaeD_m8LCF8bwb-O3rGx0Kk38qz70aUdfyhXNf-PdeSm-f0UGAhX5zexwpiR247gvWp9SRz2jAmCrDgUY&flowName=GeneralOAuthFlow&client_id=947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com&redirect_uri=http://127.0.0.1:49354&scope=https://www.googleapis.com/auth/flexible-api&o2v=1&chtml=LoginDoneHtml&gidl=EgIIAA
6. https://accounts.google.com/signin/oauth/consent?authuser=0&part=AJi8hAPFdT1NN0wXnYLIoXlSLz6dLMkWdrkCm3uH1A8l-mmugkmKezyBfgzsiE8zXCydLvdBxHwla-yXeNM_16MrjupxLKl38c2z77W_0oVdqPHq9Q02PYByV9knOOeXWe2nuvkFNKI7xo_O8apEopOhMOQWN-5N6H9mw4asXLjhMQ8-_aP822iAH33-yBwmToE39ZHBtqBN603MMaSuWr9EMcSIrCoQ8ecnbc7fgOBjnLAL5T0WEvxyWjA-1pyY7O2YAUkMHhBL7A6gerDfiPGvr9IaLwVb_X9osEr0huovWbuTEyI9FWTQSRlLk3DRemddIgizTXdiVM32FTSbyvU7HpJz1L6UFuWPKrHLkvYXrIhGYFWITmvwbgAeLxuxbN0PEbcvDyaSol7IAuwH4jLnqLrFwNiQOyHeOXVEgGhnu4uhw0VSAO1RiUzW2HoClHF7s4xUBwiV&flowName=GeneralOAuthFlow&hl=en&as=S-636249750:1765316704781958&client_id=947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com&requestPath=/signin/oauth/consent&rapt=AEjHL4PUyETMhvvqvzkP6ZXHANx1UthuIWKOm0hvLuoE5Okxt8sqmdhw4HKfDOUXG49-3NEexg5Z__xCkT3si7Eu1PDQGtkbx4wy8_yaWQCTY2l8gNb5zEo#
7. https://accounts.google.com/signin/oauth/consent?authuser=0&part=AJi8hAPFdT1NN0wXnYLIoXlSLz6dLMkWdrkCm3uH1A8l-mmugkmKezyBfgzsiE8zXCydLvdBxHwla-yXeNM_16MrjupxLKl38c2z77W_0oVdqPHq9Q02PYByV9knOOeXWe2nuvkFNKI7xo_O8apEopOhMOQWN-5N6H9mw4asXLjhMQ8-_aP822iAH33-yBwmToE39ZHBtqBN603MMaSuWr9EMcSIrCoQ8ecnbc7fgOBjnLAL5T0WEvxyWjA-1pyY7O2YAUkMHhBL7A6gerDfiPGvr9IaLwVb_X9osEr0huovWbuTEyI9FWTQSRlLk3DRemddIgizTXdiVM32FTSbyvU7HpJz1L6UFuWPKrHLkvYXrIhGYFWITmvwbgAeLxuxbN0PEbcvDyaSol7IAuwH4jLnqLrFwNiQOyHeOXVEgGhnu4uhw0VSAO1RiUzW2HoClHF7s4xUBwiV&flowName=GeneralOAuthFlow&hl=en&as=S-636249750:1765316704781958&client_id=947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com&requestPath=/signin/oauth/consent&rapt=AEjHL4PUyETMhvvqvzkP6ZXHANx1UthuIWKOm0hvLuoE5Okxt8sqmdhw4HKfDOUXG49-3NEexg5Z__xCkT3si7Eu1PDQGtkbx4wy8_yaWQCTY2l8gNb5zEo#
8. https://accounts.google.com/signin/oauth/firstparty/nativeapp?authuser=0&part=AJi8hAPFdT1NN0wXnYLIoXlSLz6dLMkWdrkCm3uH1A8l-mmugkmKezyBfgzsiE8zXCydLvdBxHwla-yXeNM_16MrjupxLKl38c2z77W_0oVdqPHq9Q02PYByV9knOOeXWe2nuvkFNKI7xo_O8apEopOhMOQWN-5N6H9mw4asXLjhMQ8-_aP822iAH33-yBwmToE39ZHBtqBN603MMaSuWr9EMcSIrCoQ8ecnbc7fgOBjnLAL5T0WEvxyWjA-1pyY7O2YAUkMHhBL7A6gerDfiPGvr9IaLwVb_X9osEr0huovWbuTEyI9FWTQSRlLk3DRemddIgizTXdiVM32FTSbyvU7HpJz1L6UFuWPKrHLkvYXrIhGYFWITmvwbgAeLxuxbN0PEbcvDyaSol7IAuwH4jLnqLrFwNiQOyHeOXVEgGhnu4uhw0VSAO1RiUzW2HoClHF7s4xUBwiV&flowName=GeneralOAuthFlow&hl=en&as=S-636249750:1765316704781958&client_id=947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com&requestPath=/signin/oauth/consent&rapt=AEjHL4PUyETMhvvqvzkP6ZXHANx1UthuIWKOm0hvLuoE5Okxt8sqmdhw4HKfDOUXG49-3NEexg5Z__xCkT3si7Eu1PDQGtkbx4wy8_yaWQCTY2l8gNb5zEo
9. http://127.0.0.1:49354/?state=DlZ5NRaWdsqHUiS21f2V&code=4/0ATX87lPypweMwdXMiCRQiFSHNUkw86nymXg8LV6gv0Tb5fB9gs5iHzJ0fKHicbKzcaMahg&scope=https://www.googleapis.com/auth/flexible-api

One of the biggest things to note is the client_id in the query string. This has a value of `947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com`. If we Google that client_id the results possibly point at Google Drive?

[https://support.google.com/drive/thread/125345329/can-t-login-googledrive-backup-and-sync-error-code-400-invalid-request-required-parameter-is-missing?hl=en](https://support.google.com/drive/thread/125345329/can-t-login-googledrive-backup-and-sync-error-code-400-invalid-request-required-parameter-is-missing?hl=en)
[https://www.reddit.com/r/gsuite/comments/1bl4zjo/cannot_sign_in_gdrive_desktop_app/](https://www.reddit.com/r/gsuite/comments/1bl4zjo/cannot_sign_in_gdrive_desktop_app/)

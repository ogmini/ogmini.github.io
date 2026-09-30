---
layout: post
title: Steam Frame - Chaperone
author: 'ogmini'
tags:
 - Steam-Frame
---

Do you need a chaperone? Chaperone is the name for the system that SteamVR uses to establish boundaries for the VR play area. Stops people from bumping into stuff or punching their wall.

> [!WARNING]  
> The research below is ongoing and very much stream of consciousness. At times, I will be theorizing and making jumps to a conclusion. As with any theory, these may prove to be WRONG. Do your own testing/verification. When I am ready and comfortable, I will publish my findings in a more rigorous manner.

I'm going to keep a running list of interesting files:

| Filename | Location | Notes | Post |
| --- | --- | --- | --- |
| chaperone_info.vrchap | /home/steamos/.config/openvr/config/ | Stores information about the chaperone/playspace/boundaries | |

Information about this file can be found on Valve's wiki - [https://developer.valvesoftware.com/wiki/SteamVR/chaperone_info.vrchap](https://developer.valvesoftware.com/wiki/SteamVR/chaperone_info.vrchap). One thing to take note, the documentation calls out lighthouse tracking. The Steam Frame does not use lighthouse tracking. At the moment, I'm not sure how the Steam Frame anchors the collision bounds to a location. It is definitely aware of location in some sense as it loads up the correct chaperone when I'm in the correct place. If I am somewhere new or that it doesn't recognize, I have to create a new chaperone.

You do get a timestamp for when a "universe" was created or modified.

``` json
"time" : "Tue Sep 29 22:50:08 2026",
```

The "universeID" is documented as by default being set to the epoch time during initialization. This is NOT the case on the Steam Frame where I have a "universeID" of:

``` json
"universeID" : "6982239039241517218"
```

This does not appear to be time based. But, I am hoping this identifier might be useful in linking the chaperone to a location and its data representation.

We can extrapolate the user's height from this file. Note, this doesn't appear to be the case for lighthouse tracked devices like the Valve Index and the example linked above.

Each universe is defined by collision bounds which are arrays of [x,y,z] vectors. Taking one of these arrays from my file:

``` json
[
    [ -0.373399854, 0, 1.02530694 ],
    [ -0.373399854, 2.43000007, 1.02530694 ],
    [ -1.06035137, 2.43000007, 0.908716857 ],
    [ -1.06035137, 0, 0.908716857 ]
],
```

In this case, the y is our vertical of 2.43000007 meters.

The standing details are also stored with another [x,y,z] vector translation. Looking at my file again:

``` json
"standing" : {
    "translation" : [ 0.194531888, 0.738694072, 1.2737397 ],
    "yaw" : -0.768006325
},
```

We now have a percentage of 0.738694072 for the y. A little bit of math...

0.738694072 * 2.4300007 = 1.79502711

1.79502711 meters = 5' 10.6"

You now know my height.

## Open Questions

- Does the `chaperone_info.vrchap` file keep old universes? (It probably does)
- If the universe is anchored to a location. How does it know about the location and what can we glean from this information?
- Impact of using the headset in "Streamed" mode
- Best way to acquire data from the device?

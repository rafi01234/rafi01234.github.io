---
layout: post
title: "Out of the fridge and into the fire"
categories: analysis
---

I was talking to my partner the other day about something I had seen making the rounds the day before - [a load of fridges in France had been hacked](https://www.bbc.co.uk/news/articles/crq5nzxv9q4yo). Her first response was "Who's got a fridge that can be hacked??" What followed was ramblings about smart fridges <!-- more -->and what we *think* they are used for - but to be honest I'm still not really sold on the idea. Why do I need an internet-connected fridge to track/manage food stock and temperatures? I can just open the door, use my eyeballs, order stuff if it's not there, and fiddle with the temperature-knob-thing if needs be. It doesn't take long at all, and is something everyone does naturally by this point anyway, but I digress.

The actual reporting was less about smart fridges in the home and more about those in French supermarkets, so slightly different kettle of fish. Fridges and freezers across several hundred supermarkets were instructed by the attackers to heat up overnight - **some up to 50°C!!** As you can imagine, this resulted in spoiled stock, and even fires in some locations.

## How did this happen?

As it turns out, fairly simply.

The fridges are controlled by controllers (who would have guessed!?).

The controllers are managed by system managers (nice).

These particular system managers (Danfoss AK-SM and AK-SC) appear to be at different stages of their lifecycle. The AK-SC series were [phased out and ultimately end-of-life in 2016](https://www.scribd.com/document/398859172/AK-SC255-355-Phase-Out), while the AK-SM series was identified as the immediate replacement for these. However, the AK-SM 800 system manager is [also end-of-life as of 2021](https://assets.danfoss.com/documents/latest/535265/AO540953778522en-010101.pdf).

*Interestingly, in this PDF they outline and describe some previous attacks targeting these end-of-life devices, so this not being fixed or heeded by the victims is another point of irresponsibility.*

These being end-of-life means that they "can no longer be updated to meet the latest regulatory requirements or industry standards", to quote Danfoss directly. As such, they do not receive any software or security updates, and should be replaced by more modern and active models.

*However*, the still-in-life AK-SM 800A system manager was also affected, [reportedly as they were running outdated firmware](https://www.coolingpost.com/world-news/danfoss-moves-to-mitigate-cyberattack/).

***OK, so the controllers weren't very secure. Neither are a lot of things.***

That is true, very astute. 

But, in this instance, the insecure controllers are public-facing, i.e. exposed to the internet, for anyone to access - *provided they have the password*. Likely for engineers or whoever to be able to monitor and manage the units remotely, which makes sense I guess - it would be an arduous task to go and check every fridge and freezer in every supermarket one by one and make changes to each one indiviudally, physically.

The problem for bad actors now is, of course, the password. But not *that* much of a problem, it seems.

The system managers have a login method that goes around the typical username/password verification, leveraging a function called `check_code_of_day`, as discussed in detail by [Claroty's Team82](https://claroty.com/team82/research/freeze-the-controller-defrost-the-food-uncovering-vulnerabilities-in-danfoss-refrigeration-controllers). Essentially, this function creates a precomputed password based on the device's current date and time. Team82 were able to reverse engineer the function and figure out how this password was generated exactly, meaning that, technically, anyone capable of recreating the algorithm would be able to create a valid password, bypassing the need for the regular login process entirely and granting immediate admin access.

This is kind of insane of Danfoss to have left in. *It's basically an open backdoor to their system managers, with full admin control.* Granted, it was [patched in 2025](https://www.danfoss.com/en/service-and-support/coordinated-vulnerability-disclosure/danfoss-security-advisories/dsa-2025-03-01/), but still for it to even exist for any length of time in the wild is a massive oversight. 

***However***, as a large portion of the system managers that were compromised were end-of-life, they did not get this patch. And as the still-in-life system managers were running outdated firmware, they also did not get this patch. Therefore, all the threat actors had to do was figure out the algorithm and open the doors.

This attack, as it turns out, was not limited to Danfoss system managers. France's CERT-FR (Computer Emergency Response Team) reported that there were [many compromises of both the Danfoss system monitors as well as Copeland/Dixell xweb](https://cert.cyber.gouv.fr/alerte/CERTFR-2026-ALE-012/) - an alternative that does pretty much the same thing. There is not as much detail surrounding the Copeland/Dixell compromises, but I suspect it will be something to do with these similar [authentication bypass vulnerabilities](https://www.cisa.gov/news-events/ics-advisories/icsa-26-057-10).

The motivation behind the attack is currently unclear, so we can't exactly say why this happened, or even who did it, but the how is evident.

## What does this mean?

It means that supermarkets and other places that use these devices need to patch, update, and/or replace them as appropriate. They also need to take the devices off the publicly accessible internet, and instead use a VPN or something similar for limited, specified remote access. 

According to [Shodan](https://www.shodan.io) (a search engine for internet-conencted devices), there are over 5,000 Danfoss devices exposed to the internet, so not a small number.

![Internet-connected Danfoss devices over time](/assets/images/Shodan-Danfoss-Over-Time.png)

Interestingly, the majority of these are in France. 

![Internet-connected Danfoss devices world map](/assets/images/Shodan-Danfoss-Oct-26.png)

Considering CERT-FR states in their advisory that attacks are likely still ongoing targeting this, I'm sure we will see more reports of these in the near future.

More stats from Shodan on this can be found [here](https://trends.shodan.io/search?query=http.favicon.hash:-853297641#overview), although you may need to create a (free!) account to access it.

Another interesting and eerily similar report I saw discussed some recent failures/issues impacting some US military installations in August. I won't go into the post in too much detail here as I recommend you giving it a read [here](https://signalandsilence.substack.com/p/i-think-someone-hacked-the-commissary) instead. But basically, some fridges at some US military bases, *that may or may not be using Danfoss AK-SM controllers*, reported to reach 180°F (~82°C) at around the same time, resulting in spoiled goods.

For the regular member of the public, so *you* probably, this likely doesn't mean too much. You may be inconvenienced and have to go to another shop to do your grocery shopping, but outside of that, just be cautious of things you expose to the internet - I'm sure someone will find a similar vulnerability in smart fridges at some point if they haven't already. **So I guess the question is, do you really need one?**

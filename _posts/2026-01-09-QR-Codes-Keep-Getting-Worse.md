---
layout: post
title: "QR codes keep getting worse"
categories: research
---

I came across [this research by Steve Legere](https://sansorg.egnyte.com/dl/7cfd6RM3TyHM) a few weeks ago on how QR codes can bypass enterprise email gateways, and it certainly piqued my interest.

I already have a great distrust of QR codes (*due to countless reporting of b bad actors replacing QR codes with malicious ones in public spaces*), and for the most part don't really see the point in them (*why do I need to scan one AND be connected to the internet to read a menu, I'm just trying to eat*). If they are taking me to a specific URL that is tailored to my access, that makes more sense; but for generic URLs *please* just give me the link.

## What's in a QR code?

A quick background on QR codes for those who want to know: *QR* actually stands for *Quick Response*, and they were invented in 1994 by engineer Masahiro Hara at Denso Wave in order to track car parts more efficiently during manufacturing. I won't go into the hyper-specifics of how they work, but generally they have five key areas/patterns: 

<div style="display: flex; align-items: flex-start; gap: 20px; margin: 20px 0;">
  <img src="/assets/images/QR-code-patterns.png" alt="QR code patterns" style="width: 250px; flex-shrink: 0; border-radius: 8px; margin-bottom: 0;" />
  <ol style="margin-top: 0; padding-left: 20px;">
    <li><p style="color: #A72929; margin: 0 0 10px 0;">Finder Patterns - the three large corner squares, to identify and orientate. </p><p style="color: #7ED7AE; margin: 0;">These are always surrounded by Separators (empty lines).</p></li>
    <li>Alignment Patterns - smaller squares to allow the reader to measure how the grid might be skewed/warped. These aren't always present on smaller QR codes.</li>
    <li><p style="color: #C1AA9C; margin: 0;">Timing Patterns - alternating black and white pixels between the corners, to measure the size of the grid.</p></li>
    <li>Data Area - the main block of pixels that holds the actual data.</li>
    <li><p style="color: #2C4B80; margin: 0;">Format Information - determines mask pattern and error correction level.</p></li>
    <li>Quiet Zone - the white border around a QR code, to identify where it begins/ends.</p></li>
  </ol>
</div>

For further reading [check out this site](https://qr.blinry.org/). They also go into how to read QR codes by hand, which is probably not too useful in day-to-day but a nifty party trick nonetheless.

On top of the above, there are  two model variations of this type of QR code - aptly named Model 1 and Model 2, with Model 1 being the OG and Model 2 being created as an improvement to Model 1 and can be read smoothly even if distorted (see [QRcode.com](https://www.qrcode.com/en/codes/model12.html)).

<div style="display: flex; gap: 20px; justify-content: center; align-items: center; margin: 20px 0;">
  <img src="/assets/images/QR Model 1.png" alt="QR Model 1" style="width: 48%; height: auto; border-radius: 8px;" />
  <img src="/assets/images/QR Model 2.png" alt="QR Model 2" style="width: 48%; height: auto; border-radius: 8px;" />
</div>

To go even further, there are technically [four other QR code variations](https://www.qrcode.com/en/codes/), each with their own distinct features and use cases, which you can explore at your own leisure.

## What's new?

Now onto where the research comes in. Legere found that they could combine and overlay two distinct Model 2 QR codes on top of each other into a single image, with one acting as the primary (base) module and the overlay as the sub-module. 

![Combining two QR codes into a single QR code](/assets/images/Combining-two-QR-codes-into-a-single-QR-code.png)

This creates a *dual-module* QR code where the output can be different depending on which decoder is used - some decoders will process the primary module and others the sub-module instead.

Legere tested their dual-module QR codes against multiple different mobile devices and email security gateways, and found that, for the mobile devices, the malicious URL (primary module) was extracted, while the benign URL in the sub-module was extracted by the email gateway.

## What does this mean?

With the recent rise of QR code popularity, particularly in phishing attacks over the last few years, this marks a significant evolution. Most security tools nowadays are designed to decode only one module of a dual-module QR code, thereby potentially allowing attackers to deliver a malicious URL or payload to a victim undetected. This could also lead to a security tool falsely labelling the medium that the QR code was in (email/attachment etc.) as non-malicious as it sees only a benign payload on scanning.

Basically, all this waffle to say there is not much you can do, really. I don't expect we will see much of this day-to-day in public spaces and the like; it'll likely be limited to enterprise environments where threat actors tend to use more sophisticated techniques. Hopefully, if you don't already do so, you will start to approach QR codes with the caution they deserve.

Legere has written a blog post on their website (which you can find [here](https://itsec.blog/posts/qrupt0r/)), and has also published [a tool](https://github.com/steve-legere/qrupt0r) that creates a dual-module QR code from two URLs, *for testing purposes, of course*.

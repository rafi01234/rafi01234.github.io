---
layout: post
title: "QR codes keep getting worse"
categories: research
---

I came across [this research](https://sansorg.egnyte.com/dl/7cfd6RM3TyHM) a few weeks ago and it certainly piqued my interest.

I already have a great distrust of QR codes, and for the most part don't really see the point in them (*why do I need to scan one AND be connected to the internet to read a menu, I'm just trying to eat*). If they are taking me to a specific URL that is tailored to my access, that makes more sense; but for generic URLs *please* just give me the link.

A quick background on QR codes for those who want to know: *QR* actually stands for *Quick Response*, and they were invented in 1994 by engineer Masahiro Hara at Denso Wave in order to track car parts more efficiently during manufacturing. I won't go into the specifics of how they work, but generally they have five key areas/patterns: 

<div style="overflow: auto; margin: 20px 0;">
  <img src="/assets/images/QR-code-patterns.png" alt="QR code patterns" style="float: left; width: 250px; margin-right: 20px; margin-bottom: 10px; border-radius: 8px;" />
  <ol style="margin-top: 0; padding-left: 20px;">
    <li><p style="color: #A72929;">Finder Patterns - the three large corner squares, to identify and orientate. </p><p style="color: #7ED7AE;">These are always surrounded by Separators (empty lines).</p></li>
    <li>Alignment Patterns - smaller squares to allow the reader to measure how the grid might be skewed/warped. These are not always present on smaller QR codes.</li>
    <li><p style="color: #C1AA9C;">Timing Patterns - alternating black and white pixels between the corners, to measure the size of the grid.</p></li>
    <li>Data Area - the main block of pixels that holds the actual data.</li>
    <li><p style="color: #2C4B80;">Format Information - determines mask pattern and error correction level.</p></li>
    <li><p style="color: #7ED7AE;">Quiet Zone - the white border around a QR code, to identify where it begins/ends.</p></li>
  </ol>
</div>

For further reading [check out this site](https://qr.blinry.org/). They also go into how to read QR codes by hand, which is probably not too useful in day-to-day but a nifty party trick nonetheless.

On top of the above, there are also currently two model variations of QR codes - aptly named Model 1 and Model 2, with Model 1 being the OG and Model 2 being created as an improvement to Model 1 and can be read smoothly even if distorted (see [QRcode.com](https://www.qrcode.com/en/codes/model12.html))

<div style="display: flex; gap: 20px; justify-content: center; align-items: center; margin: 20px 0;">
  <img src="/assets/images/QR Model 1.png" alt="QR Model 1" style="width: 48%; height: auto; border-radius: 8px;" />
  <img src="/assets/images/QR Model 2.png" alt="QR Model 2" style="width: 48%; height: auto; border-radius: 8px;" />
</div>

Now onto where the research comes in. The researcher found that they could combine and overlay two distinct Model 1 and Model 2 QR codes on top of each other into a single image, with one acting as the primary module and the overlay as the sub-module. 

![Combining two QR codes into a single QR code](/assets/images/Combining-two-QR-codes-into-a-single-QR-code.png)

This creates a dual-module QR code where the output can be different depending on which decoder is used - some decoders will process the primary module and others the sub-module instead.

With the observed rise of QR code popularity, particularly in phishing attacks over the last few years, this poses a significant threat. Most security tools nowadays are designed to decode only one module of the dual-module QR code, thereby potentially allowing attackers to deliver a malicious URL or payload to a victim undetected. This could also lead to a security tool falsely labelling the medium that the QR code was in (email/attachment etc.) as not malicious as it sees only a benign payload on scanning.

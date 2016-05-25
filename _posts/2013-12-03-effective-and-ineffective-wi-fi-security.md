---
layout: post
title: Effective and ineffective Wi-Fi security
excerpt: Which methods to secure a Wi-Fi network actually work — and which are just bumps on the road to an attacker gaining full access to your private network?
---

Modern Wi-Fi access points have a plethora of features available to you for securing your wireless network. Unfortunately, only a few of these are effective at actually stopping sophisticated network intruders with any level of success.

## Why worry about Wi-Fi security?

If an unauthorised person gains access to your network, they can cause a lot of trouble. This includes:

* Using your network connection for access to other networks (e.g. the internet). If they were to download illegal material (e.g. copyrighted content), it would be traced back to *you*, and you would be responsible for their activities.
* Spoofing your wireless access point (e.g. your broadband router) in order to watch, block or alter all network traffic being broadcast to and from all devices on your network, including redirecting you to malicious websites or attempting to infect your device with malware. This is called a Man In the Middle (MITM) attack.
* Gaining access to all unprotected network resources, such as network drives and printers.

Thankfully, there are a few options available to us to protect ourselves. Some are more effective than others!

## Hiding the network by not broadcasting the SSID

**Effectiveness**: 1/5

Each Wi-Fi network has an associated [SSID](http://www.webopedia.com/TERM/S/SSID.html) (service set identifier), usually seen as the network name, which differentiates one Wi-Fi network from another. The SSID is the name you see when picking which network to connect to from a list on your device.

![SSIDs](assets/images/SSIDs.png)

By not broadcasting the SSID from the access point, you're removing the network from this list. The theory is that if you don't know the network's SSID, you won't be able to connect to the network in the first place.

Unfortunately, this stops only the most naïve of attackers.Setting aside the fact that this is just an attempt to obscure your network, and not actually providing any security, the SSID is still included in plaintext within the network packets being transmitted between wireless clients and the access point. So, even though the broadcast packets are no longer being sent, an attacker can easily fire up a packet sniffing tool and grab your SSID.

![Hidden SSID](assets/images/hidden-SSID.png)

Then, it's just a matter of entering it manually.

## Whitelisting clients by performing MAC address filtering

**Effectiveness**: 2/5

Each network device has a hard-coded 48-bit [MAC](https://en.wikipedia.org/wiki/MAC_address) (Media Access Control) address, which is used as a unique identifier to enable devices to be individually addressed on a network. Permitting only certain MAC addresses to communicate with the access point means that only the devices you choose can connect to the network.

![Whitelisted MAC addresses](assets/images/Whitelisted-MAC-addresses.png)

…at least, that’s the theory. While this is true, it relies on the assumption that a MAC address is unspoofable. Network sniffing tools allow hackers to easily change their MAC address to something other than the default “hard-coded” address. And, since an existing client’s MAC address is already broadcast in the clear (just like the network SSID), it is trivial for an attacker to copy an already-whitelisted MAC address and access the network.

![MAC address capture](assets/images/MAC-address-capture.png)

## Securing the network with encryption

There are a few different encryption algorithms that are generally available to consumers and businesses.

### WEP encryption

**Effectiveness**: 2/5

[WEP](https://en.wikipedia.org/wiki/Wired_Equivalent_Privacy) (Wired Equivalent Privacy) is an encryption algorithm that was introduced along with the original 802.11 standard. Unfortunately, it is a long way from providing the equivalent of wired security!

WEP works on the concept of a pre-shared key, usually in the form of a passphrase that is known to both endpoints. In the consumer market this key is usually printed onto a router’s case, but you are also able to manually set the key to something else of your choosing.

There have been many security issues with WEP over the years, which means that it has now been deprecated in favour of other, more secure encryption algorithms. In fact, with modern hardware, it is possible to obtain the encryption key to any WEP-protected network in [under 60 seconds](http://eprint.iacr.org/2007/120.pdf).

### WPA encryption

**Effectiveness**: 4/5

WPA (Wi-Fi Protected Access) was developed in response to the weaknesses found in WEP. It also implemented a new protocol, [TKIP](http://en.wikipedia.org/wiki/Temporal_Key_Integrity_Protocol) (Temporal Key Integrity Protocol), as part of the 802.11i standard. Although WPA also makes use of a pre-shared key, and still uses the RC4 cipher, it does not suffer from the same cipher flaw as implemented by WEP.

Apart from some minor vulnerabilities that have been discovered over the years, the WPA protocol has for the most part remained robust since its introduction in 1999. However, while there have not yet been any serious vulnerabilities found in WPA's implementation, its security may be compromised by a major security flaw found in [WPS](http://en.wikipedia.org/wiki/Wi-Fi_Protected_Setup) (Wi-Fi Protected Setup) in 2011. The technical vulnerability present in WPS will be the subject of a future post, but the end result is that access points implementing WPS can have their WPS PIN [cracked in under 4 hours using a tool called Reaver](https://code.google.com/p/reaver-wps/), allowing an attacker full access to the encrypted network. (To make things worse, the "Disable WPS" option many manufacturers made available in their configuration screens *didn't actually disable WPS*!)

### WPA2 Personal encryption

**Effectiveness**: 5/5

WPA2 is the natural evolution of WPA. The main difference between WPA and WPA2 is the implementation of an AES cipher for wireless encryption rather than RC4. In the same way that WEP and WPA use a key, the "Personal" variant of WPA2 utilises a pre-shared key to perform authentication. And, while WPA2 still suffers from the weakness in WPS, the WPA2 protocol itself has so far been bulletproof, and is currently considered the gold standard in consumer Wi-Fi technology.

### WPA2 Enterprise encryption

**Effectiveness**: 5/5

WPA2 Enterprise is identical to WPA2 Personal in almost every way, the only differences being the use of [CCMP](http://www.tech-faq.com/ccmp-counter-mode-with-cipher-block-chaining-message-authentication-code-protocol.html) and the method of authentication. Instead of utilising a single pre-shared key, the access point can make use of an enterprise service such as a RADIUS server to perform client authentication. The client is then sent a unique encryption key that they can use to connect to the network as normal. This has the added advantage that a user's use of the network can be controlled and revoked as desired by the network administrator.

## Summary

1. It is important to secure your wireless network using the correct techniques, as some offer much better security than others.
2. WPA2 encryption (Personal or Enterprise) is the best way to protect a wireless network from intruders. WPA encryption can also be used as a reliable fallback if not all devices on the network support WPA2.

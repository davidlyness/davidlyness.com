---
layout: post
title: Terribly convenient biometric security
excerpt: Biometric authentication methods — fingerprint readers, iris scans and facial recognition — are a massive convenience. But you shouldn't be relying on them.
---

An [article written by Dustin Kirkland](http://blog.dustinkirkland.com/2013/10/fingerprints-are-user-names-not.html) prompted me to think more about methods of biometric authentication and their place in society. While undoubtedly convenient, I believe that they are not (and can never be) a drop-in replacement for traditional hardware- or password-based authentication.

Aside from being potentially prohibitively expensive to implement, there are a surprisingly large number of major drawbacks that present themselves when you depend on a biometric authentication solution.

## You can’t revoke biometric information

Let’s say someone nefarious gains access to your raw biometric data – for example, exploiting a poorly implemented biometrics system or someone scanning and reproducing your biometric features without your knowledge. If you found out they knew your password, you would change it. (And how often nowadays do you receive emails from online services advising you to do just that?) If you found out they had a copy of your code-generating token device, you’d replace or re-key it. But it’s game over as soon as they get their hands on your biometric data – short of some [fairly invasive and expensive transplant surgery](https://en.wikipedia.org/wiki/Minority_Report_%28film%29#Plot), there’s no way for you to ever decouple that biometric information from your identity.

## You run the risk of very literal identity theft

When there’s a movie that includes a room locked with a biometric scanner, you can be pretty sure that someone’s going to lose a finger or an eye soon. While it’s (hopefully!) not going to affect most of us, those high-risk/high-reward systems that have historically been secured by biometric devices will always be at risk from the more surgically inclined criminals.

## There’s the potential for permanent loss of access

If the worst happens and you lose a finger or an eye in an accident (or if it’s taken from you as above), you can no longer authenticate yourself to any system that is secured solely with your biometric information. Without a backup method of authentication (e.g. a password or email verification), you have no way of proving that you have any greater claim to your identity than any other person.

Even if you’re cautious and manage to keep all your limbs, even superficial markings can cause difficulties authenticating with biometric devices. Recent technologies – such as that employed by [Apple’s Touch ID sensor](https://en.wikipedia.org/wiki/Touch_ID) – are able to scan the [sub-epidermal layer of skin on a finger](https://support.apple.com/en-bn/HT204587), allowing them to continue to function despite superficial injuries.

## Some people just can’t use biometric scanners

A small proportion of people – whether due to age, ethnicity or other factors – are unable to register biometric information such as fingerprints with current scanner technology. For example, in a [study conducted by the UK Passport Service](http://www.dematerialisedid.com/PDFs/UKPSBiometrics_Enrolment_Trial_Report.pdf), the successful authentication rate for able-bodied participants was:

* 69% for facial recognition,
* 96% for iris verification, and
* 81% for fingerprint verification.

It was also the case that older participants were more likely to fail to authenticate their fingerprint than younger participants, suggesting a deterioration in the quality of the finger scan.

## You leave your biometric data behind everywhere you go

If you touch something, you leave your fingerprints behind. Skin cells and hairs that fall off your body every day contain your DNA. High-resolution cameras can capture a detailed iris scan. And while scanner technology is always improving and becoming better at recognising duplicated biometric data, [time](https://pacsec.jp/psj06/psj06krissler-e.pdf) and [again](http://www.ccc.de/en/updates/2013/ccc-breaks-apple-touchid) the sensors are fooled.

## It’s very difficult to maintain identity separation between services

Let’s say you want to authenticate yourself to multiple systems using your biometric data. If you use the same biometric data to authenticate yourself to both your bank and ScamCorp, it’s akin to using the same password on multiple websites – a data breach or malicious employee at one business will leave you vulnerable everywhere else. Your only option in this case is to give everyone different biometric data – but given that most people have one face, two eyes and ten fingers, your level of potential exposure will remain high.

## Biometric technology is encumbered by hardware and software patents

Biometric technology is laden with patents, so much so that new players in the industry are often forced to buy up existing businesses for their intellectual property rather than for their talent or market position. For example, AuthenTec went on an acquisition spree – acquiring or merging with EzValidation, Atrua Technologies, SafeNet and UPEK, before being [acquired by Apple in 2012](http://www.reuters.com/article/us-authentec-acquisition-apple-idUSBRE86Q0KD20120727). This has the effect of monopolising the market, effectively limiting competition and innovation – so much so that I doubt the biometric technology that we have today will see much improvement in the near future without external influence. (In general, I believe that software patents should go away – but that’s a bigger topic for another time.)

## It’s impossible to remain anonymous

If biometric identification is required, you as an individual are permanently tying yourself to any identities you create. While creating a “throwaway” account and password will preserve your anonymity from all but the most highly-equipped adversary, there is no such protection afforded once biometrics are involved.

## Collecting and verifying biometric data can be socially intrusive

The mass collection of biometric data can undermine the notion of “innocent until proven guilty” in society. While in the past police officers would need “probable cause” before arresting and fingerprinting suspects, the tradition shifts when the state implements biometric authentication schemes. People will expect to (and are expected to) submit to biometric identification at government offices, where it is likely integrated with criminal, social and other databases.

## TL;DR

Biometric authentication should be a misnomer and you shouldn’t depend exclusively on it to establish your identity. But you’ll probably still use it because it’s so convenient!

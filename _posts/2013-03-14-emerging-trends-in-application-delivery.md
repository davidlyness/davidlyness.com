---
layout: post
title: Emerging trends in application delivery
excerpt: A look back at how application delivery has evolved, and how we can expect it to change in the coming years.
redirect_from: /emerging-trends-application-delivery
---

I've been contemplating the evolution of application delivery trends and the direction the technology industry could take in the next few years.

## Users are now centric to a system's behaviour

Not so long ago, applications required users to conform to the nature of the system (think command line prompts and green screens), and the machines demanded a relatively high degree of technical knowledge in order to make the system function effectively. Soon afterwards, the client / server model introduced the GUI with concepts like buttons, drop-down menus and point-and-click functions. However, little thought went into the user experience — how often have you come across an application that provides a drop-down menu with 100's of results? The assumption was still that a subgroup of users would be trained to work with the system.

Now think of the web — anyone with a computer and internet connection can access and use your application. And even more recently with the shift to mobile devices — where location, social networks and the current environment all play a part in the expectation of how an application should behave — **the user, not the system, is now central to dictating how an application should behave**. There will be resistance to this change, as people who grew up experiencing the mainframe or client / server model may have an ingrained concept of how a system *should* behave, and will resist this movement or write it off as a fad. But recent entrants to the workforce — *and every generation to follow* — will have this radically different expectation for how systems should work and behave.

![Centricity](assets/images/centricity.png)

## Mobile apps are disrupting traditional business processes

> *“Effort on mobile app development will outstrip PC application development by 4:1 by 2015”* — Gartner Research

On the flip-side, business that can take advantage of this changing environment can reap immense rewards. They can take advantage of location, environment sensors, user data and new technologies like NFC to create new experiences for their users. For example, in just a few years a marketing firm could viably use a user's location, combined with their purchase history, to send them promotions while they are in a store, even based on which aisle they are shopping in. Or companies like [Uber](https://www.uber.com/), who are leveraging mobile devices to give customers the ability to schedule a taxi pickup, track the car and then pay for the whole service. Or, how in 2009 [USAA made use of smart phones and cameras to speed up the process of depositing cheques into your bank account](http://www.nytimes.com/2009/08/10/technology/10check.html) — now something that almost all US banks copy, and a feature that will no doubt be introduced in the UK before long.

This also has huge implications for quality assurance. Putting users in the driving seat, combined with the huge array of instantly-available competitors, users will very quickly move to another product if the one they are currently using does not perform as expected. In fact, according to Gartner, **12% of users will switch instantly when encountering poor performance if a competing product is available to them**.

At the same time, these performance failures are increasingly visible to customers even before they have had a chance to try out the product for themselves — marketplaces like the Apple App Store and Google Play place app reviews front and centre on the product pages. And with [high-profile consumer product failures receiving increasing focus from the media](http://www.techradar.com/news/phone-and-communications/mobile-phones/apple-admits-maps-failure-suggests-users-try-competitor-apps-1100534), it's more important than ever to ensure the quality of an application before it goes out the door and into customers' hands.

## Becoming agile and delivering continuously

> *"Water-Scrum-fall is the reality of agile"* -- Dave West, [SD Times](http://www.sdtimes.com/default.aspx)

How often are features and code changes written as part of an agile release cycle, only to encounter a brick wall when it comes to pushing these changes to Production? All too often such changes are met by processes, approvals, paperwork and change windows — which, although absolutely necessary to the stability of the Production environment, introduce a backlog or delay to pushing out these much-needed features.

![Release delay](assets/images/release-delay.png)
*(Source: Forrester Research Inc., “Five Ways to Streamline Release Management”, February 2011)*

This behaviour can be born out of a dichotomy between development and operational staff. In fact, when companies were asked to describe the relationship between application development teams and IT operations, the results were unsettling.

![DevOps collaboration](assets/images/devops-collaboration.png)
*(Source: Gartner, “Catalysts Signal the Growth of DevOps”, February 2011)*

How can we go about solving this issue – where on one side the agile delivery teams are pushing for faster delivery, and on the other the IT operations staff are attempting to ensure the continued stability of the product as a whole?

1. **Automation**. Whether this be with application building, testing or deployment, correctly implemented automation is much more effective than expending manual effort, and vastly reduces the risk of human error.
2. **Quality**. If developers want to push code changes through to Production faster, they must ensure (and build a reputation of) quality. This means stable changes that are properly documented and traceable throughout their life cycle.
3. **Collaboration**. Without an active atmosphere of collaboration between application developers and operations staff, there will always be friction – no matter how high-quality or automated the processes are.
4. **Governance**. Lack of a proper governance structure will mean that motivation will not be maintained for the above three factors in the longer term.

**Agile needs to extend beyond application development to operations**. An attitude change from *“Be more agile, deliver faster”* to *“Build to run“*, and from *“Change is evil”* to *“Regular releases reduce risk“*, will help enable continuous, stable delivery. With the **speed of release cycles set to increase tenfold over the next 5 to 7 years**, businesses need to ensure they are addressing this problem now to avoid a crisis in the future.

## Applications are hybrid compositions
> *“By 2016, 50% of projects will include both on-premises applications and cloud services”* — Gartner Research

Businesses are changing more rapidly than ever before, and applications are changing more often to better serve the business. As a result, products are not being developed solely in-house. In fact, based on the statistics below a large proportion of product code is outsourced, or comes from open source providers or third-party vendors.

![Code sources](assets/images/code-sources.png)

This is in large part due to processes and composite applications containing **transactions and call-outs to third-party services**. For example, in an e-commerce application there is a GUI to locate and order an item, and then pay with a credit card. Behind the scenes, components deal with updating stock inventories, sending the order for fulfilment, performing a credit check and taking payment from the credit card provider. With such call-outs to external sources requiring testing every time a change is made to the process (for example, adding a new product line or supporting customers in additional countries), and **fees often being charged per transaction** (e.g. credit checking), such testing is often scaled-down or skipped. ** High-volume load testing** is especially susceptible to this, due to the nature of the volume of transactions. What is a tester to do?

The answer is service virtualisation. Instead of calling out to an external source when a system is under test, we can effectively “stub out” these services and point the call-outs to our own servers, making such checks available in Development and Test environments just as they are in Production. A business experiences other benefits as well as a QA cost reduction – availability of these services is now within the business’s control, testing can be automated at a much lower level of risk and earlier in the release cycle, and sharing standardised virtualised services across an enterprise can vastly reduce the cost and effort of maintaining multiple services externally.

## Summary

1. Users, not systems, now dictate how products should function.
2. Huge rewards are available for businesses taking advantage of mobile, but can come at the cost of increased competition and instant transitions to competitors if the app does not perform as expected.
3. Continuous delivery needs to be embraced by both application development teams and operations staff to take advantage of future opportunities.
4. Applications are a hybrid collection of components, which brings new challenges for development and QA teams.

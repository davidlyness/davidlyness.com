---
layout: post
title: Twitter OAuth
excerpt: How OAuth works, using Twitter as an example, and the advantages it affords over basic authentication.
---

A practice that you've no doubt come across in the past when signing up for a new website is the "*Let us send emails to all of your friends so that we can invite them to join too*!" activity. Most often, this is accomplished by you giving the website your username and password, which then starts an automated process of them logging into your account, scanning your contact list for email addresses and then sending an email to each and every one. Aside from encouraging spam, it also raises the question of security. You may trust Facebook or Google with your email credentials, but what about other websites that may not have as much to lose from a privacy breach? And aren't we all told never to give away our passwords anyway?

The same issue is encountered by Twitter application developers. There are many, many Twitter applications out there — desktop applications, browser plugins, mobile apps and so on. In order to post from your account they obviously need to be "logged in" as you. One way of doing this is giving them your username and password (*Basic Authentication*) — and if you're testing out some of the many existing applications, more than a handful of developers now potentially have access to your Twitter account. This may not be the most high-security target in the world, but it illustrates the point.

The current answer to this is [OAuth](http://en.wikipedia.org/wiki/OAuth). Wikipedia's definition of OAuth is "*an open protocol that allows users to share their private resources (e.g. photos, videos, contact lists) stored on one site with another site without having to hand out their username and password*". The following diagrams serve to illustrate the difference between Basic and OAuth Authentication.

![Basic authentication](assets/images/basic-authentication.png)

![OAuth authentication](assets/images/oauth-authentication.png)

Obviously Step 1 in Basic Authentication and Steps 1-5 in OAuth Authentication only need to happen once. Furthermore, we can derive several advantages from the use of OAuth:

* No application ever has access to your password, so if you use the same password across different sites your identity is not compromised.
* The server can revoke access to any application at any time by removing its authorisation token from its database — useful for when the application is ever found doing anything non-professional.
* The user can see an entire list of applications that have access to their account, and what privileges they have. They can also revoke access at the click of a button.
* Changing the password does not prevent these applications from functioning.

According to Twitter's [official blog](http://blog.twitter.com/2010/06/switching-to-oauth.html), the move from Basic Authentication to OAuth Authentication was supposed to happen on 30th June. However, in order to avoid any kind of major outage during the 2010 World Cup, it was postponed until 31st August — which is why around that time you probably saw messages along the lines of "The Twitter API is down" in the sidebar. I didn't think that they'd go so far as to stop pulling in public tweets that don't require a username / password in the first place, but they did. Such is life. If you're interested as to how the sidebar widget is now working, it is scanning the RSS feed and parsing the result in order to display just the most recent tweets.

Currently in development is the [OAuth 2.0 specification](http://oauth.net/2/), which focuses more on specific authorisation flows for various hardware devices — take a look at the [draft specification](http://tools.ietf.org/html/draft-ietf-oauth-v2-10).
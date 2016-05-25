---
layout: post
title: Email address obfuscation
excerpt: A description of email obfuscation techniques in use today, and an evidence-based comparison of which ones work.
---

Although less common nowadays with the pervasion of social media, you still see people posting their email addresses online and obfuscating them in some way. For example, `user@example.com` may be displayed as `user[at]example[dot]com` or `user@example.com.nospam`. The purpose of this obfuscation is to allow regular visitors to learn your email address, but prevent automated bots from harvesting it to send you spam. There are many such techniques — both effective and ineffective — for obfuscating email addresses, and since I couldn't find any quantifiable evidence of a comparison between the various methods I decided to compile some evidence myself.

## The experiment

In 2010, I set up a web page containing 9 `davidlyness.com` email addresses — one in plaintext and each other obfuscated using a different method. I then directed each of these email addresses to separate mail accounts, and ensured that no received messages were marked and deleted as spam by the email software. I ensured it was indexed by Google and other search engines by including a hidden link on this website's homepage and including the page in the sitemap. Then I let two years pass, allowing bots to harvest the email addresses and send spam.

## Obfuscation methods

### Plaintext (control)

This email address was displayed in plaintext to act as a baseline for the other addresses.

### Use [at] and [dot]

Nothing too special going on here — we simply replace the `@` with `[at]` and the `.` with `[dot]` as described above.

### Add a `nospam` comment

Also shown above, add a portion of the email address that the sender is expected to know to remove before sending.

### Intersperse HTML comments

In HTML, a comment is included using the following syntax:

{% highlight html %}
<!-- this is a comment -->
{% endhighlight %}

HTML comments are not parsed by browsers, meaning an email with an embedded comment should display as normal, but may fool a bot attempting to harvest email addresses — it is likely looking at the source code of the page, not the final rendered version. This email address looks like:

{% highlight html %}
<!-- blah -->user<!-- blah -->@<!-- blah -->example<!-- blah -->.<!-- blah -->com
{% endhighlight %}

### Percent encoding

Using a method similar to PHP's [`urlencode`](http://php.net/manual/en/function.urlencode.php) function, we can encode each character of the email address as a percentage sign followed by two hexadecimal characters, which is automatically parsed by the browser when the page is rendered. Again, this is obfuscated in the source code but not in the final rendered output. For example, `user@example.com` will be encoded as `%75%73%65%72%40%65%78%61%6d%70%6c%65%2e%63%6f%6d`.

### ROT13 on alphabetic characters

The [ROT13](http://en.wikipedia.org/wiki/ROT13) function encodes alphabetic characters by shifting them 13 places. (Forwards or backwards makes no difference as there are 26 characters in total.) So, `user@example.com` becomes `hfre@rknzcyr.pbz`. If this is encoded by server-side software, it can be decoded using JavaScript by performing the function again.

### JavaScript string concatenation

We can "build" the email address using JavaScript using something like this:

{% highlight javascript %}
var string1, string2, string3;
string1 = "user";
string2 = "example";
string3 = "com";
document.write(string1 + "@" + string2 + "." + string3);
{% endhighlight %}

### Insert element with `display:none` CSS property

A span element is added to the email string, similar to the HTML comment method above, but we also add a rule for this element to have the `display:none` CSS property. This allows the user to copy the email address without obfuscation, but obfuscates the address at both a source code and non-stylesheet level.

### Encoding the email address as a picture

The last method is also a common one, and it is to encode the email address as a picture so that the email address does not exist at all in text form on the page. So, user@example.com would be encoded as follows:

![Picture email address](assets/images/picture-email-address.png)

## Results

![Obfuscation results](assets/images/obfuscation-results.png)

The ROT13 and picture methods of obfuscation were the only two methods that yielded a perfect score of zero spam messages received, which I believe means that the addresses themselves were not harvested. The CSS `display:none` method was close behind, with only 36 messages received after a 2 year period. (What may be interesting to note is that all 36 messages came in the last 4 months of the experiment — meaning that spammers likely updated their harvesting software to decode this method.)

## Conclusion

If you have to make your email address publicly available, from this experiment it looks like the ROT13 and picture methods of obfuscation are currently the most effective. However, they each have their own disadvantages:

* ROT13 requires the use of server-side software like PHP to encode the address (or have the address hard-coded in ROT13 form already), and also client-side JavaScript to re-encode / decode it on the other side. If the user's browser does not support JavaScript (or they are using a tool like [NoScript](http://noscript.net) to selectively enable JavaScript), the address will continue to appear in obfuscated form.
* By placing your email address in a picture, the user can neither copy and paste the email address into their mail client nor click a mailto link for the address, discouraging them from sending an email in the first place. An image also takes up significantly more bandwidth than text, but as the email image above is less than 4KB in size this is not a major concern.

As mentioned at the beginning of the post, the prevalence of social media websites has mitigated the severity of this problem. Sites like Facebook and Twitter take on the responsibility of protecting your mailbox from spam, and you don't have to deal with obfuscating a link to your profile. Alternatively, you can handle the email delivery yourself using a web-based contact form.

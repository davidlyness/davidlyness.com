---
layout: post
title: Fixed points of hash functions
excerpt: Can a fixed point of a cryptographic hash function exist — where a string hashes to itself?
---

This post is inspired by someone who asked me question today along the lines of “can a string's [`MD5` hash](http://en.wikipedia.org/wiki/MD5) be its own value?”. The answer is that it's possible — nothing fundamental about the algorithm prevents it — but the more interesting challenge is to find the probability that such an event can occur.

The `MD5` hash algorithm takes a string of arbitrary length and returns a 128 bit result — usually displayed as a 32-digit hexadecimal number. For example:

{% highlight plaintext %}
md5("test123") = c38f4ec93b73956709932e6464f4f339
md5("test456") = 0f3c7ce16f5c313b81a9e54758bcae8a
{% endhighlight %}

In order for a string to hash to itself, it’s not difficult to see that the string itself must be exactly 128 bits, or 32 hexadecimal characters. Therefore, a naive (and somewhat pointless) approach would be to try all possible strings for a potential match. We've limited our search space to all strings of exactly 32 hexadecimal characters in length, which gives us a total of \\(16^{32}=2^{128}\\) strings to search. At a modest rate of calculating 1,000,000 hashes per second, it would take us \\(1.079 \times 10^{25}\\) years to try all possible combinations. A quicker way would be nice.

Let's start from a purely mathematical standpoint, and come back to specific functions like `MD5` later. At the most basic level, for a given function \\(f\\), we would like to check whether there exists an input \\(x\\) such that \\[f\left(x\right)=x.\\] We also know a few things about \\(f\\). It is a [cryptographic hash function](http://en.wikipedia.org/wiki/Cryptographic_hash_function), meaning that the output will always be a string of fixed length — let's say length \\(n\\) when represented in binary. This means that \\(x\\) — our candidate for a fixed point of \\(f\\) — must also be of length \\(n\\). This gives a total of \\(2^n\\) candidates for \\(x\\).

Another property of cryptographic hash functions is that they aim to distribute their results over its range as uniformly as possible to minimise hash collisions. Modelling \\(f\\) as a uniform distribution, \\[\mathbb{P}\left({f\left(x\right)=x}\right)=\frac{1}{2^n}\\] for a given \\(x\\). As an intermediate step, let's calculate the probability that, for all possible values of \\(x\\), \\(x\\) is not a fixed point of \\(f\\). This will be \\(1-\frac{1}{2^n}\\) multiplied by the number of possible values of \\(x\\) — so, \\[\mathbb{P}\left({\forall x:f\left(x\right)\not=x}\right)=\left({1-\frac{1}{2^n}}\right )^{2^n},\\]and since \\(\lim_{\alpha \to \infty} \left(1-\frac{1}{\alpha} \right )^\alpha = \frac{1}{e}\\), we get \\[\lim_{n \to \infty}\mathbb{P}\left({\forall x:f\left(x\right)\not=x}\right)=\frac{1}{e}.\\] Now, to get the probability that some \\(x\\) *does* has this property, we calculate the opposite: \\[\lim_{n \to \infty}\mathbb{P}\left({\exists x:f\left(x\right)=x}\right)=1-\frac{1}{e}.\\] Therefore, the probability that a hash function of sufficient length has a fixed point is approximated by \\(1-\frac{1}{e}\approx 0.6321=63.21\%\\).

Coming back to the `MD5` function, it outputs a string of binary length 128. So, \\(n=128\\) and as \\(2^{128}\\) is a very large number, the probability of a fixed point of the `MD5` algorithm is very well approximated as 63.21%. As our mathematical method is generic and only dependent on certain properties common to all cryptographic hash functions, it also works for other algorithms like [`SHA-1`](http://en.wikipedia.org/wiki/SHA-1) (\\(n=160\\)) and [`SHA-2`](http://en.wikipedia.org/wiki/SHA-2) (\\(n=224\\), \\(n=256\\), \\(n=384\\) or \\(n=512\\)).

While the above calculation predicts the existence of a fixed point, it doesn't give us any details about how to find it. The process of locating arbitrary hash collisions is relatively easier due to the [birthday paradox](http://en.wikipedia.org/wiki/Birthday_problem), but even these are infeasible to determine for a cryptographically secure hash function. As it stands, we're unlikely to find such a fixed point in these large-\\(n\\) hash functions unless calculation speeds hugely increase, a mathematical weakness is found in the algorithm itself, or someone gets very, very lucky.
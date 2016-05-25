---
layout: post
title: Determining PageRank
excerpt: A script to determine the Google PageRank of a given web domain — a major factor of a page's position in the list of search results.
---

Google's PageRank algorithm assigns a number between 1 and 10 to web domains (such as "facebook.com" or "davidlyness.com"), and is one of the main factors in determining how high to place a website in the list of Google search results. The method by which a domain's PageRank is calculated is [patented (and hence public)](http://en.wikipedia.org/wiki/PageRank#Algorithm), but a domain's PageRank is never actually shown in the search results themselves. It is also notoriously difficult to find a simple way to query Google's servers for this information.

When Google released the Google Toolbar back in 2000, it included a feature which displayed the PageRank of the website currently being viewed. With a bit of reverse-engineering, I developed the below function which will return the PageRank of a given domain. Interesting to note is that the returned rank is always between 1 and 9 — so sites like "google.com" that have a de facto PageRank of 10 appear as having a PageRank of 9.

{% highlight php %}
<?php
function getPageRank($domain) {
	$domainlen = strlen($domain);
	$seed = "Mining PageRank is AGAINST GOOGLE'S TERMS OF SERVICE. Yes, I'm talking to you, scammer.";
	$seedlen = strlen($seed);
	$result = 0x01020345;
	for ($i = 0; $i < $domainlen; $i++) {
		$pos = $i % $seedlen;
		$result ^= ord($seed[$pos]) ^ ord($domain[$i]);
		$result = (($result >> 23) & 0x1ff) | $result << 9;
	}
	$checksum = 8 . dechex($result);
	$url = sprintf("http://toolbarqueries.google.com/tbr?client=navclient-auto&ch=%s&features=Rank&q=info:%s", $checksum, $domain);
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	$response = curl_exec($ch);
	curl_close($ch);
	$rank = substr(strrchr($response, ":"), 1, 1);
	return $rank;
}
?>
{% endhighlight %}

Disclaimer:

* As can be seen from `$seed`, abuse of this script is in violation of Google's Terms of Service. Use at your own risk.
* Google can change the initialised value of `$result` at any time — doing so will render this script unusable until this value is updated.

---
layout: post
title: Search and replace using regular expression callbacks
excerpt: How can you have more programmatic control of replacements in regex expressions?
---

A problem I've been working with recently involves performing conditional search and replace operations using regular expression matching. For example, you might want to replace all occurrences of `[color='red']text[/color]` with red text, `[color='blue']text[/color]` with blue text, and so on. The naïve way to achieve this is with individual search / replace statements for each discrete case.

{% highlight php %}
preg_replace("/\[color='red'\](.*)\[\/color\]/", '<font color="#FF0000">${1}</font>', $content);
preg_replace("/\[color='blue'\](.*)\[\/color\]/", '<font color="#0000FF">${1}</font>', $content);
// and so on...
{% endhighlight %}

While using a separate find / replace operation for each case works fine, if the number of cases grows performance will start to take a hit, not to mention generating unmaintainable and ugly code.

Consider a real-world example. [Wikipedia](http://en.wikipedia.org/wiki/Main_Page) automatically converts Wiki markup links to other Wikipedia articles to HTML, and formats the link as blue if the article exists, and red if it does not. Wikipedia is performing conditional regular expression matching on the links, but obviously can't do this on the basis of the displayed text alone — some kind of lookup is required to determine whether the linked-to article is valid.

This is where the simple (but useful) function `preg_replace_callback` comes in. Instead of performing the replacement using static regular expression syntax, this function allows the code to "call back" to another function to determine the text to act as the replacement. Assuming the [standard Wiki markup format for hyperlinks](http://en.wikipedia.org/wiki/Help:Wiki_markup#Links_and_URLs) (including renamed links) is used, the example for Wikipedia above could be implemented as follows.

{% highlight php %}
preg_replace_callback("/\[\[(.*?)\|([^ ]*?)\]\]/", 'outputLink', $content);
function outputLink($matches) {
	if (articleExists($matches[2])) {
		$class = "linkexists";
	} else {
		$class = "linknotexists";
	}
	return "<a href='" . $matches[2] . "' class='" . $class . "'>" . $matches[1] . "</a>";
}
{% endhighlight %}

The `articleExists` function would be a simple database lookup, returning whether the article reference provided in the Wiki markup is valid for an existing article, and the class attribute of the link allows for standard formatting of the link based on its validity.

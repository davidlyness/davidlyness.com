---
layout: post
title: CSS — specificity and priority
excerpt: Determining the specificity and priority of CSS rules when applied to HTML.
---

CSS (or Cascading Style Sheets) is a language used to define the presentation of a page — most often used on web pages in conjunction with HTML. The language contains a few intricacies that may not be obvious to a web developer — all-too-often causing them to ask the question "why won't my page display properly?!".

A fragment of HTML may look like this.

{% highlight html %}
<span class="exampleclass">This is some example text.</span>
{% endhighlight %}

We can use CSS to apply formatting to the `exampleclass` span element with CSS rules. These can specify many things — margins, alignment, visibility, colour, borders, size and so on. The following rule specifies the colour and boldness of `exampleclass`.

{% highlight css %}
.exampleclass {
  color: #FF0000; /* This is the colour red represented as a hexadecimal string */
  font-weight: bold;
}
{% endhighlight %}

CSS can do a lot more than change the format of text. In fact, this entire website depends on CSS — it allows you to view the site as it is now, instead of like this.

![No CSS](assets/images/no-css.png)

CSS rules can have varying degrees of scope — some can apply to HTML element classes (as above), to element IDs:

{% highlight css %}
#exampleid {
  color: #FFFF00; /* This is the colour yellow represented as a hexadecimal string */
}
{% endhighlight %}

Or even to HTML elements as a whole (for example, to all images on the page).

{% highlight css %}
img {
  border: 5px solid #FF0000;
}
{% endhighlight %}

These rules have a many-to-many relationship with the elements themselves — a rule can apply to many HTML elements, and an HTML element can have many rules applied to it. It is the latter that can cause some confusion — where multiple rules apply to an element, which takes priority?

To determine this, we need to delve into CSS specificity. This is the concept of weighing CSS selectors according to how specific they are — the more "specifically" they apply to a certain element, the more weight they will carry when determining which rule to apply to the HTML.

The specificity of a certain element can be thought of as a 4-tuple (A,B,C,D) calculated as follows:

* A = 1 if the style declaration is from an inline `style` attribute (see below), 0 otherwise.
* B = the number of ID attributes (prefixed with a `#`) in the selector.
* C = the number of class attributes (prefixed with a `.`) and [pseudo-classes](http://www.w3schools.com/css/css_pseudo_classes.asp) (like visited links) in the selector.
* D = the number of element names and [pseudo-elements](http://www.w3schools.com/css/css_pseudo_elements.asp) (like the first letter of a paragraph) in the selector.

The official specification of the rules can be found on the [W3C website](http://www.w3.org/TR/CSS2/cascade.html#specificity).

The tuples (A1,B1,C1,D1) and (A2,B2,C2,D2) can then be compared using standard tuple comparison rules. (Compare A1 and A2, if they are equal compare B1 and B2, if they are equal compare C1 and C2, and if they are equal compare D1 and D2.) Given two rules that apply to selector X, the rule with higher specificity will apply. If two rules have equal specificity, the location of the CSS rule is used to determine a prioritised order as follows.

* Highest priority goes to rules specified inline in the HTML:
{% highlight html %}
<div style="color: #FF0000;">Example text.</div>
{% endhighlight %}
* The next highest priority goes to rules specified in an internal style sheet:
{% highlight html %}
<style>
	p {
		color: #FF0000;
	}
</style>
{% endhighlight %}
* The next highest priority goes to rules specified in an external style sheet:
{% highlight html %}
<link href="/include/styles.css" rel="stylesheet" type="text/css" />
{% endhighlight %}
* Lastly, the lowest priority rules are those specified in the browser's default style settings.

If two rules are specified in the same location with equal specificity, the latter rule (i.e. the rule recorded lower down in the CSS file) will apply. This is the main reason why the "C" in CSS stands for "Cascading"!

The one exception to the above specificity calculation is if a CSS rule is tagged with the importance flag.

{% highlight css %}
.exampleclass {
  color: #FF0000 !important;
  font-weight: bold;
}
{% endhighlight %}

In this case, the above rule for the `exampleclass` element will apply even if other CSS rules exist with higher specificity (for example, rules including an ID attribute or more than one class attribute). For anything to take precedence above this rule, it will also require an importance flag. However, the scope of the importance flag is the current property only — the font-weight property in the example above is unaffected by the importance flag, and will be used in accordance with the normal specificity rules.

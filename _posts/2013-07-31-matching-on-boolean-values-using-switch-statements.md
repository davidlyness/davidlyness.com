---
layout: post
title: Matching on boolean values using switch statements
excerpt: A different way to think about switch statements, using a boolean value to evaluate against each case's logical expression for comparison.
---

All [Turing-complete](http://en.wikipedia.org/wiki/Turing_completeness) programming languages provide some way to branch conditionally into different sections of code. This is true in the most basic programming languages (i.e. assembly language):

{% highlight plaintext %}
cmp eax, 0
jne notzero
iszero:
	mov ebx, 1
	jmp endif
notzero:
	mov ebx, 2
	jmp endif
endif:
	nop
{% endhighlight %}

In most higher-level languages (e.g. the JavaScript below), this is implemented in the form of an `if..then..else` statement:

{% highlight javascript %}
if (a == 0) {
	b = 1;
} else {
	b = 2;
}
{% endhighlight %}

In fact, even an [instruction set with a single instruction (OISC)](http://en.wikipedia.org/wiki/One_instruction_set_computer) will implement conditional branching in some way!

However, consider the following slightly messy code:

{% highlight javascript %}
if (a == "this") {
	doThis();
} else if (a == "that") {
	doThat();
} else if (a == "otherwise") {
	doOtherwise();
} else {
	doSomethingElse();
}
{% endhighlight %}

Although this makes programmatic sense, and after a few seconds we can tell what is going on, we allow ourselves a bit of syntactic sugar when we want to branch out on a few different paths — based on the result of one variable — using a `switch` statement. So, we could equivalently write the above as:

{% highlight javascript %}
switch (a) {
	case "this":
		doThis();
		break;
	case "that":
		doThat();
		break;
	case "otherwise":
		doOtherwise();
		break;
	default:
		doSomethingElse();
}
{% endhighlight %}

This is the normal use for a `switch` statement — it allows the program to branch into different sub-routines based on the system's evaluation of a variable. However, consider the following code:

{% highlight javascript %}
if (variableOne == "Something") {
	func1(variableOne);
} else if (variableTwo == "SomethingElse") {
	func2(variableTwo);
} else if (variableThree == "SomethingElseAgain") {
	func3(variableThree);
} else if (variableFour == "SomethingelseEntirely") {
	func4(variableFour);
}
{% endhighlight %}

This code could (will) start to get messy if we start introducing more branches or additional instructions in each branch. Thankfully, the `switch` statement can help us again. The subject of a `switch` statement — the `a` variable in the `switch` example above — does not need to be a single variable declaration. It can instead be any logical expression with a [truthy (or falsey) value](http://james.padolsey.com/javascript/truthy-falsey/, against which each of the cases can be evaluated for comparison. For example, the above snippet could be re-written as:

{% highlight javascript %}
switch (true) {
	case (variableOne == "Something"):
		func1(variableOne);
		break;
	case (variableTwo == "SomethingElse"):
		func2(variableTwo);
		break;
	case (variableThree == "SomethingElseAgain"):
		func3(variableThree);
		break;
	case (variableFour == "SomethingelseEntirely"):
		func4(variableFour);
		break;
}
{% endhighlight %}

This implementation of a switch statement — switching on a boolean value and matching one case (or multiple cases, if you remove the break statements) — is a little-known paradigm and one that I've found hugely useful. Now I just have to work out who thought it was a good idea that [Python not have switch statements at all](http://www.python.org/dev/peps/pep-3103/)!

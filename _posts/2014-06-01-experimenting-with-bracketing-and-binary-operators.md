---
layout: post
title: Experimenting with bracketing and binary operators
excerpt: Combining parentheses and binary operators to calculate arithmetic properties, and reasoning about their recurrence relations and generic closed forms.
redirect_from: /experimenting-bracketing-binary-operators
---

## Introduction

(Note — the code used to generate all data in this article can be found on [GitHub](https://gist.github.com/davidlyness/70703d4fca9d7e40a2403eaee9aa63cd).)

I've been experimenting with combinations of bracketing and binary operators recently, and came across some interesting patterns.

Let's say you have \\(n\\) instances of some digit \\(d\\) between 0 and 9. There are multiple ways of writing these digits with binary operators between them and some form of bracketing.

For example, let's say you have four 2's. (In this case, \\(n=4\\) and \\(d=2\\).) Then there are five different ways we could insert parentheses into the expression if the same generic binary operator \\(\cdot\\) is used in all three positions:

\\(\left ( \left (2 \cdot 2 \right ) \cdot \left (2 \cdot 2 \right )\right )\\)

\\(\left ( \left ( \left ( 2 \cdot 2 \right ) \cdot 2 \right ) \cdot 2 \right )\\)

\\(\left ( \left ( 2 \cdot \left (2 \cdot 2 \right ) \right ) \cdot 2 \right )\\)

\\(\left (2 \cdot \left ( \left ( 2 \cdot 2 \right ) \cdot 2 \right ) \right )\\)

\\(\left (2 \cdot \left (2 \cdot \left ( 2 \cdot 2 \right ) \right ) \right )\\)

And since there is no guarantee that \\(\cdot\\) is associative, all five of these expressions could evaluate to different values.

To procedurally generate all possible bracketings given \\(n\\) digits, we can define a function as follows:

{% highlight python %}
def all_bracketings(expr):
    """
    Generates all possible permutations of parentheses for an expression.
    @param str expr: the non-bracketed source expression
    @rtype: str
    """
    if len(expr) == 1:
        yield expr
    else:
        for i in range(1, len(expr), 2):
            for left_expr in all_bracketings(expr[:i]):
                for right_expr in all_bracketings(expr[i+1:]):
                    yield "({}{}{})".format(left_expr, expr[i], right_expr)
{% endhighlight %}

(I got to use the super-useful [Python `yield` keyword](https://docs.python.org/3/reference/expressions.html#yield-expressions) here, which will be the subject of a future post.)

What's more, there's no reason all the binary operators in the expressions above have to be the same — if we restrict ourselves to the standard arithmetic operators of addition, subtraction, multiplication and division, there are \\(4^{3} \times 5=320\\) unique expressions. Here are a few examples from the 320 possible combinations:

\\(\left ( \left (2 + 2 \right ) -\left (2/2 \right )\right )=3\\)

\\(\left ( \left (2 + 2 \right ) /\left (2-2 \right )\right )\\) is undefined (division by zero)

\\(\left ( \left ( 2 \times \left ( 2 \times 2 \right ) \right ) +2\right )=10\\)

Of these, I'm calling the first and third valid, and the second not valid (since the result of evaluating the expression is undefined). Given the restrictions on parentheses and binary operators we've already defined, the only way an expression cannot be valid is for division by zero to occur.

## Questions

At this point, I was interested in the following questions:

1. What's the maximum value an expression can evaluate to in terms of \\(n\\) and \\(d\\)?
2. What's the minimum value an expression can evaluate to in terms of \\(n\\) and \\(d\\)?
3. What's the smallest (absolute) value an expression can evaluate to in terms of \\(n\\) and \\(d\\)?
4. How many expressions exist for a given \\(n\\)?
5. How many valid expressions exist for a given \\(n\\)?

## Maximum value

This one is quite easy. Of addition, subtraction, multiplication and division, multiplication grows the fastest as \\(n\\) grows. So, a likely candidate for the maximum value is \\[\underbrace{d\times d \times \dots \times d}_{n\text{ times}}=d^{n}.\\] The data confirms this too — for example, when \\(d=7\\):

|---
| \\(d\\) | \\(n\\) | Maximum expression value | Maximum expression
|-|-|-
| \\(7\\) | \\(1\\) | \\(7\\) | \\(7\\)
| \\(7\\) | \\(2\\) | \\(49\\) | \\(\left ( 7 \times 7 \right )\\)
| \\(7\\) | \\(3\\) | \\(343\\) | \\(\left (7 \times \left(7 \times 7 \right ) \right )\\)
| \\(7\\) | \\(4\\) | \\(2401\\) | \\(\left(7 \times\left (7 \times \left(7 \times 7 \right ) \right )\right )\\)
| \\(7\\) | \\(5\\) | \\(16807\\) | \\(\left ( 7 \times\left(7 \times\left (7 \times \left(7 \times 7 \right ) \right )\right )\right )\\)
| \\(7\\) | \\(6\\) | \\(117649\\) | \\(\left ( 7 \times\left ( 7 \times\left(7 \times\left (7 \times \left(7 \times 7 \right ) \right )\right )\right )\right )\\)
| \\(7\\) | \\(7\\) | \\(823543\\) | \\(\left ( 7 \times\left ( 7 \times\left ( 7 \times\left(7 \times\left (7 \times \left(7 \times 7 \right ) \right )\right )\right )\right )\right )\\)
| \\(7\\) | \\(8\\) | \\(5764801\\) | \\(\left ( 7 \times\left ( 7 \times\left ( 7 \times\left ( 7 \times\left(7 \times\left (7 \times \left(7 \times 7 \right ) \right )\right )\right )\right )\right )\right )\\)

The only exception is when \\(d=1\\), because \\(1\times 1 < 1 + 1\\). In this case, the 1's are partitioned into sums of 3, then multiplied together. For example: \\[\left ( \left ( 1+\left ( 1+1 \right ) \right ) \times \left ( 1+\left ( 1+1 \right ) \right )\right )=9.\\]

The closed form for the maximum expression value when \\(d=1\\) is \\[\text{maximum}(n)=\begin{cases} 1 &\mbox{if } n = 1 \\\ \frac{3^{\left \lfloor \frac{n}{3} \right \rfloor}}{1-\frac{n\mod 3}{4}} & \mbox{if }n>1. \end{cases}\\]

Alternatively, a more readable recurrence relation is \\[\text{maximum}(n)=\begin{cases} n&\mbox{if } n < 4 \\\ 3\times\text{maximum}(n-3) &\mbox{if }n\geq 4. \end{cases}\\]

# Minimum value

The minimum value (or "most negative") is closely related to the maximum value. If the maximum value is \\(M\\), then the minimum value can be no smaller than \\(-M\\), otherwise we'd be able to have a maximum larger than \\(M\\) by symmetry. In fact, the minimum value is achieved when we calculate the \\(n-1\\)th maximum, and subtract this from the remaining \\(d\\): \\[d-\underbrace{d\times d \times \dots \times d}_{n-1\text{ times}}=d-d^{n-1}.\\] And when \\(d=7\\) as before:

|---
| \\(d\\) | \\(n\\) | Minimum expression value | Minimum expression
|-|-|-
| \\(7\\) | \\(1\\) | \\(7\\) | \\(7\\)
| \\(7\\) | \\(2\\) | \\(0\\) | \\(\left ( 7-7 \right )\\)
| \\(7\\) | \\(3\\) | \\(-42\\) | \\(\left (7 - \left(7 \times 7 \right ) \right )\\)
| \\(7\\) | \\(4\\) | \\(-336\\) | \\(\left(7 -\left (7 \times \left(7 \times 7 \right ) \right )\right )\\)
| \\(7\\) | \\(5\\) | \\(-2394\\) | \\(\left ( 7 -\left(7 \times\left (7 \times \left(7 \times 7 \right ) \right )\right )\right )\\)
| \\(7\\) | \\(6\\) | \\(-16800\\) | \\(\left ( 7 -\left ( 7 \times\left(7 \times\left (7 \times \left(7 \times 7 \right ) \right )\right )\right )\right )\\)
| \\(7\\) | \\(7\\) | \\(-117642\\) | \\(\left ( 7 -\left ( 7 \times\left ( 7 \times\left(7 \times\left (7 \times \left(7 \times 7 \right ) \right )\right )\right )\right )\right )\\)
| \\(7\\) | \\(8\\) | \\(-823536\\) | \\(\left ( 7 -\left ( 7 \times\left ( 7 \times\left ( 7 \times\left(7 \times\left (7 \times \left(7 \times 7 \right ) \right )\right )\right )\right )\right )\right )\\)

## Smallest value

The smallest value differs from the minimum value as it is the value closest to zero. It turns out that for \\(n>1\\) we can always find an expression equal to zero for a given \\(d\\): \\[\underbrace{d\times d \times \dots \times d}_{n-2\text{ times}}\times\left(d-d \right )=d^{n-2}\times 0=0.\\] So, maybe it's more interesting to look at the smallest non-zero value.

## Smallest non-zero value

The smallest non-zero value is similar to the maximum in the same way as the minimum. Whereas with the minimum we wanted the most negative expression — and  therefore subtracted \\(d^{n-1}\\) from \\(d\\) — for the smallest value, we want to divide \\(d\\) by \\(d^{n-1}\\): \\[\frac{d}{\underbrace{d\times d \times \dots \times d}_{n-1\text{ times}}}=d^{2-n}.\\] This matches up with our data:

|---
| \\(d\\) | \\(n\\) | Smallest non-zero expression value | Smallest non-zero expression
|-|-|-
| \\(2\\) | \\(1\\) | \\(2\\) | \\(2\\)
| \\(2\\) | \\(2\\) | \\(1\\) | \\(\left ( 2 \div 2 \right )\\)
| \\(2\\) | \\(3\\) | \\(0.5\\) | \\(\left( 2\div \left (2\times 2 \right )\right )\\)
| \\(2\\) | \\(4\\) | \\(0.25\\) | \\(\left( 2\div \left ( 2 \times\left (2\times 2 \right )\right )\right )\\)
| \\(2\\) | \\(5\\) | \\(0.125\\) | \\(\left( 2\div \left (2 \times \left ( 2 \times\left (2\times 2 \right )\right )\right )\right )\\)
| \\(2\\) | \\(6\\) | \\(0.0625\\) | \\(\left( 2\div \left (2 \times \left (2 \times \left ( 2 \times\left (2\times 2 \right )\right )\right )\right )\right )\\)
| \\(2\\) | \\(7\\) | \\(0.03125\\) | \\(\left( 2\div \left (2 \times \left (2 \times \left (2 \times \left ( 2 \times\left (2\times 2 \right )\right )\right )\right )\right )\right )\\)
| \\(2\\) | \\(8\\) | \\(0.015625\\) | \\(\left( 2\div \left (2 \times \left (2 \times \left (2 \times \left (2 \times \left ( 2 \times\left (2\times 2 \right )\right )\right )\right )\right )\right )\right )\\)

## Total expressions

Ignoring binary operations for a moment, the total number of ways to insert parentheses into an expression with \\(n\\) digits is as follows.

|---
| \\(n\\) | Number of expressions
|-|-|
| \\(1\\) | \\(1\\)
| \\(2\\) | \\(2\\)
| \\(3\\) | \\(5\\)
| \\(4\\) | \\(14\\)
| \\(5\\) | \\(42\\)
| \\(6\\) | \\(132\\)
| \\(7\\) | \\(429\\)
| \\(8\\) | \\(1430\\)

It turns out that these are the [Catalan numbers](http://en.wikipedia.org/wiki/Catalan_number), and this sequence arises in all sorts of combinatorial problems. Their closed form is \\(C(0)=1\\), and \\[C(n)=\frac{1}{n+1}\sum_{i=0}^{n}\binom{n}{i}^{2}.\\] Since the \\(n\\) in \\(C(n)\\) refers to the number of pairs of brackets rather than the number of digits in our problem, the number of expressions for \\(n\\) digits is equal to \\(C(n-1)\\).

However, this doesn't give us every single expression for a given \(n\) — we need to take the choices of binary operator into account too. We have \\(n-1\\) "slots" into which to place binary operators, and for each slot there are 4 binary operators to choose from. The total number of expressions for a given \\(n\\) is therefore \\[4^{n-1}\times C(n-1).\\]

## Total valid expressions

I mentioned above that not all expressions are valid — some include a division by zero operation which results in the expression's value being undefined.

The following table shows the total number of expressions overall for each \\(n\\), and the total number of valid expressions for each \\(d\\) — they differ in an interesting way!

|---
| \\(n\\) | Total expressions | \\(d=0\\) | \\(d=1\\) | \\(d=2\\) | \\(d=3\\) | \\(d=4\\) | \\(d=5\\) | \\(d=6\\) | \\(d=7\\) | \\(d=8\\) | \\(d=9\\) |
|-|-|-|-|-|-|-|-|-|-|-|-
| \\(1\\) | \\(1\\) | \\(1\\) | \\(1\\) | \\(1\\) | \\(1\\) | \\(1\\) | \\(1\\) | \\(1\\) | \\(1\\) | \\(1\\) | \\(1\\)
| \\(2\\) | \\(4\\) | <span class="post-highlight">\\(3\\)</span> | \\(4\\) | \\(4\\) | \\(4\\) | \\(4\\) | \\(4\\) | \\(4\\) | \\(4\\) | \\(4\\) | \\(4\\)
| \\(3\\) | \\(32\\) | <span class="post-highlight">\\(18\\)</span> | \\(31\\) | \\(31\\) | \\(31\\) | \\(31\\) | \\(31\\) | \\(31\\) | \\(31\\) | \\(31\\) | \\(31\\)
| \\(4\\) | \\(320\\) | <span class="post-highlight">\\(135\\)</span> | <span class="post-highlight">\\(301\\)</span> | \\(305\\) | \\(305\\) | \\(305\\) | \\(305\\) | \\(305\\) | \\(305\\) | \\(305\\) | \\(305\\)
| \\(5\\) | \\(3584\\) | <span class="post-highlight">\\(1134\\)</span> | <span class="post-highlight">\\(3275\\)</span> | <span class="post-highlight">\\(3337\\)</span>  | \\(3345\\) | \\(3345\\) | \\(3345\\) | \\(3345\\) | \\(3345\\) | \\(3345\\) | \\(3345\\)
| \\(6\\) | \\(43008\\) | <span class="post-highlight">\\(10206\\)</span> | <span class="post-highlight">\\(38211\\)</span> | <span class="post-highlight">\\(39367\\)</span>  | <span class="post-highlight">\\(39481\\)</span> | \\(39505\\) | \\(39505\\) | \\(39505\\) | \\(39505\\) | \\(39505\\) | \\(39505\\)
| \\(7\\) | \\(540672\\) | <span class="post-highlight">\\(96228\\)</span> | <span class="post-highlight">\\(467367\\)</span> | <span class="post-highlight">\\(485113\\)</span>  | <span class="post-highlight">\\(487523\\)</span> | <span class="post-highlight">\\(487855\\)</span> | \\(487935\\) | \\(487935\\) | \\(487935\\) | \\(487935\\) | \\(487935\\)
| \\(8\\) | \\(7028736\\) | <span class="post-highlight">\\(938223\\)</span> | <span class="post-highlight">\\(5914620\\)</span> | <span class="post-highlight">\\(6199332\\)</span>  | <span class="post-highlight">\\(6236058\\)</span> | <span class="post-highlight">\\(6243604\\)</span> | <span class="post-highlight">\\(6244838\\)</span> | \\(6245118\\) | \\(6245118\\) | \\(6245118\\) | \\(6245118\\)

As you read from left to right across a row of the table (i.e. as \\(d\\) increases for the same \\(n\\)), the number of valid expression stabilises. For example, for \\(n=5\\), the monotonically-increasing sequence is \\[1134,3275,3337,3345,3345,\dots,3345.\\]

Each value highlighted in yellow in the table above is one which is strictly less than its stabilised counterpart (i.e. \\(1134\\), \\(3275\\) and \\(3337\\) are highlighted in yellow above because they are all less than \\(3345\\)). This sequence is bounded above by the total number of expressions (calculated previously as \\(4^{n-1}\times C(n-1)\\)), but anything further than that is still to be determined. One good thing has come out of this though — a new sequence has been approved and added to the [On-Line Encyclopedia of Integer Sequences (OEIS)](https://oeis.org/) — check out [A243312](https://oeis.org/A243312)!

## Summary

|---
| Property | Expression
|-|-|
| maximum value | \\(\text{Maximum}(d,n)=\begin{cases}\begin{cases} n &\mbox{if } n < 4 \\\ 3\times\text{Maximum}(n-3) &\mbox{if }n\geq 4 \end{cases} & \mbox{for }d=1 \\\ d^n & \mbox{otherwise}\end{cases}\\)
| minimum value | \\(\text{Minimum}(d,n)=d-\text{Maximum}(d,n-1)\\)
| smallest value | \\(\text{Smallest}(d,n)=\begin{cases} d & \mbox{for }n=1 \\\ 0 & \mbox{otherwise}\end{cases}\\)
| smallest non-zero value | \\(\text{SmallestNZ}(d,n)=d\div\text{Maximum}(d,n-1)\\) for \\(d\neq 0\\)
| total number of expressions | \\(\text{TotalExpressions}(n)=4^{n-1}\times C(n-1),\text{ where }C(n)=\begin{cases} 1 & \mbox{for }n=0 \\\ \frac{1}{n+1}\sum_{i=0}^{n}\binom{n}{i}^{2} & \mbox{otherwise}\end{cases}\\)
| total number of valid expressions | A formula for \\(\text{TotalValidExpressions}(n)\\) is currently unknown, but the sequence is bounded above by \\(\text{TotalExpressions}(n)\\)

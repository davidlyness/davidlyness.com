---
layout: post
title: Set Theory — Part 1
excerpt: The first post in a series of 3 on Set Theory — detailing set fundamentals and exploring paradoxes that can arise from naive set theory.
---

One of the fundamental concepts of mathematics is that everything you're working with must be "well-defined". That means that a precise mathematical definition must be given for anything you want to work with, be it functions, properties, operations or sets. And these definitions themselves must be based on even more fundamental concepts of mathematics, and so on. Eventually you get down to one of two things:

* Axioms, or
* intuitive definitions.

Axioms are mathematical statements which are taken as "obvious" (I'll leave what "obvious" means in this context to the philosophers!) For example, an axiom saying that we can add two numbers together either way around and get the same result is called commutativity, and written \\(a + b = b + a\\). From this, and other [axioms like it](http://en.wikipedia.org/wiki/Field_(mathematics)#Definition_and_illustration), we are able to build up all of the fundamentals of modern-day mathematics, which comprises the 1st year course in [real analysis](http://en.wikipedia.org/wiki/Real_analysis).

However, even these axioms have a more fundamental basis. If you visit the axiom link above, you'll notice that we define mathematical structures in terms of sets. Naïve set theory tells us that a set is just a collection of objects — for example, an apple, a pear and an orange. We denote a set by putting curly braces around the objects in the set (called the elements of the set), and denote it by \\[\\{\text{apple, pear, orange}\\}.\\]
Note that a set is uniquely determined by its elements — that is, if two sets contain precisely the same elements as each other, we say they are equal. We can also write \\(x\in X\\) to mean that “\\(x \text{ is an element of the set } X\\)”. Two more simple rules about sets:

* The way a set is ordered is irrelevant: \\(\\{\text{apple, pear}\\} = \\{\text{pear, apple}\\}\\).
* Duplicates in a set are irrelevant: \\(\\{\text{apple, apple}\\} = \\{\text{apple}\\}\\).

Being mathematicians, we usually prefer to work with numbers rather than fruit. So, the set of non-negative whole numbers less than 10 would be \\(\\{0,1,2,3,4,5,6,7,8,9\\}\\), the set of square numbers less than 100 would be \\(\\{0,1,4,9,16,25,36,49,64,81\\}\\), and so on. In particular, we call the set containing no elements the empty set, and write it as \\(\emptyset\\). But what if we wanted a list of all the non-negative whole numbers? We'd write it as \\[\\{0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,…\\},\\]
and we soon see that it would be quite impossible to write them all down! Such a set is said to be infinite — it has infinitely many elements. Since we can't denote such a set by writing down all the elements, we denote it by using a symbol — in the case of the example above, \\(\mathbb{N}\\). We have more symbols for other infinite sets:

* \\(\mathbb{Z}\\) for the set of all whole numbers (both positive and negative);
* \\(\mathbb{Q}\\) for the set of all fractions;
* \\(\mathbb{R}\\) for the set of all real numbers, which include numbers like \\(\pi\\).

If you look closely you can see that every element that is in \\(\mathbb{Z}\\) is also in \\(\mathbb{Q}\\) — in this case, we say that “\\(\mathbb{Z}\\) is a subset of \\(\mathbb{Q}\\)”, and denote it by \\(\mathbb{Z}\subseteq \mathbb{Q}\\). In actual fact, each of the sets I mentioned above is a subset of its successor, and we can say \\[\emptyset\subseteq \mathbb{N}\subseteq \mathbb{Z}\subseteq \mathbb{Q}\subseteq \mathbb{R}.\\]
Finally, a common practice is to define a set by specifying that all of the elements in that set must have a specific property. For example, above we defined the set of non-negative whole numbers that were less than 10. We would write this formally as \\[\\{ x \in \mathbb{N}:x < 10\\}.\\]

This is how mathematicians proceeded for many years, and everyone was satisfied with this intuitive definition of sets. However, in 1901 a mathematician named [Bertrand Russell](http://en.wikipedia.org/wiki/Bertrand_Russell) published a paper in which he outlined a paradox derived from the very foundations of set theory. He proposed the following: take a set \\(R\\), which contains the sets which do not contain themselves: \\[R=\\{x:x \notin x\\}.\\] So far so good, as long as you can wrap your head around the notion of what a set containing itself would "look like". But there's nothing in our rules which says this cannot be defined. He then asked the question: does \\(R\\) belong to itself: is \\(R \in R\\)? There are two possibilities, either \\(R \in R\\) or \\(R \notin R\\). We consider each case in turn.

* \\(R \in R\\): then by definition \\(R\\) does not contain itself, and so \\(R \notin R\\), which is a contradiction.
* \\(R \notin R\\): then \\(R\\) does not satisfy the property of elements of \\(R\\): namely, it is false that \\(R\\) does not contain itself. Therefore \\(R \in R\\), which is again a contradiction.

In the second post on this topic I'll talk about how this issue was solved, and some interesting concepts that arose in set theory thereafter, which I've been studying this term.
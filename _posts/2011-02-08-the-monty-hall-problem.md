---
layout: post
title: The Monty Hall Problem
excerpt: The somwhat counter-intuitive Monty Hall Problem, and a quick mathematical explanation and generalisation.
---

You find yourself on a game show, with the opportunity to win big.

> In front of you are three doors, behind one of which contains a car, and behind the other two doors are goats. After selecting a door, the host (Monty Hall), knowing what is behind each of the doors, opens one of the unselected doors to reveal a goat. He now gives you the choice: do you want to stick with your original selection, or switch to the remaining unselected door?
> 
> ”Well," you think, "I have a 50/50 chance either way. So I might as well stick with my original selection." Monty Hall then opens the two remaining doors, revealing that you have chosen a goat, while the car lies behind the door you rejected in favour of your original choice. "A shame," says Monty, "you stood twice the chance of winning if you had switched."

What I've just explained is a popular counter-intuitive problem in probability theory, most often called the [Monty Hall problem](http://en.wikipedia.org/wiki/Monty_Hall_problem).

On first glance, it seems to be a simple problem. Your chance of choosing the correct door initially was \\(1 \over 3\\), and now that you only have two doors to choose from it seems reasonable that the chance of winning has risen to \\(1 \over 2\\). However, in reality your chance of winning never changed: you always had a chance of \\(1 \over 3\\) with your selected door, which leaves a \\(2 \over 3\\) chance that the car is behind the remaining unopened door.

A way to gain a more intuitive understanding of the puzzle is to consider another hypothetical game show scenario, except this time there are 1,000,000 doors instead of the usual three. After a door is selected the host opens 999,998 other doors, all revealing goats. You are then left with the same choice: to stick with your original door, or to switch to the one unopened door. Are you still convinced that there is a 50/50 chance of you being right straight off? Well, before any of the other doors are opened, you have a \\(1 \over 1000000\\) chance of winning. That means that you have a \\(999999 \over 1000000\\) chance of losing — that is, the chance that the car is behind a door that you did not choose is \\(999999 \over 1000000\\). Therefore, when all of the other doors have been opened leaving you with a choice between two, it is still the case that there is a \\(999999 \over 1000000\\) chance that the car is behind one of the other doors — but there's only one left! In this case, the chance of winning by switching doors will be 99.9999%. If you can grasp that, then the problem "scales down" to the three-door scenario where you have a \\(1 \over 3\\) chance of winning by sticking with your current choice or a \\(2 \over 3\\) chance by switching.

There are a few assumptions that need to be made in order to ensure that what has been described above really is the case. In addition to the intuitive assumption that the car is equally likely to reside behind any one door, we must assume that the host knows which door the car is behind — otherwise he runs the risk of revealing the car when he opens an unselected door… which would ruin it somewhat for the contestant! A variant on the original problem assumes that the host never knows which door the car is behind, but somehow always manages to reveal a goat when opening the unselected door. In that case intuition prevails and the car is won exactly \\(1 \over 2\\) the time — switching makes no difference.

A generalisation to \\(N\\) doors, with the host opening \\(p < N-1\\) goat doors before offering the player their choice. The probability of winning by switching is then \\[N-1 \over N(N-p-1)\\] and by the algebra of limits \\[{N-1 \over N(N-p-1)} \rightarrow {1 - {1 \over N}}, \quad \text{as } p \rightarrow {N-2}.\\] In addition, letting \\(N\\) tend to infinity, the probability of winning by switching tends to 1 — that is, the more doors there are to open in the first place, the more likely you are to win by switching doors.

Interestingly, when originally published in Parade (an American magazine), along with the correct answer that switching provides the optimal chance of success, the editors received over 10,000 letters insisting that the published solution was incorrect; including nearly 1,000 people holding PhDs! (I suppose this means that even mathematicians get it wrong sometimes…)

If you're interested in these type of problems, then you may also want to check out the [Three Prisoners](http://en.wikipedia.org/wiki/Three_Prisoners_problem) problem, a puzzle mathematically equivalent to the Monty Hall problem.

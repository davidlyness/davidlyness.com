---
layout: post
title: Strategy — hangman
excerpt: The best strategy to use when choosing words in Hangman — depending on the number of guesses allowed and the length of the word.
---

The basic idea of the game Hangman is simple: think of a word that your opponent cannot correctly guess the letters of within the allowed letter limit. Recently I was looking at a Wolfram post about the [best hangman words](http://blog.wolfram.com/2010/08/13/25-best-hangman-words), and thought it would be worth talking about here. I'm going to be discussing the research contained in the linked article, and how we can apply this to our own games.

In my personal history of playing the game, I often found that choosing short words with few common letters was a good strategy: by minimising or avoiding the use of vowels, words like `fly` or `jinx`, while they’re “easy” words, they could fall through the gaps when your opponent is concentrating on the vowels and more common letters. However, this same strategy cannot necessarily be used against a computer: they could have a dictionary of hundreds of thousands of words, with each letter narrowing the possible candidates, and choosing the letter that is most likely to lead to a correct guess. Therefore, while `_ i _ _` may not come easily to a human, a computer can soon determine that an `n` somewhere in the word is likely, and thereafter `j` and `x` will likely follow.

Since we all love to beat computers "at their own game" so to speak, it is interesting to think about optimal word choice to defeat a particular artificial opponent. I say particular because there is no guarantee that any two algorithms will choose their next letter in the same (or similar) way — the algorithm linked to above would be completely deterministic if they did not attempt to move toward a [Nash equilibrium point](http://en.wikipedia.org/wiki/Nash_equilibrium#Informal_definition) by choosing letters probabilistically based on their ratio of success — for example, the article mentions a ratio of `x`:`e` of 13:1000, approximately 1:77.

It seems that my intuitive strategy for using short words was correct: a 3-letter word has a maximum of 23 incorrect guesses, so without any kind of insightful frequency analysis, there's a much better chance with these words already — longer words allow algorithms (and humans) to get a foothold on the word, permitting easier further progress.

The best words also depend on the 'length' of the game you're playing. For example, an 8-game would permit 8 incorrect guesses before the player loses, whereas a 13-game allows 5 more guesses, requiring 13 (half the alphabet) before the game ends. The researcher decided to use a 'brute force' method to attempt to discern the best words — that is, throw every word you can think of at it, numerous times, and see how often it gets the right answer. While time-consuming and boring for a human, thanks to the wonders of modern technology we can do this all very quickly. The results below come from the 90,000 word dictionary built into Mathematica.

Results time. The top 10 words are ranked in the table below, with the best words to use at the top.

|---
| Word rank | 8-game | 9-game | 10-game | 11-game | 12-game | 13-game |
|-|-|-|-|-|-|-|
| #1 | `jazz` | `jazz` | `jazz` | `jazz` | `jazz` | `jazz` |
| #2 | `buzz` | `buzz` | `buzz` | `jazzed` | `jazzed` | `jazzed`  |
| #3 | `jazzed` | `jazzed` | `jazzed` | `buzz` | `hajj` | `faff` |
| #4 | `hajj` | `hajj` | `hajj` | `hajj` | `jazzy` | `faffed` |
| #5 | `jazzy` | `jazzy` | `jazzy` | `jazzy` | `buzz` | `jazzy` |
| #6 | `buzzed` | `jazzing` | `jazzing` | `jazzing` | `jazzing` | `hajj` |
| #7 | `jazzing` | `buzzed` | `buzzed` | `faffed` | `faffed` | `jazzing` |
| #8 | `fizz` | `fizz` | `fuzz` | `buzzed` | `faff` | `buzz` |
| #9 | `fuzz` | `buzzing` | `jinx` | `jinx` | `faffs` | `faffs` |
| #10 | `buzzing` | `jinx` | `fizz` | `faff` | `jinx` | `vex` |

From the above table, we can see that the word `jazz` ranks top in all game types tested. So, if you want to beat a computer using this (or a similar) algorithm, `jazz` is a good bet. There are simply too many other words of the easily guessed form `_ a _ _` (and with more common letters than `j` and `z`).

This is all well and good, but what about if you wanted to have the best 7-letter word? Or 10-letter word? Fear not, for the following table shows the top 10 words of each length, from 4 letters to 10 letters, played in a 10-game.

|---
| Word rank | 4 letters | 5 letters | 6 letters | 7 letters | 8 letters | 9 letters | 10 letters
|-|-|-|-|-|-|-|-|
| #1 | `jazz` | `jazzy` | `jazzed` | `jazzing` | `jazziest` | `muzziness` | `zigzagging` |
| #2 | `buzz` | `fuzzy` | `buzzed` | `buzzing` | `quizzing` | `bowwowing` | `wigwagging` |
| #3 | `hajj` | `faffs` | `jazzes` | `jazzier` | `fuzziest` | `huzzahing` | `grogginess` |
| #4 | `fuzz` | `fizzy` | `faffed` | `faffing` | `fizzling` | `powwowing` | `beekeeping` |
| #5 | `jinx` | `jiffs` | `fizzed` | `fuzzing` | `puppying` | `bubbliest` | `mummifying` |
| #6 | `fizz` | `swizz` | `buzzer` | `buzzers` | `whizzing` | `mugginess` | `fluffiness` |
| #7 | `puff` | `dizzy` | `bovver` | `buffing` | `jammiest` | `fuzziness` | `fulfilling` |
| #8 | `jiff` | `joked` | `fuzzed` | `fizzing` | `babbling` | `kokkiness` | `shabbiness` |
| #9 | `razz` | `jives` | `joking` | `jinxing` | `bubbling` | `fluffiest` | `revivified` |
| #10 | `buff` | `buffs` | `buffed` | `puffing` | `quizzers` | `peppiness` | `hobnobbing` |

So, in a game where the length of the word is your prerogative, go for `jazz`. If you're forced to choose a 10-letter word, try `zigzagging`. A few caveats though:

* This technique has proven to be effective against a computer algorithm, not human intuition.
* Others may also know the "least often guessed words", and may know the strategy ahead of time.
* Make sure you have a dictionary to hand: `jazzing` doesn't sound like a real word, and you'll probably get called out on it.
* People may get annoyed at you if they are consistently not able to guess the answer correctly!

Coming soon: [Connect 4](http://www.boardgamegeek.com/boardgame/2719/connect-four) — is it winnable, or does it always result in a tie if both players are skilled enough (à la Tic-Tac-Toe)? Does being the first player and placing the first piece matter? Wait and see…

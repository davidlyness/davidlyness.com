---
layout: post
title: Proper password policies
excerpt: A discussion of various password policies that are often enforced, their relative effectiveness and — in some cases — their harmfulness.
---

When choosing a password for a new service, there are often restrictions on the password you can choose or the policies enforced. For example, requiring the password to be a certain length, using certain character sets or requiring a new password be chosen after a fixed time period. While some of these restrictions are effective in preventing the password from being compromised through a brute force (exhaustive search) attack, others are arbitrary and some actually lessen the security of the password — the very thing they're designed to prevent. Looking at each policy I've come across in turn, I'll label them as effective, ineffective or harmful.

Throughout this post, I use the term "key space" to refer to the set of all possible passwords given that certain policies are in place. For example, the key space of all one-character lowercase passwords is \\(\\{a, b, c, ..., z\\}\\), and has size 26; and the key space of all two-character lowercase passwords is \\(\\{aa, ab, ac, ..., ba, bb, bc, ..., zz\\}\\) and has size \\(26^2=676\\).

## Password construction policies

### Minimum length
**Effective**

If a minimum length of 6 characters were enforced, "pass" is invalid and "password" is valid. This is one of the simplest policies to implement and understand, and it is also one of the most effective. By mandating that all passwords are of a certain length, it makes it much more difficult for an attacker to try all passwords in the key space in an attempt to find one that works. If we're using just the lowercase English alphabet with 26 characters, the number of passwords of length 5 is \\(11881376\\), whereas the number of passwords of length 6 is \\(308915776\\) — increasing the key space by a factor of 26. A side effect is that the attacker would know not to try any passwords of length 5 or less, but this is negligible next to the increased password length.

### Maximum length
**Harmful**

If a maximum length of 6 characters were enforced, "password" is invalid and "pass" is valid. In complete opposition to the previous policy, this limits the key space and makes passwords much easier to crack. It also calls into question the method by which the service is storing the passwords in the database — more on that later.

### Multiple character sets
**Effective**

This policy enforces the use of multiple character sets — usually lowercase, uppercase, numeric and special symbols. For example, "abcd" is invalid and "Ab1!" is valid. Similar to having a minimum length, this vastly increases the size of the key space and makes a brute force attack more difficult. As an example, let's consider a password of 4 characters. If we are using only lowercase letters, the size of the key space is \\(26^4=456976\\). If we now include lowercase letters (26), uppercase letters (26), numbers (10) and the "?" and "!" special symbols (2), this gives us a total of 64 characters, making our key space \\(64^4=16777216\\) in size — over 35 times larger.

### Password cannot be an English word
**Effective**

A website could maintain a wordlist (for example, a list of all the words in the English dictionary), and disallow any passwords that match a word in the list (or contain a word in the list). For example, "cat" is invalid, whereas "abc" is valid. At first glance this may appear to be ineffective as it limits the key space, giving an attacker less passwords to search. However, a brute force attack is often not the first line of attack — instead, the attacker will try all words in the dictionary, with the thinking that most users will choose an English word as their password. By removing this possibility it forces the attacker to perform a brute force attack instead, which is much more cumbersome.

### Password cannot contain an English word
**Ineffective**

This is similar to the previous policy, but here the English word could be a substring of the password, rather than the whole password. "cat" is still invalid, but so is "qqqcatqqq". While the previous policy is designed to force attackers to use a brute force attack rather than a dictionary attack, an attacker would already be using a brute force attack before he came across a password such as "qqqcatqqq". (Technically, the attacker could use a heuristic brute force algorithm to determine the probability of a substring in the password being more likely as it is a dictionary word and therefore tried sooner — but this is overkill for minimal gain, and may even slow down the candidate throughput if CPU time is being consumed by the heuristic engine.)

### Password cannot contain your other information
**Effective**

When signing up to a new web service, you're often asked to provide additional information like your full name, your email address and a username. Many users may choose to use something like their username as their password, making their password very easy to remember, but also easily guessable. Unfortunately this was a common practice before this password policy was implemented on a widespread basis.

### Password cannot contain repeating characters
**Ineffective**

As an example, "yyy" is invalid and "xyz" is valid. While this may be a sensible policy when using a keypad (it is often easy to see which keys are pressed most often), when implemented as a password policy all it serves to do is restrict the key space.

## Password administration policies

### Password lockout after a number of failed attempts
**Effective**

This essentially thwarts any brute force attack when the attacker does not have access to the verification details stored in the database (i.e. it forces an offline attack). For example, by requiring administrator intervention if an incorrect password is entered 5 times, it strikes a balance between convenience for the user (who is not likely to incorrectly enter their password 5 times) and security against a brute force attack. An attacker, who would normally try hundreds or thousands of passwords per second using an automated program, would instead find that they are locked out after only a few attempts.

### New password must be different from previous password(s)
**Effective**

If an attacker manages to gain knowledge of our password, we would want to be able to change our password so that he no longer has access. If the current password is "pass" and a policy is in place to prevent identical passwords in the most recent three passwords, the user would need to change their password 3 times before changing it back to "pass" (for example, "pass" to "pass1" to "pass2" to "pass3" to "pass"). This helps to mitigate the possibility that the attacker will re-gain access after a user changes their password. (This policy is often combined with password expiration, below.)

### New password must not be similar to previous password(s)
**Harmful**

This seems like a very similar policy to the previous one, but with a crucial difference. To understand it, we need to delve into how these passwords are being stored. As [discussed in a previous post](/salt-pepper-and-rainbows-storing-passwords-properly), the proper practice is for passwords to be hashed (and salted) when stored in a database, rather than being stored in plaintext. This prevents an attacker who gains control of the password database from instantly knowing all users' passwords. A good cryptographic hash function's outputs should reveal nothing about the plaintext passwords — for example, the sha1 hash of "abcd" is `81fe8bfe87576c3ecb22426f8e57847382917acf`, and the sha1 hash of "abce" is `0a431a7631cabf6b11b984a943127b5e0aa9d687` — that is, the hashes are completely different regardless of the similarity of the plaintexts. A system that can tell whether the new password is similar to the old one(s) is storing the previous passwords in plaintext (or using a form of reversible encryption, which is almost as bad).

### Minimum length of time between password changes
**Harmful**

A 24-hour policy of this nature simply means "If a password is changed at 9am on Monday, it cannot be changed again until 9am on Tuesday at the earliest". I cannot see any advantages to this policy, and would welcome feedback from anyone who believes it has its uses. On the other hand, this gives a would-be attacker a 24-hour window to have guaranteed access to a user's system if they can find out their new password.

### Maximum length of time between password changes (password expiration)
**Debatable**

I believe this is the most debatable policy on this list. While it guarantees that attackers who have discovered a password have only a finite usage time, it often forces the user to choose similar passwords every time the current password expires ("pass" to "pass1" to "pass2" to "pass3", etc). This practice is undetectable to the system administrator if they are following correct password storage procedure, but is intuitive for an attacker to guess even if a lockout policy is in place — an expired password of "pass3" would likely prompt an attacker to guess "pass4". An interesting (mathsy) [paper](http://www.cs.unc.edu/~yinqian/papers/PasswordExpire.pdf) was published in 2010 that weighs the benefits of password expiration.

## Conclusion

The following password policies are **effective**:

* Minimum length
* Multiple character sets
* No English words
* No personal information
* Lockout policy
* New password different from previous passwords

The following password policies are **ineffective**:

* No English words contained in password
* No repeating characters

The following password policies are **harmful**:

* Maximum length
* New password not similar to previous passwords
* Minimum length of time between password changes

The benefits of the following password policies are **debatable**:

* Maximum length of time between password changes (password expiration)

While weighing up the above methods of keeping passwords safe from attackers, we often forget that users are only human and will exert the minimum amount of effort required to comply with policy. If we enforce all of the above recommended policies, an average user may take to writing their password down and storing it under their keyboard or on their monitor. While this may be against corporate policy, it is less enforceable than technical password restrictions and is much more prone to discovery by nearby colleagues — whose intentions may not be benign.

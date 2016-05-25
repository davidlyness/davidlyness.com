---
layout: post
title: Input sanitization (the easy way)
excerpt: How and why we want to sanitise user input, with examples using the PDO structure of PHP.
---

In the 1.0.12 update to this site, the single entry in the changelog was "nicer input sanitization". What does this mean, and why is it now "nicer"?

A website such as this accepts information from site visitors. For example:

* [GET and POST data](http://www.tizag.com/phpT/postget.php);
* [Cookies](http://en.wikipedia.org/wiki/HTTP_cookie), [user agent strings](http://en.wikipedia.org/wiki/User_agent), etc;
* Information sent via the contact form and comments;
* Login data.

Adhering to the basic security model, all such data should be regarded as untrusted — that is, there is no guarantee that the data is not designed to maliciously affect the system. For example, a comment could be submitted which includes a SQL command that drops a table whenever the comment is inserted into the database. This sort of thing needs to be stamped out before it gets near the database interpreter.

Before 1.0.12, I was using a strategy of escaping characters. This means that, for example, whenever a `”` is seen, the database replaces it with `\”` — this ensures that an attacker cannot interject their own SQL commands into their comment. PHP provides a useful function called `addSlashes` to do this automatically for escapable characters; when the comment is to be outputted, a similar function called `stripSlashes` can be used to remove all the backslashes, changing the `\”` back to a `”` so that the comment is displayed correctly.

This is a somewhat messy way to do things — it relies on the developer remembering to use the `addSlashes` and `stripSlashes` functions on all database interactions. In addition, PHP includes a "feature" called [magic quotes](http://php.net/manual/en/security.magicquotes.php) (thankfully deprecated, unfortunately still enabled by default) which automatically escapes data inserted into the database. This may sound like a good thing, but not when you don't know it's going on, and it does it in all circumstances regardless of context!

The new method being used in 1.0.12 is prepared SQL statements. This involves sending the structure of the SQL command to the database first, and afterwards sending in the parameters (user provided data). Because the command is submitted in two separate steps like this, the database knows that anything received in the second step will be just data, nothing else. This prevents SQL injection attacks in a much cleaner way.

The PHP [`PDO`](http://www.php.net/manual/en/intro.pdo.php) architecture provides a clean method for preparing such statements — by calling [`prepare`](http://php.net/manual/en/pdo.prepare.php) first we can outline the structure of the command, and with [`bindParam`](http://php.net/manual/en/pdostatement.bindparam.php) we can insert the untrusted data.

For example, consider the following (insecure) PHP code snippet:

{% highlight php %}
$db = new PDO(/* database connection information */);
$sql = “INSERT INTO tblcomments (postid, comment) VALUES ($postid, $comment);";
$stmt = $db->prepare($sql);
$stmt->execute();
{% endhighlight %}

If we assume the comment stored in the variable $comment has not been escaped or otherwise sanitized, it is possible a SQL injection could occur by way of someone submitting a malicious comment.

Now consider the following improved version:

{% highlight php %}
function test() {
	$db = new PDO(/* database connection information */);
	$stmt = $db->prepare(“INSERT INTO tblcomments (postid, comment) VALUES (:postid, :comment));
	$stmt->bindParam(':postid', $postid);
	$stmt->bindParam(':comment', $comment);
	$stmt->execute();
}
{% endhighlight %}

Because we have two user-provided parameters (the post that the comment is attached to, as well as the comment itself) we require two `bindParam` statements. Notice that on line 2, when the SQL command structure is being sent to the database, there is no user-provided data involved — the same command will be sent regardless of the post or comment. This also gives performance enhancements when using `SELECT` statements, as we're able to cache commonly requested queries.

There is still an outstanding problem in the current codebase. There is a function which has the field selection, table selection, field selection and field data all defined in terms of variables — so the barebones SQL command has the following structure:

{% highlight plaintext %}
SELECT $request FROM $table WHERE $givenfield=$givenvalue LIMIT 1
{% endhighlight %}

This poses a problem as the `bindParam` method can only be used on field data — i.e. the `$givenvalue` variable above. Therefore the above SQL command cannot (as far as I know) be effectively paramaterised using prepared statements. This isn't a huge problem as by design this function is internal — that is, it is always called with hard-coded data rather than user-supplied input. (The point of the above command is to give me a function I can call to execute repetitive-but-slightly-different commands like selecting a field from a table based on another field's value.)

SQL injection is one of the two main web attacks. The other, cross site scripting (XSS), is more complicated — expect to see a post on it in the future.

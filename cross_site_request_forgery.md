##Cross Site Request Forgery (CSRF)
***

[Source](https://owasp.org/www-community/attacks/csrf#:~:text=Cross%2DSite%20Request%20Forgery%20CSRF,which%20they're%20currently%20authenticated.)


Cross-Site Request Forgery(**CSRF**) is an attack that forces an end user to execute unwanted actions on a web application in which they're currently authenticated. With a little help of social engineering (such as sending a link via email or chat), an attacker may trick the users of a web application into executing actions of the attacker’s choosing. If the victim is a normal user, a successful **CSRF** attack can force the user to perform state changing requests like transferring funds, changing their email address, and so forth. If the victim is an administrative account, **CSRF** can compromise the entire web application.

[**Main Example OWASP Code Review Guide**](https://owasp.org/www-project-code-review-guide/migrated_content)

[**Reviewing Code for Cross-Site Request Forgery Issues**](https://owasp.org/www-project-code-review-guide/reviewing-code-for-csrf-issues)

[**CSRF Prevention Cheat Sheet**](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)

Most frameworks have built-in **CSRF** support such as Joomla, Spring, Struts, Ruby on Rails, .NET and others.


#####Description
**CSRF** is an attack that tricks the victim into submitting a malicious request. It inherits the identity and privileges of the victim to perform an undesired function on the victim’s behalf. For most sites, browser requests automatically include any credentials associated with the site, such as the user’s session cookie, IP address, Windows domain credentials, and so forth. Therefore, if the user is currently authenticated to the site, the site will have no way to distinguish between the forged request sent by the victim and a legitimate request sent by the victim.

**CSRF** attacks target functionality that causes a state change on the server, such as changing the victim’s email address or password, or purchasing something. Forcing the victim to retrieve data doesn’t benefit an attacker because the attacker doesn’t receive the response, the victim does. As such, **CSRF** attacks target state-changing requests.

It’s sometimes possible to store the **CSRF** attack on the vulnerable site itself. Such vulnerabilities are called “stored **CSRF** flaws”. This can be accomplished by simply storing an IMG or IFRAME tag in a field that accepts HTML, or by a more complex cross-site scripting attack. If the attack can store a **CSRF** attack in the site, the severity of the attack is amplified. In particular, the likelihood is increased because the victim is more likely to view the page containing the attack than some random page on the Internet. The likelihood is also increased because the victim is sure to be authenticated to the site already.

#####Synonyms
**CSRF** attacks are also known by a number of other names, including XSRF, “Sea Surf”, Session Riding, Cross-Site Reference Forgery, and Hostile Linking. Microsoft refers to this type of attack as a One-Click attack in their threat modeling process and many places in their online documentation.

**Prevention measures that do NOT work**

#####Using a secret cookie
Remember that all cookies, even the secret ones, will be submitted with every request. All authentication tokens will be submitted regardless of whether or not the end-user was tricked into submitting the request. Furthermore, session identifiers are simply used by the application container to associate the request with a specific session object. The session identifier does not verify that the end-user intended to submit the request.

#####Only accepting POST requests
Applications can be developed to only accept POST requests for the execution of business logic. The misconception is that since the attacker cannot construct a malicious link, a **CSRF** attack cannot be executed. Unfortunately, this logic is incorrect. There are numerous methods in which an attacker can trick a victim into submitting a forged POST request, such as a simple form hosted in an attacker’s Website with hidden values. This form can be triggered automatically by JavaScript or can be triggered by the victim who thinks the form will do something else.

A number of flawed ideas for defending against **CSRF** attacks have been developed over time. Here are a few that we recommend you avoid.

#####Multi-Step Transactions
Multi-Step transactions are not an adequate prevention of **CSRF**. As long as an attacker can predict or deduce each step of the completed transaction, then **CSRF** is possible.

#####URL Rewriting
This might be seen as a useful **CSRF** prevention technique as the attacker cannot guess the victim’s session ID. However, the user’s session ID is exposed in the URL. We don’t recommend fixing one security flaw by introducing another.

#####HTTPS
HTTPS by itself does nothing to defend against **CSRF**.

However, HTTPS should be considered a prerequisite for any preventative measures to be trustworthy.

#####Related Attacks
Cross-site Scripting (**XSS**)
Cross Site History Manipulation (**XSHM**)
## Logon (Web)
1. Description
The factory is hiding things from all of its users. Can you login as Joe and find what they've been looking at? https://jupiter.challenges.picoctf.org/problem/13594/

2. Solution
a. When you goto the above link, you are taken to a login page. In the description it is mentioned that you have to login as Joe. 
b. The first thing we can try is SQL injection. Try "Joe';-- " as the username and any random string as password and boom we're in.
c. However we still don't get the flag. Let's try looking at the cookies. There is a cookie with the name "admin" and the value set to False. Let's set it to true and we get the flag.

Flag: picoCTF{th3\_c0nsp1r4cy\_l1v3s\_d1c24fef}

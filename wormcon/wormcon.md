## 1. Secret provider (Web)
### 1. Challenge description
I will provide you a secret for next level security.

http://34.102.84.223/

Note: Flag is located in etc directory

Author : x3rz

### 2. Solution
1. When you goto the challenge site, you are greeted with a page having a single textbox. Any string you enter will be base64 decoded and printed back on the page.
 
 ![image](https://user-images.githubusercontent.com/78410304/131538470-abd32561-48d7-448d-ac4b-b43b773444ab.png)

2. Since the string is passed to the backend through GET requests, I initially thought that this must be a directory traversal attack. But it wasn't the case.
3. Since the website uses the python flask framework, we can try the server side template injection attack. First we have to check if the site actually uses a templating injection. For this we can try payloads from the [swisskey github repo](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#tools). 
4. First convert the payload "{{4\*4}}" into base64 and enter it into the site. We get the result as 16, that means the site probably uses the jinja2 templating engine.
5. Now to get the flag, we need to read the file contents. For this I encoded the payload "{{ self.\_TemplateReference__context.cycler.__init__.__globals__.os.popen('strings /etc/flag.txt').read() }}" and passed it into the textbox. This prints the flag on the webpage.
6. Flag: wormcom{55T1_15r311y50m3th1g_1y2v4ugu}

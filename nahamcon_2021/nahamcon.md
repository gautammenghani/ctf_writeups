Nahamcon 2021 had some interesting challenges. The writeups for the ones I solved are as follows: 

## 1. HOMEWARD BOUND (Web)
This challenge shows how the http headers can be manipulated. 
1. When you go to the home page of the site, you get a message "This page is not accessible externally".
2. So to bypass this, we need to think of ways to trick the page into thinking that we are internal users.
3. The solution is to use the 'X-Forwarded-For' header in the http request.
```bash
    $ curl -H "X-Forwarded-For: 127.0.0.1" -X GET "http://challenge.nahamcon.com:31365/"
```
flag: flag{26080a2216e95746ec3e932002b9baa4}

## 2. POLLEX (Warmups)
This challenge is a steganography challenge. It contains an image hidden inside the given image. The solution is as follows:  
1. Download the given image.
2. Use the binwalk tool to list the hidden files.
![3_1_pollex](https://user-images.githubusercontent.com/78410304/119316053-1e08b180-bc94-11eb-84e8-66cf2ae58711.png)
3. Now we can use the dd command in linux to extract the image. View it to get the flag.
```bash
    $ dd if=pollex.jpg of=pollex3.jpg skip=334 bs=1
```
flag: flag{65c34alec121a286600ddd48fe36bc88}

## 3. CHICKEN WINGS (Warmups)
This challenge is about the wingding fonts. Here's how I solved it:
1. I copy pasted the file contents in google search. There I got the hint that the symbols are some kind of a font.
2. Use any online translator to translate the font. I used https://lingojam.com/WingDing
![3_2_chickenwings](https://user-images.githubusercontent.com/78410304/119316080-26f98300-bc94-11eb-9e35-e56ef4919092.png)
flag: flag{e0791ce68f718188c0378b1c0a3bdc9e}

## 4. CAR KEYS (Warmups)
This challenge is about monoalphabetic substitution cipher. The solution is as follows:
1. Goto cryptii.com and set the cipher as 'Alphabetical substitution'.
2. Input the given plain text, set the ciphertext alphabet as 'QWERTY', and you'll get the flag.
3. Incase the working of the cipher is not clear, I'll break it down here:  
    a. Write down the entire alphabet in 1st row.  
    b. In the second row, start by writing the given ciphertext alphabet (QWERTY in this case).  
    c. Fill up the rest of the spots in alphabetical order, without repeating the letters.  
![3_3_carkeys](https://user-images.githubusercontent.com/78410304/119316111-2f51be00-bc94-11eb-997c-e5a8f5c2906d.png)

flag: flag{6f980c0101c8aa361977cac06508a3de}  

## 5. BUZZ (Warmups)
This challenge demonstrates the use of uncompress command.
1. In this challenge, we are given a file called buzz.
2. Running the file command on it shows "compress'd data 16 bits".
3. Use the uncompress command to get the flag.
```bash
    $ cat buzz | uncompress
```
flag: flag{b3a33db7ba04c4c9052ea06d9ff17869}


## 6. RESOURCEFUL (Apk reversing)
This challenge demonstrates the problem of hardcoded credentials. The solution is as follows:
1. Use jadx-gui to reverse the apk file. 
2. In the main activity, you can see that the password is hardcoded in the if condition.
![3_4_resourceful_apk](https://user-images.githubusercontent.com/78410304/119316154-3c6ead00-bc94-11eb-968e-f3d56825cc80.png)
3. Enter the password in login screen of the app and get the flag.  
flag: flag{7eecc051f5cb3a40cd6bda40de6eeb32}

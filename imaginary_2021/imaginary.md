This post has writeups for the imaginary ctf 2021. 

## 1. WEB - SaaS (Sed as a service)
1. In this challenge, we are given a webpage that accepts parameters for the linux sed command and executes the command on the server side. If you look at the source code, the command is as follows: 
```python 
sed <User input from webpage> stuff.txt
```
2. From the above command, we can assume that the flag is in a file called flag.txt. 
3. If you try executing the sed command, it performs the desired operations on the file and prints the file contents on the console.
4. The payload I used : "s/i/I/ \*". So the command executed will be:
```python
sed s/i/I/ * stuff.txt
```
5. This command would replace i with I in all the files and print the contents of all the files on the console.
6. Flag: ictf{:roocu:roocu:roocu:roocu:roocu:roocursion:rsion:rsion:rsion:rsion:rsion:\_473fc2d1}

## 2. PWN - stackoverflow
1. This is a classic buffer overflow challenge. We have a character array of size 40 and a gets() function to accept input from a user. The gets() function does not check the input size and goes on reading until it encounters new line("\n"). 
![buffer](https://user-images.githubusercontent.com/78410304/127282490-ac705903-1847-496a-957e-4a729020e1fc.jpg)

2. As shown in the above decompiled code (I've used Ghidra for this), there is long variable whose contents are compared to an ascii string and if correct, we get a shell prompt that can help us read the flag. 
3. Now looking at the way stack stores variables, the buffer will occupy 40 bytes and the target_var variable will be stored immediately after the buffer. So if we enter more than 40 characters in the buffer, we can easily override the target_var with any value we want. 
4. My payload: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAftci". If you look at the if statement in the code, the comparison is performed with "ictf" but we have entered "ftci". This is because the x86 architecture follows the little endian format for storing data. Little endian basically means the data on the right hand hide is stored in the smaller location. So the input "ftci" will be stored as "ictf" and the if statement will evaluate to true.
5. We can easily run "cat flag.txt" when we get the shell prompt.
6. Flag: ictf{4nd_th4t_1s_why_y0u_ch3ck_1nput_l3ngth5_486b39aa}

## 3. MISC - stonks
1. Given script
```python
#!/usr/bin/env python3

art = '''
    <REDACTED>
'''

flag = open("flag.txt").read()

class stonkgenerator: # I heard object oriented programming is popular
    def __init__(self):
        pass
    def __str__(self):
        return "stonks"

def main():
    print(art)
    print("Welcome to Stonks as a Service!")
    print("Enter any input, and we'll say it back to you with any '{a}' replaced with 'stonks'! Try it out!")
    while True:
        inp = input("> ")
        print(inp.format(a=stonkgenerator()))

if __name__ == "__main__":
    main()
```
2. This program contains a format string vulnerability. In the main function, in the format() parameters, a is the object of the class stonkgenerator. Now given that this is user controlled, we can use the following payload to get the flag: "{a.__init__.__globals__[flag]}"

 

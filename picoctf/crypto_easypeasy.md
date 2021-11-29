# EasyPeasy (crypto) : 40 points
## Description
A one-time pad is unbreakable, but can you manage to recover the flag? (Wrap with picoCTF{}) nc mercury.picoctf.net 36449

## Solution
1. We are given a script otp.py, that contains the code for generating a one time pad of given data. The script gets the data from the 'flag' file and XORs it with the key. 
```python
#!/usr/bin/python3 -u
import os.path

KEY_FILE = "key"
KEY_LEN = 50000
FLAG_FILE = "flag"


def startup(key_location):
	flag = open(FLAG_FILE).read()
	kf = open(KEY_FILE, "rb").read()

	start = key_location
	stop = key_location + len(flag)

	key = kf[start:stop]
	key_location = stop

	result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), flag, key))
	print(result,"This is the encrypted flag!\n{}\n".format("".join(result)))

	return key_location

def encrypt(key_location):
	ui = input("What data would you like to encrypt? ").rstrip()
	if len(ui) == 0 or len(ui) > KEY_LEN:
		return -1

	start = key_location
	stop = key_location + len(ui)

	kf = open(KEY_FILE, "rb").read()

	if stop >= KEY_LEN:
		stop = stop % KEY_LEN
		key = kf[start:] + kf[:stop]
	else:
		key = kf[start:stop]
	key_location = stop

	result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), ui, key))

	print("Here ya go!\n{}\n".format("".join(result)))

	return key_location


print("******************Welcome to our OTP implementation!******************")
c = startup(0)
while c >= 0:
	c = encrypt(c)
```
2. The key limit is 50000 characters. When this limit is exhausted, the XOR process starts from the beginning. This is the flaw. We can generate custom payload such that we directly get the key, and we can decrypt the flag. 
3. Send in 50032 0 bytes, so that the final 32 zero bytes are XORed with the key and we get the key directly.
4. Now use the key to decrypt the flag
```python
conn = remote('mercury.picoctf.net',36449)
conn.recvuntil(b'This is the encrypted flag!\n')
enc_flag=conn.recvline().decode('utf-8')
print(enc_flag)

#payload
i=50000-32
while i>=1000:
    print(f'Sending {i}')
    conn.recvuntil(b'What data would you like to encrypt?')
    conn.send(b'a'*1000+b'\r\n')
    i-=1000
print(f'Sending final payload {i}')  
conn.recvuntil(b'What data would you like to encrypt?')
conn.send(b'a'*i+b'\r\n')
conn.recvuntil(b'What data would you like to encrypt?')
conn.send(b'\x00'*32+b'\r\n')
conn.recvuntil(b'Here ya go!\n')
key=conn.recvline()
print(key)
conn.close()
key=key.strip()
i=0
pflag=''
while i+2<=64:
    t=int(enc_flag[i:i+2],16)
    p=int(key[i:i+2],16)
    pflag += (chr(t^p))
    i+=2
print(f'picoCTF{{{pflag}}}')
```
Flag: picoCTF{75302b38697a8717f0faee9c0fd36a57}

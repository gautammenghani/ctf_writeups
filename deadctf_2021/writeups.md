# Packet capture challenges
## Monstrum ex Machina
1. In this challenge we are given a pcap file and are asked to locate a search query where the attacker searched for the victim's name.
2. Since packet captures are long, it is generally a good idea to use the wireshark filters to narrow down your search space. In this example, we know that the search engines make a HTTP GET request. So we have to just look at the responses received for the HTTP GET requests.
3. When going through the pcap file, the victim's name is present in packet number 7019, as shown in the screenshot below.
4. Flag : flag{charles geschickter}

# Programming challenges
## The count
1. In this challenge we are given the IP address and port of a remote server. When we connect to the port using netcat, the server prints a word and we have to calculate the sum of its digits (where a=0, b=1, etc.) and send it to the server within 5 seconds.
2. This challenge can be solved using pwntools. My script is as follows:
```python
from pwn import *
conn = remote('code.deadface.io',50000)

conn.recvuntil(b'Your word is: ')
word = conn.recvline()
word = word.decode('utf-8').strip()
cnt=0
for letter in word:
    cnt+=(ord(letter)%97)
conn.sendline(bytes(str(cnt), 'utf-8'))
data=conn.recv()
print(data)
```
3. flag: flag{d1c037808d23acd0dc0e3b897f344571ddce4b294e742b434888b3d9f69d9944}

## Trick or treat
1. In this challenge, we are given a game where the player is supposed to dodge the multiple enemies headed its way. We have been given the source code of the game.
2. When the player crashes into the enemy, a prompt saying "death" appears and the game exits. So the solution to this challenge is to stop the prompt appearing so that even if player crashes into the enemy, the exit condition is never triggered.
3. In line 152, we can comment the do\_coll() function that is responsible for the collisions. Now run the game and the flag is printed on the terminal
```python
def get_intersect(x1, x2, w1, w2, y1, y2, h1):
    if y1 < (y2 + h1):
        if x1 > x2 and x1 < (x2 + w2) or (x1 + w1) > x2 and (x1 + w1) < (x2 + w2):
            #do_coll()
            pass
```
4. Flag : flag{CaNT\_ch34t\_d34th}

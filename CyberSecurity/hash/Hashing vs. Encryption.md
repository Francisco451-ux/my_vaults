 ## Symmetric Encryption

Symmetric algorithms use a key or secret to encrypt the data and use the same key to decrypt it. A basic example of symmetric encryption is XOR.

```
echo -n "HackTheBox123!" | md5sum

```

```

$python3

Python 3.9.10 (main, Feb 22 2022, 13:54:07) 
[GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from pwn import xor
>>> xor ("opens3same","academy")
/home/avataris12/.local/lib/python3.9/site-packages/pwnlib/util/fiddling.py:325: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  strs = [packing.flat(s, word_size = 8, sign = False, endianness = 'little') for s in args]
b'\x0e\x13\x04\n\x16^\n\x00\x0e\x04'
>>> 

```

## Asymmetric Encryption
- On the other hand, asymmetric algorithms divide the key into two parts (i.e., public and private). The public key can be given to anyone who wishes to encrypt some information and pass it securely to the owner. The owner then uses their private key to decrypt the content. Some examples of asymmetric algorithms are [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)), [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm), and [Diffie-Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange).


## # Identifying Hashes
```
pip install hashid
```

```
hashid '$apr1$71850310$gh9m4xcAn3MGxogwX/ztb.'

```

---

SHA1 hash `2d5c517a4f7a14dcb38329d228a7d18a3b78ce83`

```
john hash.txt --format=raw-sha1 --wordlist=/usr/share/seclists/Passwords/Common-Credentials/10k-most-common.txt --rules=THM01

```

To locate the JtR install directory run locate `john.conf`, then create `john-local.conf` in the same directory (in my case`/usr/share/john/john-local.conf`) and create our rules in here.
Let's edit `/usr/share/john/john-local.conf` and add a new rule:  

```
[List.Rules:THM01]
$[0-9]$[0-9]

```

--- 
## Haiti
Just replace `x.x.x` with the gem version you see after `gem build`.

```
$ git clone https://github.com/noraj/haiti.git haiti
$ cd haiti
$ gem install bundler
$ bundler install
$ gem build haiti.gemspec
$ gem install haiti-x.x.x.gem
```

Note: if an automatic install is needed you can get the version with `$ gem build haiti.gemspec | grep Version | cut -d' ' -f4`.

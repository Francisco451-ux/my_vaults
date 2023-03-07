w#### IDS/IPS Rule Triggering


Each IDS/IPS has a certain syntax to write its rules. For example, Snort uses the following format for its rules: `Rule Header (Rule Options)`, where **Rule Header** constitutes:

1.  Action: Examples of action include `alert`, `log`, `pass`, `drop`, and `reject`.
2.  Protocol: `TCP`, `UDP`, `ICMP`, or `IP`.
3.  Source IP/Source Port: `!10.10.0.0/16 any` refers to everything not in the class B subnet `10.10.0.0/16`.
4.  Direction of Flow: `->` indicates left (source) to right (destination), while `<>` indicates bi-directional traffic.
5.  Destination IP/Destination Port: `10.10.0.0/16 any` to refer to class B subnet `10.10.0.0/16`.

Below is an example rule to `drop` all ICMP traffic passing through Snort IPS:

`drop icmp any any -> any any (msg: "ICMP Ping Scan"; dsize:0; sid:1000020; rev: 1;)`

The rule above instructs the Snort IPS to drop any packet of type ICMP from any source IP address (on any port) to any destination IP address (on any port). The message to be added to the logs is “ICMP Ping Scan.”

### Encrypt the Communication Channel

Because an IDS/IPS won’t inspect encrypted data, an attacker can take advantage of encryption to evade detection. Unlike encoding, encryption requires an encryption key.

One direct approach is to create the necessary encryption key on the attacker’s system and set `socat` to use the encryption key to enforce encryption as it listens for incoming connections. An encrypted reverse shell can be carried out in three steps:

1.  Create the key
2.  Listen on the attacker’s machine
3.  Connect to the attacker’s machine

**Firstly**, On the AttackBox or any Linux system, we can create the key using `openssl`.

```
openssl req -x509 -newkey rsa:4096 -days 365 -subj '/CN=www.redteam.thm/O=Red Team THM/C=UK' -nodes -keyout thm-reverse.key -out thm-reverse.crt

```
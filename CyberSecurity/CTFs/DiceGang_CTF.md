0000000000000000000000000000000000000000 2ce03bc4ae69cd194b7680b18172641f7d56fbbf William Wang <defund@users.noreply.github.com> 1658084429 -0400     commit (initial): add foo

e03bc4ae69cd194b7680b18172641f7d56fbbf
7c87df63fa33dd32a65da19177dfd11ae41d39
1d542395c9663b57f8576d2aa38cbb242bce44
e9d6731db1921561a96a4c8e234672cf027ad9
27ddf1a74659171e4755a6d407c79ac381c3a5

2c  43  8d  b5  d9

main

2ce03bc4ae69cd194b7680b18172641f7d56fbbf


hope{': fail()
if flag[-1] != '}': fail()
if flag[5::3] != 'i0_tnl3a0': fail()
if flag[4::4] != '{0p0lsl': fail()
if flag[3::5] != 'e0y_3l': fail()
if flag[6::3] != '_vph_is_t': fail()
if flag[7::3] != 'ley0sc_l}

hope{l3a0i0_lsl{0p0_3le0y_3s_t_vplley}

l3a0i0_lsl{0p0_3le0y_3s_t_vplley

hope{l0l__t_l}

For instance, let’s get the first 4 values of a list. To do this, you can leave out the 0 as the start, and only specify 4 as the stop:
```

nums = [1, 2, 3, 4, 5, 6, 7]
part = nums[:4]
print(part)
```
```
Output:

[1, 2, 3, 4]
Also, if you want to slice a sequence from some point all the way up to the end of the sequence, you can omit the stop value.
```
```

For instance, let’s get all the values from the 3rd value from a list of numbers. To do this, you only need to specify the start index 2 and leave out the stop:

nums = [1, 2, 3, 4, 5, 6, 7]
part = nums[2:]
print(part)
```
```
Output:

[3, 4, 5, 6, 7]


arr[start:stop]         # items start through stop-1
arr[start:]             # items start through the rest of the array
arr[:stop]              # items from the beginning through stop-1
arr[:]                  # a copy of the whole array
arr[start:stop:step]    # start through not past stop, by step
```

def fail():
    print('Wrong!')
    exit(-1)
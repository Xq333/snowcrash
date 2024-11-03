Feeling a bit lost, we start by searching everywhere, looking for something useful.

After searching for a while, we see that the user flag00 has only created two files.

```
level00@SnowCrash:~$ find / -type f -user flag00 2>/dev/null
/usr/sbin/john
/rofs/usr/sbin/john
```

We begin investigating these files and find a strange series of letters.

```
level00@SnowCrash:~$ cat /usr/sbin/john
cdiiddwpgswtgt
```

After some research, it appears this could be decrypted using a ROT cipher.

https://www.dcode.fr/rot-cipher

We can use the rot-cipher from dcode to figure out what is the most probable rot algorithm that we need to use.

A ROT-15 decryption gives us the string "nottoohardhere."

So we try it:

```
level00@SnowCrash:~$ su flag00
Password:nottoohardhere
Don't forget to launch getflag !
```

Success!

```
flag00@SnowCrash:~$ getflag
Check flag.Here is your token :
```

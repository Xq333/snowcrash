After inspecting `/etc/passwd`, we identify that the `flag01` user has an encrypted password.

```
level01@SnowCrash:~$ cat /etc/passwd | grep flag01
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
```

We save the encrypted password to a file to crack it with `john`.

```
echo '42hDRfypTqqnw' > hashes.txt
```

Using `john` with the DES encryption format, we attempt to decrypt the password.

```
john --format=descrypt hashes.txt
Using default input encoding: UTF-8
Loaded 1 password hash (descrypt, traditional crypt(3) [DES 128/128 SSE2])
No password hashes left to crack (see FAQ)
```

Next, we check if `john` has successfully cracked the password.

```
john --show hashes.txt

Result: 'abcdefg'
```

With the password 'abcdefg', we try logging in as `flag01`.

```
level01@SnowCrash:~$ su flag01
Password:abcdefg
Don't forget to launch getflag !
Success!

flag01@SnowCrash:~$ getflag
Check flag.Here is your token :
```

Success!

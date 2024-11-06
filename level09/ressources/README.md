We start by exploring the files in the directory and find two items: an executable named `level09` and a file named `token`.

```
level09@SnowCrash:~$ ls
level09  token
```

Running `./level09` without arguments gives an error message.

```
level09@SnowCrash:~$ ./level09
You need to provied only one arg.
```

Providing an argument, we notice the program modifies the input by shifting characters.

```
level09@SnowCrash:~$ ./level09 test
tfuw
```

The `token` file contains a complex string with unreadable characters:

```
level09@SnowCrash:~$ cat token
f4kmm6p|=pnDBDu{
```

Since the characters are not readable, we inspect `token` with `hexdump` to reveal the hexadecimal values.

```
level09@SnowCrash:~$ hexdump token
0000000 3466 6d6b 366d 7c70 823d 707f 6e82 8283
0000010 4244 4483 7b75 8c7f 0a89
000001a
```

We write a C program to reverse the encoding pattern by decrementing each character by its position index. Below is the code:

```c
#include <stdio.h>

int main()
{
    char t[] = {0x66, 0x34, 0x6b, 0x6d, 0x6d, 0x36, 0x70, 0x7c,
                0x3d, 0x82, 0x7f, 0x70, 0x82, 0x6e, 0x83, 0x82,
                0x44, 0x42, 0x83, 0x44, 0x75, 0x7b, 0x7f, 0x8c,
                0x89, 0x0a};

    int i = 0;
    while (t[i])
    {
        t[i] -= i;
        i++;
    }
    printf("%s", t);
    return 0;
}
```

Compiling and running this code decodes the string, yielding the password for `flag09`:

'f3iji1ju5yuevaus41q1afiuq'

Using this password to switch to the `flag09` user:

```
level09@SnowCrash:~$ su flag09
Password: f3iji1ju5yuevaus41q1afiuq
Don't forget to launch getflag !
```

After switching users, we run `getflag` to retrieve the flag:

```
flag09@SnowCrash:~$ getflag
Check flag.Here is your token :
```

This completes the exploit for Level09.

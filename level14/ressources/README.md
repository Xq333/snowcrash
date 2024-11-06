Level14 was a bit tricky.
No file in the user home, we started searching for a hidden file but nothing, a look at processus and service running see if there is something unusual but still nothing.
After searching for quite a lot of time, we got an idea.

What about the getflag itself.
You know the way know --> ghidra

And we can see that it's very similar to level13 : Get the UID -> Depending of the UID take a string and pass it through ft_des. With a slight difference, that there is a PTRACE call at the beginning that stop the program if it was running from a debugger like gdb.
But it is implemented in 2 part, a call and then a if comparison, so it's exploitable too.

We did exactly the same as the previous one :
```
level14@SnowCrash:~$ gdb getflag
GNU gdb (Ubuntu/Linaro 7.4-2012.04-0ubuntu2.1) 7.4-2012.04
Reading symbols from /bin/getflag...(no debugging symbols found)...done.
(gdb) b 0x804898e
Breakpoint 1 at 0x804898e
(gdb) b0x8048b0a
Breakpoint 2 at 0x8048b0a
(gdb) run
Starting program: /bin/getflag 

Breakpoint 1, 0x0804898e in main ()
(gdb) set $eax=1
(gdb) continue
Continuing.

Breakpoint 2, 0x08048b0a in main ()
(gdb) set $eax=3014
(gdb) continue
Continuing.
Check flag.Here is your token : xxxxxxxxxxxxxxxx
```
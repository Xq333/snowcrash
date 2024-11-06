Level14 was a bit tricky.
No file in the user home, we started searching for a hidden file but nothing, a look at processus and service running see if there is something unusual but still nothing.
After searching for quite a lot of time, we got an idea.

What about the getflag itself.
You know the way know --> ghidra

And we can see that it's very similar to level13 : Get the UID -> Depending of the UID take a string and pass it through ft_des. With a slight difference, that there is a PTRACE call at the beginning that stop the program if it was running from a debugger like gdb.
But it is implemented in 2 part, a call and then a if comparison, so it's exploitable too.

We did exactly the same as the previous one :
```
gdb getflag
b *0x804898e (ptrace bypass)
b *0x8048b0a (uid bypass)
run
set $eax=1 (Since the condition is eax < 0)
continue
set $eax=3014 (UID of flag14 user)
continue
```
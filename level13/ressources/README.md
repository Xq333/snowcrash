For this level, we have a file "level13".
When we execute it it says that it have to be executed by UID 4242, a quick look at /etc/passwd and we see that there is no user with UID 4242 so we'll have to exploit the file.

We pass it through ghidra and we can see that it get the uid and then check if he is equal to 4242.
After that it take a string defined in the function and pass it through a function ft_des.
So I think there is 2 way to solve it, but we chose this approach :
- Get the address of the comparison
- Start the executable with gdb
- Break on the comparison
- Set the value of the variable that contains the uid to 4242
- Go back to the normal execution flow

In ghidra we can get the address of the comparison pretty easily : 0x0804859a
```
level13@SnowCrash:~$ gdb ./level13 
GNU gdb (Ubuntu/Linaro 7.4-2012.04-0ubuntu2.1) 7.4-2012.04
Reading symbols from /home/user/level13/level13...(no debugging symbols found)...done.
(gdb) b *0x0804859a
Breakpoint 1 at 0x804859a
(gdb) run
Starting program: /home/user/level13/level13 

Breakpoint 1, 0x0804859a in main ()
(gdb) info registers
eax            0x7dd    2013
ecx            0xbffff7d4    -1073743916
edx            0xbffff764    -1073744028
ebx            0xb7fd0ff4    -1208152076
esp            0xbffff720    0xbffff720
ebp            0xbffff738    0xbffff738
esi            0x0    0
edi            0x0    0
eip            0x804859a    0x804859a <main+14>
eflags         0x200246    [ PF ZF IF ID ]
cs             0x73    115
ss             0x7b    123
ds             0x7b    123
es             0x7b    123
fs             0x0    0
gs             0x33    51
(gdb) set $eax=4242
(gdb) continue
Continuing.
your token is xxxxxxxxxxxxxxx
```
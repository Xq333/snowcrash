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
gdb ./level13
b *0x0804859a
info registers
set $eax=4242
continue
```
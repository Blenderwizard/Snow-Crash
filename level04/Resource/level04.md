# Level04 Writeup

## Info 
User level04 has a setuid (flag04) perl script `level04.pl` in the root of the home directory.
 
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

## The Browser
 
Checking on a different machine, we can see with `nmap -p- IPOFVIRTUALMACHINE` that indeed the port 4747 is open. We can navigate with a browser and we are greeted with a blank page.
 
Changing the parameter x allows us to change the content of the site. By setting x to $(getflag), because of command substitution, the flag will appear in the browser.
 


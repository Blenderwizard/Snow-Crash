# Level04 Writeup

User level04 has a setuid perl script in the root of the home directory. The script runs as user flag04.

level04.pl:
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

It is immediatly apparent that the port 4747 is used to display the content created by script and that parameter x is used to pass information to the subroutine x. Using `nmap -p- vmport` shows that indeed the port 4747 is open and opening it in the browser shows an empty page.

Changing the parameter x changes the content within the page and since print is simply executing shell cod, we can abuse it by setting x to $(getflag), when executing the perl file, getflag will run because of command substitution, printing out the flag.
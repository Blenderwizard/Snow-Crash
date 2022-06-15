# Level03 Writeup

User level03 has a single file `level03` in their home directory. Using `file` we can see that `level03` is a setuid ELF binary, AKA an executable that will run as another user when executed. Using `ls`, we can confirm the setuid and the executable flag and that the binary runs as flag03. 

Executing the file gives us the folowing output: `Exploit me`. Using the strings command we can see that one of the strings is `/usr/bin/env echo Exploit me`. Meaning that the program uses either execve or system to execute `echo` to display the text.

This is where the security vulrability lies. Since `echo` is not hardcoded as a path, we can define our own `echo` script and add it to the PATH env variable, executing our own script instead of the normal `echo`. Giving our home directory read + write permisions with `chmod 777 ~` we create the folowing file named `echo`:
``` bash
#!/bin/bash

/bin/bash
``` 

After editing the PATH variable with `export PATH=~:$PATH`, executing `level03` drops us into the a shell for the user flag03. Running `getflag` gets us the flag.
# Level09 writeup

User level09 has a setuid (flag09) binary ELF file `level09` and a file `token` containing data. Running strings on `level09` we find the message "You should not reverse this" and "You need to provied only one arg.". Executing `level09` with the argument token returns tpmrh, revealing the pattern.

```
t o k e n 
+ + + + +
0 1 2 3 4
= = = = =
t p m r h
```

The contents of `token` are simply the result of a string passed as an argument to `level09` therefor reversing the progress is as simple as subtracting the letters position from it self. The values of non standard ascii characters can be seen with either hexdump or xxd.

```
f 4 k m m 6 p | = . p . n . . D B . D u { . . . .
- - - - - - - - - - - - - - - - - - - - - - - - -
                    1 1 1 1 1 1 1 1 1 1 2 2 2 2 2
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4
= = = = = = = = = = = = = = = = = = = = = = = = =
f 3 i j i 1 j u 5 y u e v a u s 4 1 q 1 a f i u q
```

The resulting string is the password for the user flag09, just switch to the account and run getflag.
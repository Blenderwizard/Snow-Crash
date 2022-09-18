# Level08 Writeup
 
## Info
 
User level08 has setuid `flag08` ELF binary `level08` and a plaintext file with no read permissions owned by `flag08` `token`. Using a decompiler like `ghidra` we can see the c source code of `level08`
 
`level08`
``` c
int main(int argc,char **argv,char **envp) {
   char *comparer; // Ghidra mess
   int f;
   size_t size;
   ssize_t ret;
   int in_GS_OFFSET;
   int fd;
   int rc;
   char buf [1024]; // Setting up a read buffer 
   undefined data [1024];
   int bufferoverflowcheck;
   bufferoverflowcheck = *(int *)(in_GS_OFFSET + 0x14); // Buffer overflow checker
   if (argc == 1) {
       printf("%s [file to read]\n",*argv);
       exit(1);
   }
   comparer = strstr(argv[1],"token"); // if name == token, deny access
   if (comparer != (char *)0x0) {
       printf("You may not access \'%s\'\n",argv[1]); 
   exit(1);
   }
   f = open(argv[1],0);
   if (f == -1) { // Attempt to open
       err(1,"Unable to open %s",argv[1]);
   }
   size = read(f,data,0x400); // read file
   if (size == 0xffffffff) {
       err(1,"Unable to read fd %d",f); // error occured
   }
   ret = write(1,data,size); // write file to stdout
   if (bufferoverflowcheck != *(int *)(in_GS_OFFSET + 0x14)) {
       __stack_chk_fail();
   }
   return ret;
}
```
 
## Victory
 
`level08` attempts to read a file passed to it as a command line argument. However if the argument matches token, it refuses to read it. 

Simply rename `token` to anything else, for example `mv token tok`, and your able to get the password of `flag08`. Login to `flag08` and run `getflag` for the flag!
 


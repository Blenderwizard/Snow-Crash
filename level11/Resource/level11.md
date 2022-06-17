# Level11 Writeup
 
## Info
 
User level11 has a setuid (flag11) lua `level11.lua` in the root of their home directory.
 
`level11.lua`
``` lua
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))
 
function hash(pass)
 prog = io.popen("echo "..pass.." | sha1sum", "r")
 data = prog:read("*all")
 prog:close()
 
 data = string.sub(data, 1, 40)
 
 return data
end
 
 
while 1 do
 local client = server:accept()
 client:send("Password: ")
 client:settimeout(60)
 local l, err = client:receive()
 if not err then
     print("trying " .. l)
     local h = hash(l)
 
     if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
         client:send("Erf nope..\n");
     else
         client:send("Gz you dumb*\n")
     end
 
 end
 
 client:close()
end
```
 
Attempting to execute `level11.lua` errors out in a failure port already in use, using nc we can indeed connect to the server. We can decrypt the hash, giving us the password NotSoEasy, but that will neither give us the flag through the program nor is the password for flag11 user.
 
## Victory
 
We can simply force the server to execute getflag and store the file somewhere temporarily. Example: `\";getflag > /tmp/flag;\"`
 


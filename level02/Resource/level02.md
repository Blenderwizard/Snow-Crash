# Level02 Writeup

## Info 

`level02` has a single file `level02.pcap` within their home directory. This file is a capture of network packets, and can be opened with a program like `Wireshark`.


## Wireshark

Opening the file with Wireshark shows communication between two hosts. Analyzing each of the packet's data we see that at one point one of the hosts sends a prompt for a login. Packet by packet we can reconstruct the response, the password `ft_waNDReL0L`.

## Victory

Loging into `flag02` with password `ft_waNDReL0L` and running `getflag` grabs the flag.
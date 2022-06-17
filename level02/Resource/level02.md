# Level02 Writeup

# Info 

User level02 has a single `level02.pcap` file within his home directory. Opening the pcap file with wireshark shows communication between two IPs. Analyzing each packet's data we see that at one point a machine sends a prompt for a login. Packet by packet we can see the response to the prompt and reconstruct the password "ft_waNDReL0L", allowing us to log into the flag02 user and get the flag.
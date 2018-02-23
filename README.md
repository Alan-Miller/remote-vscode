INSTALL 'REMOTE VSCODE'

CHECK SETTINGS

INSTALL COMMAND IN THIS FILE ON SERVER (COMING FROM THIS GITHUB REPO)
sudo wget -O /usr/local/bin/rmate(or rcode) https://raw.github.com/aurora/rmate/master/rmate

MAKE THE COMMAND EXECUTABLE
sudo chmod a+x /usr/local/bin/rmate(or rcode)

TURN ON 'REMOTE: START SERVER' IN COMMAND PALETTE IN VS CODE

OPEN TUNNEL 
ssh -NR 52698:localhost:52698 YOUR_SERVER

OPEN THE FILE IN VS CODE
rmate (or rcode) -w [-p 52698] file
  -w waits for VS Code to close the file and then exits (avoiding a killall rmate/rcode process later)
  -p is not needed because that port is the default


yay



IF YOU FORGET -W WHEN RUNNING RMATE/RCODE:
sudo killall rmate

SEE PROCESSES RUNNING 
netstat -ntpl
JUST IPV4
netstat -ntpl4
JUST IPV6
netstat -ntpl6
JUST NODE PROCESSES
netstat -ntpl | grep node
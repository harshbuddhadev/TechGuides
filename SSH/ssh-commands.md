# SSH Commands

1. SSH using Password
```bash
ssh john@serverdomain
ssh <username>@<serverdomain>
ssh <username>@<ip-address>
```
2. SSH by specifying Identity File(SSH Key)
```bash
ssh john@serverdomain -i "path/to/key"
```

3. SSH using specifying to use Password 
```bash
ssh john@serverdomain -o PubkeyAuthentication=no
```

4. Enable Verbose Mode
```bash
ssh john@serverdomain -v #will inform you what is happening mostly on your end.
ssh john@serverdomain -vv #will inform you low level on both ends.
ssh john@serverdomain -vvv #will inform you about everything from both ends.
```

4. SSH using specifying port
```bash
ssh john@serverdomain -p 22
```

## SSH Config File
The ssh config file can be used for configuring the default options that will apply when connecting with.

1. Open Config File
```bash
cd ~/.ssh #Open ssh Folder for the Current User
nano config #Open the config file for editing using nano 
code config #Open the config file for editing using VS Code
```

2. Format of Config File
```text
Host servername #Specify the Host Nickname which will be used in the ssh command
  HostName 192.168.1.11 #Specify Hostname/IP Address
  User johndoe #Specify Username (Optional)
  LogLevel DEBUG #Specify Debug Level(DEBUG1/DEBUG2/DEBUG3) (Optional)
  IdentityFile ~/.ssh/ssh_key #Specify Identity File Location (Optional)
  IdentitiesOnly yes #Specify if you want to use the SSH Key(Identity File) only
```

- Sample config Data
```text
Host servername
  HostName 192.168.1.11
  User johndoe
  LogLevel DEBUG
  IdentityFile ~/.ssh/ssh_key
  IdentitiesOnly yes
```

## SSH Port Tunneling
We can Tunnel Ports of Remote Machine to Local machine and vice-versa using SSH Port Tunneling

### Local Forwarding
If we want to forward the ports of a remote machine on our Local Machine, we can use Local Forwarding 

1. via Direct Command 
```bash
ssh john@server -L 9000:localhost:9000
ssh john@server -L 9000:192.168.1.10:9000 #We can change localhost to a specific IP Address to forward a port of Another Machine on the Remote Machine's Network

#Syntax
#ssh john@server -L port_to_open_locally:ip_of_remote_machine:remote_port_to_forward
```
2. via Config File
```text
Host servername
  HostName 192.168.1.11
  User johndoe
  LocalForward 8888 localhost:8000
```
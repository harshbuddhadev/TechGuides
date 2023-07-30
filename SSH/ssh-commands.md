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

3. SSH using specifying to use Password even if SSH Key is available
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

5. Skip Adding SSH Fingerprint to known_hosts file
	```bash
	ssh john@serverdomain -o UserKnownHostsFile=/dev/null
	```

6. Auto Add SSH Fingerprint to known_hosts file
	```bash
	ssh john@serverdomain -o StrictHostKeyChecking=no
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
	ssh john@server -L 9000:192.168.1.10:9000 
	#We can change localhost to a specific IP Address to forward a port of Another Machine on the Remote Machine's Network

	#Syntax
	#ssh john@server -L <port_to_open_locally>:<ip_of_remote_machine>:<remote_port_to_forward>
	```
2. via Config File
	```text
	Host servername
	  HostName 192.168.1.11
	  User johndoe
	  LocalForward 8888 localhost:8000
	  #LocalForward <port_to_open_locally> <ip_of_remote_machine>:<remote_port_to_forward>
	```

### Remote Forwarding
If we want to forward the ports of our local machine to the Remote Server, we can use Remote Forwarding 

> Note: To Use SSH Remote forwarding we need to Enable Gateway Ports in sshd_config file

#### Enabling GatewayPorts/clientspecified in sshd_config
1. open SSH Server Config
	```bash
	sudo nano /etc/ssh/sshd_config
	```
2. Update the "GatewayPorts" line or append if it does not exist
	```text
	GatewayPorts yes
	```

3. If you want to allow specific the forwarding to be accessible on specific addresses, set it to the following
	```text
	GatewayPorts clientspecified
	```
#### GatewayPorts Set to "yes"
1. via Direct Command (GatewayPorts Set to YES)
	```bash
	ssh john@server -R 5555:localhost:22 
	ssh john@server -R 5555:192.168.1.10:22 
	#We can change localhost to a specific IP Address to forward a port of Another Machine on the Local Machine's Network

	#Syntax
	#ssh john@server -L <port_to_expose>:<ip_of_local_machine>:<local_port_to_forward>
	```
2. via Config File
	```text
	Host servername
	  HostName 192.168.1.11
	  User johndoe
	  RemoteForward 5555 localhost:22
	  #LocalForward <port_to_expose> <ip_of_local_machine>:<local_port_to_forward>
	```

#### GatewayPorts Set to "clientspecified"

1. via Direct Command (GatewayPorts Set to YES)
	```bash
	ssh john@server -R 192.168.1.11:5555:localhost:22 
	ssh john@server -R 192.168.1.11:5555:192.168.1.10:22 
	#We can change localhost to a specific IP Address to forward a port of Another Machine on the Local Machine's Network

	#Syntax
	#ssh john@server -L <binding address>:<port_to_expose>:<ip_of_local_machine>:<local_port_to_forward>
	# use '*' for all addresses
	```
2. via Config File
	```text
	Host servername
	  HostName 192.168.1.11
	  User johndoe
	  RemoteForward 5555 localhost:22
	  #LocalForward <binding address>:<port_to_expose> <ip_of_local_machine>:<local_port_to_forward>
	```

---
## Reference Links
1. https://www.geeksforgeeks.org/ssh-port-forwarding/
2. https://www.ssh.com/academy/ssh/config
3. https://www.unixtutorial.org/ssh-port-forwarding/
4. https://www.clearlyip.ca/2022/07/12/ssh-port-forwarding/
5. https://www.section.io/engineering-education/ssh-tunneling/
6. https://serverfault.com/questions/1052158/how-to-add-local-forward-setting-to-my-ssh-config-file
7. https://www.howtogeek.com/devops/how-to-manage-an-ssh-config-file-in-windows-linux/
8. https://serverfault.com/questions/851132/how-to-diagnose-a-ssh-remote-port-forward-not-working
9. https://askubuntu.com/questions/50064/reverse-port-tunnelling
10. https://mohitgoyal.co/2021/01/16/overview-of-ssh-tunneling-and-configuration-options/
11. https://superuser.com/questions/767524/why-can-i-not-connect-to-a-reverse-ssh-tunnel-port-remotely-even-with-gatewaypo
12. https://www.ssh.com/academy/ssh/command
13. https://www.ssh.com/academy/ssh/tunneling-example
14. https://phoenixnap.com/kb/ssh-port-forwarding

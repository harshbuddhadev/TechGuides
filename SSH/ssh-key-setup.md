# How to Setup SSH Keys for Public/Private Key Authentication
This Guide Contains the Steps to Setup SSH Key for Authentication for SSH Login/GitLab/GitHub

## SSH Key Generation
1. Open any Terminal Window (Powershell/Windows-Terminal/Command-Prompt/Terminal)
2. Navigate to the ssh Folder for the User
    - For Windows PowerShell/Linux
        ```bash
        cd ~/.ssh
        ```
    - For Windows Command Prompt
        ```bash
        cd %homepath%/.ssh
        ```
3. Generate a new RSA key pair using the following command
    ```bash
    ssh-keygen
    ```

4. Enter a Location to Save the ssh keys with the file name
    > **Note:** If no input is given then ssh-key will be saved in the `.ssh` folder with `id_rsa` name <br> 
    If only a file name is provided, then the key will be saved to the current folder with the given name <br>
    If the location is specified along with the key name, then it will be save to the specified location with the given name

5. Enter a Passphrase

    > **Note:** A SSH key can be used without a passphrase, but it is highly recommended to have a passphrase for the ssh key incase of a security breach

6. Expected Output:

    ```text
    PS C:\Users\johndoe\.ssh> ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (C:\Users\johndoe/.ssh/id_rsa): C:\Users\johndoe/.ssh/keyname
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in C:\Users\johndoe/.ssh/keyname
    Your public key has been saved in C:\Users\johndoe/.ssh/keyname.pub
    PS C:\Users\johndoe\.ssh>
    ```

7. Now go the the location where the ssh key is stored, you will see 2 files, a public key and a private key.

8. Now we can use the Keys for Further Setup

## GitLab SSH Key Setup

1. Copy the Generated Public Key
2. Open GitLab
3. Click on your profile icon and go to Preferences
4. Click on SSH Keys on the Sidebar
5. Under the Key Section, Enter the **Public Key** copied previosly
6. Under Title, Enter a title in the following format
    ```
    <first-name> @ <device-name>
    johndoe@omen15 #Sample
    johndoe@rog #Sample
    johndoe@legion5p #Sample
    ```
7. Select `Authentication & Signing` as Usage type
8. Add Expiation Date
    > **Note:** Expiration Date is Optional but **HIGHLY RECOMMENDED**
9. Click on Add Key

## GitHub SSH Key Setup
1. Copy the Generated Public Key
2. Open GitHub
3. Click on your profile icon and go to Settings
4. Click on SSH and GPG Keys on the Sidebar
5. Click on New SSH Key
6. Under Title, Enter a title in the following format
    ```
    <first-name> @ <device-name>
    johndoe@omen15 #Sample
    johndoe@rog #Sample
    johndoe@legion5p #Sample
    ```
7. Select `Authentication Key` as Usage type
7. Under the Key Section, Enter the **Public Key** copied previosly
9. Click on Add SSH Key

## Remote Machine SSH Keys Setup
1. Access the Remote machine (Local Terminal,SSH, Etc.)
2. Open the SSH Folder for the Current user
    ```bash
    cd .ssh
    ```
3. Open/Create a file with filename `authorized_keys`
4. Paste the Public Key in the file (1 Key Per Line) and save the file

## Accessing remote machines
We can Directly SSH into Remote machine by specifying the Keys by using the following command

```bash
#Format
ssh <username>@<hostname/IP> -i <identityfile/private-key file location>

#Sample Key
ssh pi@192.168.1.101 -i ~/.ssh/keyname 
```

## Linking SSH Keys with Hosts and Verifying Keys
We can specify the required parameter for each SSH Server
1. Go to the `.ssh` folder and open/create a file name `config` (No Extensions)
2. Enter the following data and replace the identity file location in bash format
    ```text
    #GitLab
    Host gitlab.com
      IdentityFile ~/.ssh/gitlab_keyname
      IdentitiesOnly yes

    #GitHub
    Host github.com
      IdentityFile ~/.ssh/github_keyname
      IdentitiesOnly yes

    #Remote SSH Server
    Host pi
      HostName 192.168.1.101
      User pi
      IdentityFile ~/.ssh/keyname
      IdentitiesOnly yes
    ```
    > For Remote SSH Server, We can specify the Username/Hostnames etc, so we can connect by just using the Host ID , Example "`ssh pi`"


> **Note for Windows Users:** '~' stands for User Home Directory, for using file in other directory follow the following format "C:/Users/johndoe/.ssh/keyname", replace 'C' with the drive letter and paste the path in front of it by using '/' as delimiters

2. Verify that keys work by connecting to Server
    ```bash
    ssh git@gitlab.com
    ```

    - Expected Output:
        ```text
        PS C:\Users\johndoe\.ssh> ssh git@gitlab.com
        Enter passphrase for key 'C:\Users\johndoe/.ssh/gitlab_key':
        PTY allocation request failed on channel 0
        Welcome to GitLab, @johndoejohndoeadev!
        Connection to gitlab.com closed.
        PS C:\Users\johndoe\.ssh>
        ```
    - Incase you receive the following output , review the Keys Entered and follow the guide again
    ```text
    PS C:\Users\johndoe\.ssh> ssh git@gitlab.com
    git@gitlab.com: Permission denied (publickey).
    PS C:\Users\johndoe\.ssh>
    ```

---

## Reference Links
1. [SSH Essentials: Working with SSH Servers, Clients, and Keys](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys#removing-or-changing-the-passphrase-on-a-private-key)
2. [SSH config file for OpenSSH client](https://www.ssh.com/academy/ssh/config)
3. [SSH Key Permissions Windows](https://superuser.com/questions/1296024/windows-ssh-permissions-for-private-key-are-too-open)

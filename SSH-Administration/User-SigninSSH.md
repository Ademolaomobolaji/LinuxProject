# SSH Administration

## Objective
By the end of this project, we would be able to access a linux server with a user account created on the server.

In this project, I will:
- What is SSH
- Sing in a Redhat server hosted on a cloud service.
- Grant permission needed for the user to be able to ssh into the server.


## What is SSH? 

SSH (Secure Shell) is a secure way to connect to and manage another computer over a network using the default port 22.

### Sign in Server with User account (Training)

- use the command $ id training to confirm the user account exists.
- create a password for training with-  sudo passwd training 
- go to your local machine (laptop) and type the command 
"ssh-keygen"
- SSH-Administration/Screenshots/Keygen.png
- switch user on linux server to training and create a .ssh folder - mkdir .ssh
- create a authentication_key file in .ssh folder and save the public key you generated from your local device in the authentication_key file.
- run chmod 400 .ssh/authentication_key (set file to only be readable by training profile).
- switch user out of training on your linux server- use user with sudo right(admin) to edit the sshd files in ssh folder.

- goto the etc folder $ cd /etc/ssh 
- view the files and folder in this directory with the ls command
- ![alt text](<SSH folders.png>)

-  Use Vi to view edit the sshd_config file 
- ![alt text](set-passwordauthentication.png)
- Make sure all the Passwordauthentication is set to yes
- save and close the sshd_config file.
- Run sudo sshd -T | grep -i passwordauthentication to check the sshd server passwordauthentication configuration.
- if it returns passwordauthentication no
- run sudo grep -Rni "PasswordAuthentication" /etc/ssh/sshd_config /etc/ssh/sshd_config.d/ to check which directory is hosting the passwordauthentication configuration that needs to be changed.
- ![alt text](passwordauthentication-No.png)
- In this case, the /etc/ssh/sshd_config.d/50-cloud-init.conf file needs to be changed to yes
- sudo vi /etc/ssh/sshd_config.d/50-cloud-init.conf
- Edit passwordauthentication to yes and save.
- run the command again to check is it has been updated- run sudo grep -Rni "PasswordAuthentication" /etc/ssh/sshd_config /etc/ssh/sshd_config.d/
- ![alt text](Passwordauthentication-yes.png)

- Run sudo systemctl restart sshd to restart the sshd deamon 

- on your local bash - run ssh -i "serverprivatekey" training@serveripaddress/dns address.

![User sign in successfully](<sign in training.png>)

Successfully signed in training user using ssh connection.

# Adding users to EC2 instances with SSH Access and connecting to them using Ansible

## Step 1: Create 3 nodes- 1 master or Ansible management control node and 2 server nodes or worker nodes(using ec2 instances in AWS)
- In this exercise we have named them as ansible-host(a Red-hat RHEL host), webserver and dbserver(Amazon 2 Linux AMIs) respectively.
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/2.png">

## Step-2: Connect to the Ansible host using SSH client method or AWS EC2 console method.
```
sudo yum update
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/3.png">


## Step-3: Installing ansible in remote ansible host
```
sudo yum install ansible
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/7.png">

## Step-4: Confirm the ansible installation
```
ansible --version
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/8.png">

## Step-5: Check the present working directory and move back to the root location
```
pwd
cd ..
cd ..
ll
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/9.png">

## Step-6: Go to the etc/ansible folder
- Now go to the hosts file
  ```
  vi hosts
  ```
-  Now, update the host file with:
   ```
   [webservers]
   <Public_IP_Address_of_webserver_node>
   ```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/11.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/12.png">

## Step-7: Connect to the webserver node from aws console
- Go to the superuser
```
sudo su
```
- Now add a user
```
useradd -d /home/ansibleweb -m ansibleweb
```
- Now create a password for this user
```
passwd ansibleweb
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/14.png">

- Next, change the user to ansible web. Then go to the ansibleweb folder and create a .ssh file, change its user access and generate the rsa private and public keys
```
su - ansibleweb
cd home/ansibleweb
mkdir .ssh
chmod 700 .ssh/
ssh-keygen
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/15.png">

- Now move to the .ssh folder, then create a file named authorized_keys and copy paste the public key(got from cat id_rsa.pub)
```
cd .ssh
ll
cat id_rsa.pub
vi authorized_keys
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/16.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/17.png">

## Step-8: Now switch back to ansible-host instance. Then do
- Go to the superuser
```
sudo su
```
- Now add a user
```
useradd -d /home/ansiblehost -m ansiblehost
```
- Now create a password for this user
```
passwd ansiblehost
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/19.png">

- Go to the root location and chnage the owner of ansible folder
```
cd ..
cd ..
cd etc/ansible
chown ansiblehost:ansiblehost .
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/20.png">

- Create  a remote-web.key file in the ansible folder and copy paste the pivate key of the webserver instance(node)
``` 
vi remote-web.key
cat remote-web.key
``` 
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/21.png">

- In ansible host chnage the permissions of remote-web.key
``` 
chmod 600 rempote-web.key
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/23.png">

- Go to the ansible-host and execute the folowing
```
ssh -p22 -i remote-web.key ansibleweb@<Public_IP_adress_of_webserver>
```
- **Hurray!** we have connected from our ansible-host instance to the webserver instance using ssh.
- Now exit from the webserver
```
exit
```
- **Note:** The screenshots in violet background indicate that user **ansiblehost:awspsk** , **ansibleweb:awstgremoteweb** , **ansibledb:awstgremotedb**
- Now try connecting to the webserver using Ansible. Remember here webservers is the group that was created in the ansible hosts file.
```
ansible webservers -m ping
```
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/24.png">

## Step-9: Now repeat the steps from step-7 to step 9:

- But now the username would be dbserver and the groupname to be given in the host would be dbservers.
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/25.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/26.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/27.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/28.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/29.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/30.png">
<img src="https://github.com/kuluruvineeth/Devops/blob/main/ansibleServerConnectionExercise/screenshots/31.png">

**Kudos!! we have established our first ansible architecture successfully by connecting remote hosts to our ansible management control node**

# Setting up jenkins master-slave architecture for testing and production servers

## 1.Setting up master and slave servers in aws.
- setup 3 nodes of type t2.micro of ubuntu 20.04 os with security group instructions to allow all traffic and keep all other details as default.
- NOTE : we will be installing jenkins only on master node and monitor slave nodes using master node.

- On master node execute these instructions
```
sudo apt install openjdk-8-jdk
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins
service jenkins status
```
![jenkins1](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.6.png)

- on completing above instructions access this server on browser using public ipv4 address with port number 8080 i.e ipaddress:8080.

## 2.To configure jenkins
- Goto manage jenkins -> configure global settings -> Agents -> random -> click on save.
- Goto manage jenkins -> manage nodes -> new node.
- Create 2 slave nodes named slave-1 and slave-2 respectively with permanent agent option enabled and with the launch method as configure when master is triggered.Also add custom workdir path to be /home/ubuntu/jenkins and click on save.

![jenkins2](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.7.png)

![jenkins2](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.11.png)

- click on respective agents and download their agent.jar exectuable file.
- Now these agent.jar files are to be sent to the respective slave nodes that were created from above points.For ubuntu os run the following command.

```
scp -i ./<pemfileofmasternode>./agent.jar <username>@<ipv4addressofslavenode>:<destinationfolderofslavenode>
```
![jenkins3](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.15.png)

![jenkins4](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.16.png)

## 3.Connect to slave nodes from master node.
- Goto manage jenkins -> manage nodes -> click on respective slave and copy paste the code under Run from agent command line.
- Now copy paste the code in slave nodes.
```
sudo apt-get update
sudo apt install openjdk-8-jdk
```
![jenkins5](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.14.png)


![jenkins6](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.19.png)

![jenkins7](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.21.png)

- Now verify that the master and slave nodes are connected.

## 4. Configure jenkins to build project on slave-1(Test server) if successful build on slave-2(Production server).
- NOTE : make sure to duplicate sessions in slave node so that connection doesn't terminate.
- Install Docker on both the slaves.
```
sudo apt-get install docker.io
```
- verify docker installation using 
```
docker --version
```
- Headback to jenkins. create new job -> make it new freestyle project -> name it slave-1.Now under configure in the general section click on github project and copy paste the url of github project.Also enable the Restrict where this project can be run and type in slave-1 under label expression section.Go to source code management and enable git.

![jenkins8](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.25.png)

- Under the build section click Add build step -> Execute shell.Now run the below commands.
```
sudo docker rm -f $(sudo docker ps -a -q)
sudo docker build /home/ubuntu/workspace/Test -t test
sudo docker run -it -p 82:80 -d test
```
- NOTE : Make sure to run a custom container on the slave node before executing above commands.The above steps are to be executed for both test and production server.
- Now click on build.
- To check the above build step headover to <ipv4addressofslave>:<portnumber>/<githubwebappfolderlocation>
  
![jenkins9](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.29.png)
  
![jenkins10](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.30.png)
  
![jenkins11](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.31.png)
  
![jenkins12](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.33.png)
  
![jenkins13](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.35.png)
  
![jenkins14](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.38.png)
  
![jenkins15](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.39.png)
  
- Now we need to configure the servers in such a way that after testing production is built.To enable this Goto the test(freestyle project of slave-1) and under the post build actions click on build other projects and enter production(freestyle project of slave-2).
  
## 5. Build in the ci-cd pipeline for test and production servers.
  - Headover to manage jenkins -> manage plugins -> available.Now search for build pipeline,install the plugin.
  - In the jenkins home page click on the + sign near the All.
  - Now click the Build pipeline view, name the view as CiCd. Also under Build pipeline view title give CiCd and with other options as default and click save.
  
  ![jenkins16](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.40.png)
  
  - Under the Configure section of the CiCd pipeline,make Pipeline flow -> select initial job -> test.
  - Now click on Run to run the respective jobs.
  
  ![jenkins17](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.41.png)
  
  ![jenkins18](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.42.png)
  
  ![jenkins19](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.44.png)
  
  - Verify the deployment into production and test servers by heading to (<ipv4addressoftest/production>/<Githubwebappprojectfolder>/)
  
## 6. Using Github-Webhook to reflect changes made to github project in the test and production servers.
  - Goto jenkins dashboard -> test -> configure -> build triggers.And enable the github hook trigger fot GIT-Scm polling option and click save.
  - Headover to your github webapp project and goto settings -> Webhooks -> Add webhook.Now copy paste the ip address of your jenkins server(master node) under the payload url section and click addwebhook. 
  - NOTE : Verify that you get tick mark under the webhook section for the url that you copy pasted.
  - Now lets trigger a build by committing port changes to the github webapp project and see the changes made to test and production server.
  ```
  git clone <githuburl>
  cd <projectfolder>
  ls 
  nano index.html // make changes to this file
  git add .
  git commit -m "message"
  git push origin master
  ```
   ![jenkins20](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.50.png)
  
   ![jenkins21](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.52.png)
  
   ![jenkins22](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.53.png)
  
   ![jenkins23](https://github.com/kuluruvineeth/Devops/blob/main/jenkins-masterSlave/screenshots-2/2.54.png)
  
  
  
 
  
  






# Jenkins-Intro
Introduction to Jenkins

### > Create Server on Cloud 
Create an EC2 instance id 
### > How to install jenkins ? 

Link to install Jenkins : https://www.jenkins.io/doc/book/installing/linux/ 

Jenkins is build in Java and due to this we need java jdk on the server.

```sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
openjdk version "21.0.3" 2024-04-16
OpenJDK Runtime Environment (build 21.0.3+11-Debian-2)
OpenJDK 64-Bit Server VM (build 21.0.3+11-Debian-2, mixed mode, sharing)
```

```sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

Checking the status of jenkins
`systectl status jenkins`
```
● jenkins.service - Jenkins Continuous Integration Server
     Loaded: loaded (/usr/lib/systemd/system/jenkins.service; enabled; preset: >
     Active: active (running) since Sat 2025-08-23 17:42:46 UTC; 26s ago
   Main PID: 4460 (java)
      Tasks: 44 (limit: 1121)
     Memory: 291.5M (peak: 301.4M)
        CPU: 16.748s
     CGroup: /system.slice/jenkins.service
             └─4460 /usr/bin/java -Djava.awt.headless=true -jar /usr/share/java>

Aug 23 17:42:39 ip-172-31-88-79 jenkins[4460]: a67be0479c4f4c0480476edbf0732437
Aug 23 17:42:39 ip-172-31-88-79 jenkins[4460]: This may also be found at: /var/>
Aug 23 17:42:39 ip-172-31-88-79 jenkins[4460]: ********************************>
Aug 23 17:42:39 ip-172-31-88-79 jenkins[4460]: ********************************>
Aug 23 17:42:39 ip-172-31-88-79 jenkins[4460]: ********************************>
Aug 23 17:42:46 ip-172-31-88-79 jenkins[4460]: 2025-08-23 17:42:46.102+0000 [id>
Aug 23 17:42:46 ip-172-31-88-79 jenkins[4460]: 2025-08-23 17:42:46.139+0000 [id>
Aug 23 17:42:46 ip-172-31-88-79 systemd[1]: Started jenkins.service - Jenkins C>
Aug 23 17:42:46 ip-172-31-88-79 jenkins[4460]: 2025-08-23 17:42:46.421+0000 [id>
Aug 23 17:42:46 ip-172-31-88-79 jenkins[4460]: 2025-08-23 17:42:46.426+0000 [id>
```

` Sudo systectl enable jenkins` => This is to enable jenkins service to start automatically at boot time.


By default, Jenkins runs on port 8080. 
This can be changed in /etc/default/jenkins or /etc/sysconfig/jenkins on Linux.
```
# port for HTTP connector (default 8080; disable with -1)
HTTP_PORT=8080
```
Try accesing the jenkins using 44.203.41.217:8080

but wait ....... !!!! but this is blocked

go to the ec2 server security group and edit the inbound rule (Controls incoming traffic to your system or resource from external sources.) and add port 8080.

again try to acesss 44.203.41.217:8080 and boom ... !!! 

 <img width="800" height="400" alt="Screenshot 2025-08-23 at 11 26 23 PM" src="https://github.com/user-attachments/assets/7c7b1c93-2287-4ac7-9272-acc1cf797cab" />


Use this to fetch the password : `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` and set the username and password and you can move forward and start creating your job.

---

### > How to create First Jobs on Jenkins ? 

- Click on Create a Job
- Write the name of the project and click on freestyle project.

<img width="800" height="400" alt="create first project" src="https://github.com/user-attachments/assets/ab847bf2-911b-4eaa-bc04-33fd4f52dbf8" />


<img width="800" height="400" alt="Adding Build Step" src="https://github.com/user-attachments/assets/059d6e5c-dbff-419d-9742-aa0cafc74294" />


<img width="800" height="400" alt="Execute Shell Commands" src="https://github.com/user-attachments/assets/e8324bd4-4af0-445d-9cf0-11f3261b35d2" />

---

### > How to Create simple pipeline ? 


<img width="800" height="400" alt="Screenshot 2025-08-24 at 12 22 59 AM" src="https://github.com/user-attachments/assets/5cf09040-76ac-46f3-b7ce-bf19c1990e5c" />

---







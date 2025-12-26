# Jenkins-Intro
Introduction to Jenkins

### > Create Server on Cloud 
Create an EC2 instance id 
### > How to install jenkins ? 

Link to install Jenkins : https://www.jenkins.io/doc/book/installing/linux/ 

Jenkins is build in Java and due to this we need java jdk on the server.

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
openjdk version "21.0.3" 2024-04-16
OpenJDK Runtime Environment (build 21.0.3+11-Debian-2)
OpenJDK 64-Bit Server VM (build 21.0.3+11-Debian-2, mixed mode, sharing)
```

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
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

` sudo systectl enable jenkins` => This is to enable jenkins service to start automatically at boot time.


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

### How to Create simple pipeline ? 


<img width="800" height="400" alt="Screenshot 2025-08-24 at 12 22 59 AM" src="https://github.com/user-attachments/assets/5cf09040-76ac-46f3-b7ce-bf19c1990e5c" />

---

# What is Jenkins Master–Agent Architecture?

Jenkins follows a master–agent (earlier called master–slave) architecture to distribute workload and scale builds.

### Jenkins Master (Controller)

The master is the brain of Jenkins.

It is responsible for:

- Hosting the Jenkins UI (Dashboard)
- Managing jobs, pipelines, credentials, plugins
- Scheduling jobs to available agents
- Monitoring agent health
- Storing build configurations and logs (by default)

The master does NOT ideally execute heavy builds (to avoid overload).

### Jenkins Agent (Slave)

Agents are worker machines that actually execute the build jobs.

They can be:
- Physical servers
- Virtual machines
- Docker containers
- Kubernetes pods
- Cloud instances (AWS, GCP, Azure)

**Agents:**

- Receive instructions from the master
- Execute build steps (compile, test, deploy)
- Send results/logs back to the master


### High-Level Workflow

```
Developer → Git Push
        ↓
   Jenkins Master
        ↓
  Assigns job to Agent
        ↓
   Agent executes job
        ↓
  Sends result back to Master
```

<img width="809" height="499" alt="Screenshot 2025-12-26 at 12 05 46 PM" src="https://github.com/user-attachments/assets/7ab2f88d-b13b-44e0-9040-0af5a7c0f7e8" />



### How jenkins work with an agent and how to set up these agents ?

Create second Ec2 Server as a jenkins-agent, Since this is just an agent which perform task given by master node there is no need for the jenkins to be installed but we need to install Java on it. 

```
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
```

**How Master connect with an agent ?**

Prerequisite : 
- Agent must have java installed.
- SSH service running on the agent node.

**Generate SSH Key on Jenkins Master**

```
cd ~/.ssh
ssh-keygen
ls
Result : authorized_keys  id_ed25519  id_ed25519.pub 
cat id_ed25519.pub 
```

**Add Public Key to Agent Node**
```
cd ~/.ssh
vim authorized_keys
and paste the public key inside it.
```

**Configure SSH Credentials in Jenkins**

Manage Jenkins → Credentials → System → Global credentials (unrestricted).

**Click Add Credentials:**

- Kind: SSH Username with private key
- Username: (user on agent node)
- Private Key: Enter the private key (~/.ssh/id_rsa from master)
- ID: e.g., agent-ssh-key

**Add the Agent Node in Jenkins**

- Manage Jenkins → Nodes → New Node
- Enter node name → Select Permanent Agent → OK
- Configure:
  - Remote root directory: /home/jenkins (or any path on the agent)
  - Launch method: Launch agent via SSH
  - Host: IP or hostname of agent
  - Credentials: Select the SSH key you added
  - Host Key Verification Strategy: Choose Non verifying or Known hosts file (for security)
 - Click Save and Jenkins will try to connect via SSH and launch the agent.


<img width="400" height="558" alt="Screenshot 2025-08-24 at 10 27 18 AM" src="https://github.com/user-attachments/assets/2e3cff32-f54d-4b9f-86ca-db39c170bd58" />

---








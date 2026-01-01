# Declarative pipeline  


Building a declaritive where I will be deploying a django based application present on github and build docker image and start a container on ec2 instance.

github url : https://github.com/iam-veeramalla/Docker-Zero-to-Hero/tree/main/examples/python-web-app

<img width="1026" height="574" alt="Screenshot 2025-12-26 at 3 29 37 PM" src="https://github.com/user-attachments/assets/bddf2780-3aad-4e21-8015-e3e636733a69" />

<img width="1059" height="671" alt="Screenshot 2025-12-26 at 3 29 47 PM" src="https://github.com/user-attachments/assets/ec909fb7-0fc9-445b-ad61-438c8ad7a7fc" />


```
pipeline {
    agent { label "dog" }

    stages {
        stage('Code') {
            steps {
                echo 'this is cloning the code'
                git url : "https://github.com/iam-veeramalla/Docker-Zero-to-Hero.git", branch: "main"
                echo "cloning Succesful .... ######"
            }
        }
        stage('Build') {
            steps {
                echo "Building Docker image"
                dir('examples/python-web-app') {
                    sh 'docker build -t notes-app:latest .'
                }
                echo "Docker image build is succesful #######"
            }
        }
        stage('test') {
            steps {
                echo "this is the testing the code"
            }
        }
        stage('Deploy'){
            steps{
                echo "this is deploying the code"
                sh '''
                docker stop notes-app || true
                docker rm notes-app || true
                docker run -d --name notes-app -p 8000:8000 notes-app:latest
                '''
            }
        }
    }
}
```



 

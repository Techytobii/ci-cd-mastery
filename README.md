# CI/CD Mastery – Automating Deployment of an E-Commerce Website


## 🚀 Project Overview


This project demonstrates the automation of building, testing, and deploying a simple e-commerce web application using a CI/CD pipeline powered by Jenkins and Docker.


## 🛠️ Tools Used


- **Jenkins** – CI/CD Orchestration
- **Docker** – Containerization
- **GitHub** – Source Control
- **Docker Hub** – Container Registry
- **Ubuntu** – Server OS


## 📦 Tech Stack


- HTML (for static frontend)
- Dockerfile (to containerize the app)
- Jenkinsfile (to define CI/CD stages)


## 🛒 App Description


This project uses a simple HTML-based e-commerce homepage (`index.html`) to simulate an app deployment process. The app is served via a Docker container on port `8081`.


## 🧪 CI/CD Pipeline Stages


1. **Clone GitHub Repository**
2. **Build Docker Image**
3. **Push Docker Image to Docker Hub**
4. **Deploy Container from Image**


## 🔧 Jenkins Configuration Summary



- Jenkins installed on Ubuntu 20.04 and accessed via `localhost:8080`
- Required plugins installed: Docker Pipeline, GitHub Integration
- Created secret credential in Jenkins: `dockerhub-token` for Docker Hub PAT
- New pipeline job created using "Pipeline script from SCM" with GitHub repo
- Pipeline defined in a `Jenkinsfile` located at the repo root


### 📝 'Jenkinsfile'

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'yourdockerhubusername/ci-cd-mastery-app'
        CONTAINER_NAME = 'ecommerce-container'
        HOST_PORT = '8081'
        CONTAINER_PORT = '80'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Techytobii/ci-cd-mastery.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKERHUB_PASSWORD')]) {
                    sh '''
                        echo $DOCKERHUB_PASSWORD | docker login -u yourdockerhubusername --password-stdin
                        docker push $IMAGE_NAME
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d -p $HOST_PORT:$CONTAINER_PORT --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
    }
}
```

### 🐳 Dockerfile

```dockerfile
# Paste your Dockerfile contents here
FROM nginx:alpine
RUN rm -rf /usr/share/nginx/html/*
COPY app/ /usr/share/nginx/html
EXPOSE 80
```

### 🛒 index.html

```html
<!-- Paste your index.html contents here -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Mini E-Commerce Store</title>
  <!-- ... -->
</head>
<body>
  <!-- ... -->
</body>
</html>
```


## 🛠️ Jenkins Freestyle Project (Alternative Approach)

In addition to the Pipeline-as-Code method, this project can also be implemented using a **Jenkins Freestyle Project**. This approach is useful for beginners or for simple automation tasks.

### 📝 Freestyle Project Steps

1. **Create a New Freestyle Project** in Jenkins.
![jkns-newitem](/ci-cd-mastery/jenkins-scm/img/jenkins-dashboard.png)



2. **Configure Source Code Management**  
   - Select "Git" and provide your repository URL.
![git](/ci-cd-mastery/jenkins-scm/img/added-jkns-scmgt.png)



3. **Add Build Steps**

   - **Execute Shell** (Linux) or **Execute Windows batch command** (Windows):
     - Build Docker image:
       ```bat
       docker build -t yourdockerhubusername/ci-cd-mastery-app .
       ```
     - Push Docker image:
       ```bat
       echo $DOCKERHUB_PASSWORD | docker login -u yourdockerhubusername --password-stdin
       docker push yourdockerhubusername/ci-cd-mastery-app
       ```
     - Deploy container:
       ```bat
       docker rm -f ecommerce-container || true
       docker run -d -p 8081:80 --name ecommerce-container yourdockerhubusername/ci-cd-mastery-app
       ```

4. **Add Credentials**  

   - Use Jenkins Credentials Manager to securely store your Docker Hub password or PAT.
   ![dockerPAt](/ci-cd-mastery/img/12.PAT-docker.png)


5. **Build Triggers**  
   - Optionally, enable "Build periodically" or "GitHub hook trigger" for automation.

   - payload url
   ![payloadurl](/ci-cd-mastery/jenkins-scm/img/payload-url.png)

   - webhook created and activated 
   ![webhook](/ci-cd-mastery/jenkins-scm/img/webhook-created.png)

   - ![build-triggerjkns](/ci-cd-mastery/jenkins-scm/img/build-trigger.png)

   - build triggered
   ![build-triggered](/ci-cd-mastery/jenkins-scm/img/first-build.png)


### 🖼️ Freestyle Project Success
- ![build-success](/ci-cd-mastery/jenkins-scm/img/changes-in-jkns.png)


### ⚠️ Challenges Encountered

Jenkins build initially failed with script returned exit code 1

Resolved by correcting Docker credentials and using Jenkins' Credentials Manager securely

Container failed to start due to port conflict — solved using docker rm -f command before each run

These experiences strengthened my troubleshooting and log analysis skills in CI/CD

![failed-build](/ci-cd-mastery/img/failed-build.png)

```text
Started by user Oluwatobi Samuel Olofinkuade
ERROR: Unable to find Jenkinsfile from git https://github.com/Techytobii/ci-cd-mastery.git
Finished: FAILURE
```

###✅ Result


Successfully automated deployment with Jenkins

Docker image built and pushed to Docker Hub

Deployed application available via http://localhost:8081

Verified that the app renders correctly in browser




## 📸 Screenshots of the Implementation



### ✅ GitHub Repository Created

![01.ci-cd-repo](/ci-cd-mastery/img/01.ci-cd-m.repo.png)



### ✅ Connected to Ubuntu via SSH
![02ssh-ubuntu](/ci-cd-mastery/img/02.ssh-ubuntu.png)


### ✅ System Updated
![sys-updated](/ci-cd-mastery/img/03.update&upgrade.png)


### ✅ Repository Cloned
![repo-clone](/ci-cd-mastery/img/04.cloned-repo.png)


### ✅ Created index.html
![index-created](/ci-cd-mastery/img/05.index-html.png)


### ✅ Created Dockerfile
![created-dockerfile](/ci-cd-mastery/img/06.docker-file.png)


### ✅ Built Docker Image
![built-docker](/ci-cd-mastery/img/07.build.docker-file.png)


### ✅ Ran Docker Container
![ran-container](/ci-cd-mastery/img/08.test.container.png)


### ✅ Updated Server
![updated-server](/ci-cd-mastery/img/09.updated-server.png)


### ✅ Website Running
![website-up](/ci-cd-mastery/img/10.ecommerce.site.png)


### ✅ Added .gitignore
![gitignore](/ci-cd-mastery/img/11-gitignore.png)


### ✅ Created Jenkinsfile
![jknfile](/ci-cd-mastery/img/13.jknsfile.png)


### ✅ Created Docker Hub PAT
![docker-PAT](/ci-cd-mastery/img/12.PAT-docker.png)


### ✅ Pushed Files to GitHub
![oushed-files](/ci-cd-mastery/img/14.add-cmt-psh.png)


### ✅ Created Jenkins Pipeline Job
![pipeline-job](/ci-cd-mastery/img/15.new-pipeline.png)


### ✅ Configured Pipeline from SCM
![job-configured](/ci-cd-mastery/img/16.job-configure.png)


### ✅ Triggered Build
![build-now](/ci-cd-mastery/img/build-now.png)


### ✅ Pipeline Successful
![pipeline-success](/ci-cd-mastery/img/18.pipelinesuccess.png)


### ✅ Console Output

```text
Started by user Oluwatobi Samuel Olofinkuade
Obtained Jenkinsfile from git https://github.com/Techytobii/ci-cd-mastery.git
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential docker-hub-credentials
 > git.exe rev-parse --resolve-git-dir C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline\.git # timeout=10
Fetching changes from the remote Git repository
 > git.exe config remote.origin.url https://github.com/Techytobii/ci-cd-mastery.git # timeout=10
Fetching upstream changes from https://github.com/Techytobii/ci-cd-mastery.git
 > git.exe --version # timeout=10
 > git --version # 'git version 2.50.0.windows.1'
using GIT_ASKPASS to set credentials Docker Hub login for CI/CD pipeline
 > git.exe fetch --tags --force --progress -- https://github.com/Techytobii/ci-cd-mastery.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git.exe rev-parse "refs/remotes/origin/main^{commit}" # timeout=10
Checking out Revision 5e119f86662bf57689ed89898e04e45f70aa0cd6 (refs/remotes/origin/main)
 > git.exe config core.sparsecheckout # timeout=10
 > git.exe checkout -f 5e119f86662bf57689ed89898e04e45f70aa0cd6 # timeout=10
Commit message: "sh replaced with bat"
 > git.exe rev-list --no-walk 151adfd23c7536d1417b32be1088098dc994df0b # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] withCredentials
Masking supported pattern matches of %DOCKERHUB_CREDENTIALS% or %DOCKERHUB_CREDENTIALS_PSW%
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Clone Code)
[Pipeline] git
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
No credentials specified
 > git.exe rev-parse --resolve-git-dir C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline\.git # timeout=10
Fetching changes from the remote Git repository
 > git.exe config remote.origin.url https://github.com/Techytobii/ci-cd-mastery.git # timeout=10
Fetching upstream changes from https://github.com/Techytobii/ci-cd-mastery.git
 > git.exe --version # timeout=10
 > git --version # 'git version 2.50.0.windows.1'
 > git.exe fetch --tags --force --progress -- https://github.com/Techytobii/ci-cd-mastery.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git.exe rev-parse "refs/remotes/origin/main^{commit}" # timeout=10
Checking out Revision 5e119f86662bf57689ed89898e04e45f70aa0cd6 (refs/remotes/origin/main)
 > git.exe config core.sparsecheckout # timeout=10
 > git.exe checkout -f 5e119f86662bf57689ed89898e04e45f70aa0cd6 # timeout=10
 > git.exe branch -a -v --no-abbrev # timeout=10
 > git.exe branch -D main # timeout=10
 > git.exe checkout -b main 5e119f86662bf57689ed89898e04e45f70aa0cd6 # timeout=10
Commit message: "sh replaced with bat"
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build Docker Image)
[Pipeline] dir
Running in C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline\ecommerce-ci-cd
[Pipeline] {
[Pipeline] script
[Pipeline] {
[Pipeline] isUnix
[Pipeline] withEnv
[Pipeline] {
[Pipeline] bat

C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline\ecommerce-ci-cd>docker build -t "great2005/ci-cd-mastery-app:latest" . 
#0 building with "default" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 314B done
#1 DONE 0.0s

#2 [internal] load metadata for docker.io/library/nginx:alpine
#2 ...

#3 [auth] library/nginx:pull token for registry-1.docker.io
#3 DONE 0.0s

#2 [internal] load metadata for docker.io/library/nginx:alpine
#2 DONE 3.3s

#4 [internal] load .dockerignore
#4 transferring context: 2B done
#4 DONE 0.0s

#5 [internal] load build context
#5 transferring context: 59B done
#5 DONE 0.0s

#6 [1/3] FROM docker.io/library/nginx:alpine@sha256:b2e814d28359e77bd0aa5fed1939620075e4ffa0eb20423cc557b375bd5c14ad
#6 resolve docker.io/library/nginx:alpine@sha256:b2e814d28359e77bd0aa5fed1939620075e4ffa0eb20423cc557b375bd5c14ad 0.0s done
#6 DONE 0.0s

#7 [2/3] RUN rm -rf /usr/share/nginx/html/*
#7 CACHED

#8 [3/3] COPY app/ /usr/share/nginx/html
#8 CACHED

#9 exporting to image
#9 exporting layers done
#9 exporting manifest sha256:8ee581392b44b7e4d2df2347b106d893e690c5bf132b6527f37234e9a9411cc2 done
#9 exporting config sha256:01b5ab1303d04f9f0b64c386e991e0cca65c0ccfb71c8180bc8c73e8378c9fcd done
#9 exporting attestation manifest sha256:58a126c1f4f93832f127e5dc408a4cc1468ba061a85612ee87ff7ec6d885c8ed
#9 exporting attestation manifest sha256:58a126c1f4f93832f127e5dc408a4cc1468ba061a85612ee87ff7ec6d885c8ed 0.0s done
#9 exporting manifest list sha256:a5b2e93728d3eecaabbb02f8e7f5b2665f68b13ff2ed7ef7dff7de6b1effd050 0.0s done
#9 naming to docker.io/great2005/ci-cd-mastery-app:latest done
#9 unpacking to docker.io/great2005/ci-cd-mastery-app:latest 0.0s done
#9 DONE 0.2s
[Pipeline] }
[Pipeline] // withEnv
Did you forget the `def` keyword? WorkflowScript seems to be setting a field named dockerImage (to a value of type Image) which could lead to memory leaks or other issues.
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // dir
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Docker Hub Login & Push)
[Pipeline] script
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] withDockerRegistry
$ docker login -u great2005 -p ******** https://index.docker.io/v1/
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
Login Succeeded
[Pipeline] {
[Pipeline] isUnix
[Pipeline] withEnv
[Pipeline] {
[Pipeline] bat

C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline>docker tag "great2005/ci-cd-mastery-app:latest" "great2005/ci-cd-mastery-app:latest" 
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] isUnix
[Pipeline] withEnv
[Pipeline] {
[Pipeline] bat

C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline>docker push "great2005/ci-cd-mastery-app:latest" 
The push refers to repository [docker.io/great2005/ci-cd-mastery-app]
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
960638c98b9a: Pushed
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
00c7cea1033a: Waiting
92971aeb101e: Waiting
2c127093dfc7: Waiting
3b7062d09e02: Waiting
4a8d626f6816: Waiting
fb746e72516f: Waiting
a9ff9baf1741: Waiting
b55ed7d7b2de: Waiting
fe07684b16b8: Waiting
63dda2adf85b: Waiting
00c7cea1033a: Layer already exists
92971aeb101e: Layer already exists
2c127093dfc7: Layer already exists
3b7062d09e02: Layer already exists
4a8d626f6816: Layer already exists
fb746e72516f: Layer already exists
a9ff9baf1741: Layer already exists
b55ed7d7b2de: Layer already exists
fe07684b16b8: Layer already exists
63dda2adf85b: Layer already exists
latest: digest: sha256:a5b2e93728d3eecaabbb02f8e7f5b2665f68b13ff2ed7ef7dff7de6b1effd050 size: 856
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withDockerRegistry
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Deploy Container)
[Pipeline] script
[Pipeline] {
[Pipeline] bat

C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline>docker stop ecommerce-container   || true
Error response from daemon: No such container: ecommerce-container
'true' is not recognized as an internal or external command,
operable program or batch file.

C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline>docker rm ecommerce-container   || true
Error response from daemon: No such container: ecommerce-container
'true' is not recognized as an internal or external command,
operable program or batch file.

C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline>docker run -d -p 8081:80 --name ecommerce-container great2005/ci-cd-mastery-app:latest 
3c52e601ad66130500dab059da9de65ca74ada1501f9160624f37816bc7d36bb
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Declarative: Post Actions)
[Pipeline] script
[Pipeline] {
[Pipeline] bat

C:\ProgramData\Jenkins\.jenkins\workspace\ci-cd-mastery-pipeline>docker system prune -f 
Deleted Containers:
823ccbb8ef28da592f3dbe5c64fd7c24dbd6d123f657ffcc9fc1fa3af4754af6

Deleted Networks:
minikube
multi-container-app_default

Deleted Images:
untagged: sha256:e7c9bc3bc515d5e73ce1f1457208edbf01958ccbbe3eec5a3b2dad556bc2b960
untagged: sha256:42e917aaa1b5bb40dd0f6f7f4f857490ac7747d7ef73b391c774a41a8b994f15
deleted: sha256:42e917aaa1b5bb40dd0f6f7f4f857490ac7747d7ef73b391c774a41a8b994f15
deleted: sha256:c375ae41bb29c0615ab18452822fedf1981f8ba7a7b00508b3d901b7ab682947
deleted: sha256:f876bfc1cc63d905bb9c8ebc5adc98375bb8e22920959719d1a96e8f594868fa
deleted: sha256:fd674058ff8f8cfa7fb8a20c006fc0128541cbbad7f7f7f28df570d08f9e4d92
deleted: sha256:566e42bcee1cd697dcab6098c082789af33bc8cbcbaa95c8adbad87283c85c75
deleted: sha256:2b99b9c5d9e5679c839357944d083c55c4c045e2cae21f84dfcfe5841e2ea59b
deleted: sha256:bd98674871f548eff8a4e4f3d4aa1ba504320ccbabfb0a217c4ea5c23b6144fd
deleted: sha256:1e109dd2a0d75eb2ab2491daec5b300e99027ffdd528998612b693f3347b97e4
deleted: sha256:da8cc133ff821c8b0ac7a6667e3a2e70ee6eb04f850e38088f59720017a869db
deleted: sha256:c44f27309ea1a5e557aff07fbd5ece457d5cb85583f795b34f342d5550a08a5c

Deleted build cache objects:
kb8kfkls0pko279387ahbx92q
j6l9laist28ntsa4q9h04d8pc
labytltrwfabeiap6vpttj4dw
rp3ar2fwpacc3uu0bbhper1rx
zlidfxiruusn6a1fyquxedavy
lmtvlbpke339d76goo3t52nsw
5xbys1aavdj0xdigwn5w4iwbm
igq3n9y6uubcsf8q4fnojobin
fyudnvezrabu091ft075xs1x3
uevzi0m4v7ztvqexuoo013twm
t4j4x006ileqe76hvg1u4rqv8
vsw52dfgfxyhtfgq4teitmfpt
tdvswetmxba8xjdeiewh3b820
ndf7cmpal8h43juryb53pwv75
v2t1ogtcidy3f8lhadd1gb0us
i10fet1o0gvrsxu30nd01vl0r
pn6vo0xkmkw8mqsd256i7p2y1
wknd405s4z3omy1amesejoe7w
mfh8jwi7iyfrutoblmz0urr5k
6xub5t0h2xvwj5zqwa1lmdsot
00yn293dtn239s69hcjyp3ing
qdaukthdx6bjy3vk3ey203urp
lh0057tzcmq1f4onv4qsocxqr
wuq8eoh0noaw6wlq1m7wvkw9q
vuqaimxov3cduxfvh4yilbnvo
ro85slqir5zv8dqzr2ie94c5g
nogc0ommoylexvkfa2921y911
k09himym3hth0gk040rs5l443
oiod571lgq7pa4qnv2ciygqqa
k8d2o92lwd5qbsyem6t250fmw
ud9hteymvbjpjnimali5ukily
svreh24mpuz76io4ndebg0avj
lriqfocz404e7boyodsujtv27
r6yhwmvnzkws29jctdxcl1q3g
nuxl23rmtq48qgp0mdr2h8be7
jzlbdazxjwvor9xiblnqlgk0r
99yu4ahc1bkidtc81exl1ckse
bbuwmzed04m22p2s6vjq3c7mm
f8oyylolz5s0d98fm0t6swv5f
u2bllyxb386v3yf6dawjbic5y
x01ctpt73nyvgcjxkrkvuwnnp
wfofv86ozbpy3ssahc2acrp31
a6zzeeaz66one9s9ujwq9nczz
tlqytqv74gip2bdflq93l841j
wg1ecy8oljnj7u91d4ctikxr5
vkcngn4ggpxb0o8jqwjo2wlqr
ix53h0t49nyxsbgqq37ziu5gh
zcqtacgye2ngfbsekpvxvc2qs
oo8suxq3ktjktlwdycrdh0dls
kudo4sftfy2zwbfyr1j9zk1e5
0pwiosvcgs5hxdok8kygsm1l8
kri6nf76h45aosic6f1w0eofn

Total reclaimed space: 1.872GB
[Pipeline] }
[Pipeline] // script
[Pipeline] echo
✅ Build and deployment succeeded!
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withCredentials
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```




### ✅ Web App Running on Port 8081
![websiteup](/ci-cd-mastery/img/websiteup.png)


### 🔗 GitHub Repository

👉 Click here to view the repo https://github.com/Techytobii/ci-cd-mastery.git

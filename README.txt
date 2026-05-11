```text
==========================
FULL DEVOPS PROJECT GUIDE
Docker + GitHub + Jenkins
==========================


STEP 1 — INSTALL DOCKER
=======================

Open Ubuntu terminal and run:

sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

sudo apt install docker.io -y

sudo systemctl start docker

sudo systemctl enable docker

docker --version

Test Docker:

sudo docker run hello-world


If you see:
“Hello from Docker!”

Docker is installed successfully.


==================================================

STEP 2 — CREATE GITHUB REPOSITORY
=================================

1. Open:
   https://github.com

2. Login

3. Click:
   New Repository

4. Repository name:
   jenkins-docker

5. Select:
   Public

6. Click:
   Create Repository


==================================================

STEP 3 — CREATE PROJECT FILES
=============================

Inside repository create these files:

1. app.py
2. requirements.txt
3. Dockerfile
4. Jenkinsfile


==================================================

STEP 4 — CREATE app.py
======================

Click:
Add file → Create new file

Filename:
app.py

Paste:

from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Docker + Jenkins!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

Scroll down

Commit message:
Added app.py

Click:
Commit new file


==================================================

STEP 5 — CREATE requirements.txt
================================

Click:
Add file → Create new file

Filename:
requirements.txt

Paste:

flask

Commit file.


==================================================

STEP 6 — CREATE Dockerfile
==========================

Click:
Add file → Create new file

Filename:
Dockerfile

Paste:

FROM python:3.9

WORKDIR /app

COPY . /app

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "app.py"]

Commit file.


==================================================

STEP 7 — CREATE Jenkinsfile
===========================

Click:
Add file → Create new file

Filename:
Jenkinsfile

Paste:

pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-python-app .'
            }
        }

        stage('Push Docker Image') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'

                    sh 'docker tag my-python-app YOUR_DOCKERHUB_USERNAME/my-python-app:latest'

                    sh 'docker push YOUR_DOCKERHUB_USERNAME/my-python-app:latest'
                }
            }
        }
    }
}


IMPORTANT:

Replace:

YOUR_DOCKERHUB_USERNAME

with your actual DockerHub username.


Example:

akshaym000


Commit file.


==================================================

STEP 8 — CLONE REPOSITORY IN UBUNTU
===================================

Open terminal:

git clone https://github.com/akshaym000/jenkins-docker.git

Go inside folder:

cd jenkins-docker

Check files:

ls

You should see:

Dockerfile
Jenkinsfile
app.py
requirements.txt


==================================================

STEP 9 — BUILD DOCKER IMAGE
===========================

Run:

sudo docker build -t myapp-image .

IMPORTANT:
Dot (.) at end is required.


==================================================

STEP 10 — CHECK DOCKER IMAGES
=============================

Run:

sudo docker images

You should see:

myapp-image


==================================================

STEP 11 — RUN DOCKER CONTAINER
==============================

Run:

sudo docker run -p 5000:5000 myapp-image


==================================================

STEP 12 — OPEN APPLICATION
==========================

Open browser:

http://localhost:5000

You should see:

Hello from Docker + Jenkins!


==================================================

STEP 13 — CREATE DOCKERHUB ACCOUNT
==================================

Open:

https://hub.docker.com

Create account.

Remember your username.


==================================================

STEP 14 — LOGIN TO DOCKERHUB
============================

Run:

docker login -u YOUR_DOCKERHUB_USERNAME

Enter password.


==================================================

STEP 15 — PUSH IMAGE TO DOCKERHUB
=================================

Tag image:

docker tag myapp-image YOUR_DOCKERHUB_USERNAME/my-python-app:latest

Push image:

docker push YOUR_DOCKERHUB_USERNAME/my-python-app:latest


==================================================

STEP 16 — DOWNLOAD JENKINS
==========================

Open:

https://www.jenkins.io/download/

Download:

jenkins.war


==================================================

STEP 17 — RUN JENKINS
=====================

Run:

java -jar jenkins.war --httpPort=8080


==================================================

STEP 18 — OPEN JENKINS
======================

Open browser:

http://localhost:8080


==================================================

STEP 19 — UNLOCK JENKINS
========================

Terminal will show admin password.

Copy password.

Paste into Jenkins unlock page.

Click Continue.


==================================================

STEP 20 — INSTALL PLUGINS
=========================

Click:

Install Suggested Plugins

Wait for installation.

Create admin user.


==================================================

STEP 21 — INSTALL DOCKER PIPELINE PLUGIN
========================================

Go to:

Manage Jenkins
→ Plugins
→ Available Plugins

Search:

Docker Pipeline

Install plugin.


==================================================

STEP 22 — ADD DOCKERHUB CREDENTIALS
===================================

Go to:

Manage Jenkins
→ Credentials
→ Global
→ Add Credentials

Select:

Username with password

Enter:
DockerHub username
DockerHub password

ID:

dockerhub

Click Create.


==================================================

STEP 23 — CREATE PIPELINE JOB
=============================

Go to Jenkins dashboard.

Click:

New Item

Name:

docker-pipeline

Select:

Pipeline

Click OK.


==================================================

STEP 24 — CONNECT GITHUB REPOSITORY
===================================

Inside job:

Pipeline
→ Pipeline script from SCM

SCM:
Git

Repository URL:

https://github.com/akshaym000/jenkins-docker.git

Script Path:

Jenkinsfile

Save.


==================================================

STEP 25 — RUN PIPELINE
======================

Click:

Build Now

Jenkins will automatically:

1. Pull code from GitHub
2. Build Docker image
3. Login to DockerHub
4. Push image to DockerHub


==================================================

FINAL RESULT
=============

You now have:

✓ Flask app
✓ Dockerized application
✓ GitHub repository
✓ DockerHub image
✓ Jenkins CI/CD pipeline
✓ Automated Docker image deployment

```

1. app.py

from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Docker + Jenkins!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)


--------------------------------------------------

2. requirements.txt

flask


--------------------------------------------------

3. Dockerfile

FROM python:3.9

WORKDIR /app

COPY . /app

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "app.py"]


--------------------------------------------------

4. Jenkinsfile

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


--------------------------------------------------

5. Commands

git clone https://github.com/akshaym000/jenkins-docker.git

cd jenkins-docker

sudo docker build -t myapp-image .

sudo docker images

sudo docker run -p 5000:5000 myapp-image


--------------------------------------------------

6. Open Browser

http://localhost:5000


--------------------------------------------------

7. DockerHub Commands

docker login -u YOUR_DOCKERHUB_USERNAME

docker tag myapp-image YOUR_DOCKERHUB_USERNAME/my-python-app:latest

docker push YOUR_DOCKERHUB_USERNAME/my-python-app:latest


--------------------------------------------------

8. Run Jenkins

java -jar jenkins.war --httpPort=8080

Open:

http://localhost:8080

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "salary-finder"
        DOCKER_TAG   = "1.0"
        DOCKERHUB_USER = "harsh101011"  // change this
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/harsh101011/salary-finder-java.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Run App (Test)') {
            steps {
                sh 'java -jar target/salary-finder-1.0.0.jar'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker tag $DOCKER_IMAGE:$DOCKER_TAG $DOCKERHUB_USER/$DOCKER_IMAGE:$DOCKER_TAG'
                    sh 'docker push $DOCKERHUB_USER/$DOCKER_IMAGE:$DOCKER_TAG'
                }
            }
        }
    }
}

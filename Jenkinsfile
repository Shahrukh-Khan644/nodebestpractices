pipeline {
    agent any

    environment {
        IMAGE_NAME = 'shahrrukh/node-jenkins-docker'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Shahrukh-Khan644/nodebestpractices.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh 'echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy Container') {
            steps {
                // Optionally stop old container if running
                sh 'docker stop node-app || true && docker rm node-app || true'

                // Run new container
                sh 'docker run -d --name node-app -p 3000:3000 $IMAGE_NAME'
            }
        }
    }
}

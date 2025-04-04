pipeline {
    agent any
    environment {
        PATH = "/usr/local/bin:$PATH:/Users/luizaugustogrein/.nvm/versions/node/v20.11.1/bin:$PATH"
    }
    stages {
        stage('checkout') {
            steps {
                checkout scm
            }
        }
        stage('install') {
            steps {
                sh 'npm install'
            }
        }
        stage('build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('test') {
            steps {
                sh 'npm run test'
            }
        }
        stage('build image') {
            steps {
                sh 'docker build -t nestjs-server:1.0 .'
            }
        }
        stage('docker push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                    sh 'docker tag nestjs-server:1.0 {IMAGEM_DOCKER}'
                    sh 'docker push {IMAGEM_DOCKER}'
                    sh 'docker logout'
                }
            }
        }
    }
}
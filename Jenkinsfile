pipeline {
    agent any
    tools {
        maven 'maven_3_5_0'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/lucas-lopes/devops-integration']])
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t lucasgt/devops-integration .'
                }
            }
        }
        stage('Add Image to Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'brasil-123456', variable: 'brasil')]) {
                        sh 'docker login -u lucasgt -p ${brasil}'
                    }
                    sh 'docker push lucasgt/devops-integration'
                }
            }
        }
    }
}
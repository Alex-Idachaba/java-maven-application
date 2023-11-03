#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/Alex-Idachaba/jenkins-shared-library.git',
     credentialsId: 'github-repo'
    ]
)

pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment {
        IMAGE_NAME = 'alexdevops31/jenkins-demo-app:java-maven-3.0'
    }
    stages {
        stage('build') {
            steps {
                script {
                    echo "Building application jar..."
                    buildJar()
                }
            }
        }
        stage('build image') {
            steps {
                script {
                   echo 'building docker image...'
                   buildImage(env.IMAGE_NAME)
                   dockerLogin()
                   dockerPush(env.IMAGE_NAME)
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo 'deploying docker image to EC2...'
                    def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"
                    sshagent(['ec2-server-key']) {
                        sh "scp docker-compose.yaml ec2-user@18.207.199.90:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@18.207.199.90 ${dockerComposeCmd}"
                }
                }
            }
        }
    }
}

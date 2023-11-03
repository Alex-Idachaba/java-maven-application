
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
                    def dockerCmd = "docker run -p 8080:8080 -d ${IMAGE_NAME}"
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@100.26.254.11 ${dockerCmd}"
                }
                }
            }
        }
    }
}

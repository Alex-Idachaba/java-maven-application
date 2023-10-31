
pipeline {
    agent any
    stages {
        stage("init") {
            steps {
                echo "initializing the repo"
            }
        }
        stage("build jar") {
            steps {
                echo "building the artifact"
            }
        }
        stage("build image") {
            steps {
                echo "building the image"
            }
        }
        stage("deploy") {
            steps {
                echo "deploying the application"
            }
        }
    }   
}
pipeline {
    agent any
    environment {
        REGISTRY = "vamsiammineni/wep-app"
        REGISTRYCREDS = 'dockerhub'
        DOCKER_TAG = getDockerTag()
    }
    stages {
        stage('Unit Tests'){
            steps {
                script {
                   echo "unit tests"
                }
            }
        }
        stage('Build Docker Image') {
            steps{
                echo 'Starting to build docker image'
                script {
                    dockerImage = docker.build("vamsiammineni/wep-app:${env.BUILD_ID}")
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('', REGISTRYCREDS){
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $REGISTRY:${env.BUILD_ID}"
            }
        }
    }
}

def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
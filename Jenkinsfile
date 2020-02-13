pipeline {
    agent any
    environment {
        REGISTRY = "vamsiammineni/wep-app"
        REGISTRYCREDS = 'dockerhub'
        DOCKER_TAG = getDockerTag()
    }
    stages {
        stage('Build Docker Image') {
            steps{
                echo 'Starting to build docker image'
                script {
                    def dockerImage = docker.build("vamsiammineni/wep-app:${env.BUILD_ID}")
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io', REGISTRYCREDS){
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}

def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
pipeline {
    agent any
    environment {
        REGISTRY = "vamsiammineni/wep-app"
        DOCKER_TAG = getDockerTag()
    }
    stages {
        stage('Build Docker Image') {
            steps{
                script {
                    def dockerImage = docker.build("vamsiammineni/wep-app:${DOCKER_TAG}", '.')
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com', '${dockerhub}'){
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
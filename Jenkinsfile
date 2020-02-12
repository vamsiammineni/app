pipeline {
    agent any
    environment {
        REGISTRY = "vamsiammineni/wep-app"
        REGISTRY_CREDENTIALS = ‘dockerhub’
        DOCKER_TAG = getDockerTag()
    }
    stages {
        stage('Build Docker Image') {
            steps{
                script {
                    docker.build REGISTRY + ":${DOCKER_TAG}"
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('', REGISTRY_CREDENTIALS){
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
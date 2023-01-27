pipeline{
    agent any

    stages {

        stage ('Build Docker Image') {
            steps{
                script {
                    dockerapp = docker.build("jmendes82/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage ('Push Docker Image') {
            steps{
                script {
                    dockerapp = docker.withRegistry("https://registry.hub.docker.com", 'https://hub.docker.com/') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}
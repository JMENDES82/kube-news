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
                    dockerapp = docker.withRegistry("https://registry.hub.docker.com", 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage ('Deploy Kubernetes') {
            environment {
                tag_version = "${env.BUILD_ID}"
            }

            steps{
                script {
                    withKubeConfig([credentialsId: 'kubeconf']){
                        sh 'sed -i "s/{{TAG}}/$tag_version/g" ./src/deployment.yaml'
                        sh 'kubectl apply -f ./src/deployment.yaml'
                    }
                }
            }
        }
    }
}
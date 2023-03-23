pipeline {
    agent any

    stages {
        stage('Checkout sources') {
            steps {
                git url:'https://github.com/deividfae/pedelogo-catalogo.git', branch: 'main'
            }
        }

        stage('Install Docker') {
            steps {
                script {
                    sh 'curl -fsSL https://get.docker.com -o get-docker.sh'
                    sh 'sudo sh get-docker.sh'
                    sh 'sudo usermod -aG docker jenkins'
                }
            }
        }
        stage('Build image') {
            steps {
                script {
                    sh "docker build -t deividfae/api-produto:${env.BUILD_ID} -f ./src/PedeLogo.Catalogo.Api/Dockefile ."
                }            
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        docker.push('latest')
                        docker.push("${env.BUILD_ID}")
                    }
                }
            }

        }

    }
}
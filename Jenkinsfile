pipeline {
    agent any

    stages {
        stage('Checkout sources') {
            steps {
                git url:'https://github.com/deividfae/pedelogo-catalogo.git', branch: 'main'
            }
        }

        stage('Build image') {
            steps {
                script {
                    dockerapp = docker.build("deividfae/api-produto:${env.BUILD_ID}",
                      '-f ./src/PedeLogo.Catalogo.Api/src/Dockefile .')
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
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
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        def dockerBuildArgs = [
                            "--build-arg", "foo=bar",
                            "--timeout=900" // definir o tempo limite para 900 segundos
                        ]
                        def dockerImage = docker.build(
                            "deividfae/api-produto:${env.BUILD_ID}",
                            "-f ./src/PedeLogo.Catalogo.Api/Dockerfile .",
                            dockerBuildArgs
                        )
                        dockerImage.push()
                    }
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
pipeline {
    agent any

    stages {
        stage('Clonar y Actualizar Código') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/RicardoYalico/websocket-server.git'
                    ]]
                ])
                sh 'pwd'
            }
        }

        stage('Construir y Desplegar Docker') {
            steps {
                script {
                    sh '''
                        docker build -t socket-server .
                        docker stop socket-server-container || true
                        docker rm socket-server-container || true
                        docker run -d -p 8080:8080 --name socket-server-container --restart unless-stopped socket-server
                        docker image prune -a -f
                    '''
                }
            }
        }
    }
}

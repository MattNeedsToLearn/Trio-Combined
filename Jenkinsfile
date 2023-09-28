pipeline {
    agent any
    stages {
        stage('Build Images') {
            steps {
                sh '''
                    docker build -t mattneedstolearn/triodb:latest ./db
                    docker build -t mattneedstolearn/triodb:${BUILD_NUMBER} ./db
                    docker build -t mattneedstolearn/trioflask:latest ./flask-app
                    docker build -t mattneedstolearn/trioflask:${BUILD_NUMBER} ./flask-app
                    docker build -t mattneedstolearn/trionginx:latest ./nginx
                    docker build -t mattneedstolearn/trionginx:${BUILD_NUMBER} ./nginx
                '''
            }
        }
        stage('Push Images') {
            steps {
                sh '''
                    docker push mattneedstolearn/triodb
                    docker push mattneedstolearn/triodb:${BUILD_NUMBER}
                    docker push mattneedstolearn/trioflask
                    docker push mattneedstolearn/trioflask:${BUILD_NUMBER}
                    docker push mattneedstolearn/trionginx
                    docker push mattneedstolearn/trionginx:${BUILD_NUMBER}
                '''
            }
        }
        stage('Deploy Containers') {
            environment {
                MYSQL_ROOT_PASSWORD = credentials ('MYSQL_ROOT_PASSWORD')

            }
            steps {
                sh '''
                    ssh jenkins@matt-vm2 << EOF
                    docker pull mattneedstolearn/triodb
                    docker pull mattneedstolearn/trioflask
                    docker pull mattneedstolearn/trionginx
                    docker network create trio
                    docker volume create trio
                    docker run -d -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} -v trio:/var/lib/mysql --network trio --name mysql mattneedstolearn/triodb
                    docker run -d -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} --network trio --name flask-app mattneedstolearn/trioflask
                    docker run -d -e -p 80:80 --network trio --name triorp mattneedstolearn/trionginx

                
                '''
                
            }
        }
    }
}
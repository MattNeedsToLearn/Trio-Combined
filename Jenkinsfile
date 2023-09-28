pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''docker build -t mattneedstolearn/triodb ./db
                docker build -t mattneedstolearn/trioflask ./flask-app
                docker build -t mattneedstolearn/trionginx ./nginx'''
            }
        }
        stage('Push') {
            steps {
                sh '''docker push mattneedstolearn/triodb
                docker push mattneedstolearn/trioflask
                docker push mattneedstolearn/trionginx'''
            }
        }
        //stage('Deploy') {
        //    steps {
                //
          //  }
       // }
    }
}
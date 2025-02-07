pipeline {
    agent any
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('hadadeluna')
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t hadadeluna/tpjenkins-docker:latest .'
            }
        }
        
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push') {
            steps {
                sh 'docker push hadadeluna/tpjenkins-docker:latest'
            }
        }
    }
    
    post {
        always {
            script {
                sh 'docker logout'
            }
        }
    }
}


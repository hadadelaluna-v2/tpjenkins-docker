pipeline {
    agent any
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('hadadelaluna-v2')
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t hadadelaluna-v2/tpjenkins-docker:latest .'
            }
        }
        
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push') {
            steps {
                sh 'docker push hadadelaluna-v2/tpjenkins-docker:latest'
            }
        }
    }
    
    post {
        always {
            sh 'docker logout'
        }
    }
}


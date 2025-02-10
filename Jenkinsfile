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
                node('any') {
                    sh 'docker build -t hadadeluna/tpjenkins-docker:latest .'
                }
            }
        }

        stage('Login') {
            steps {
                node('any') {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }
        }

        stage('Push') {
            steps {
                node('any') {
                    sh 'docker push hadadeluna/tpjenkins-docker:latest'
                }
            }
        }
    }

    post {
        always {
            node('any') {
                script {
                    try {
                        sh 'docker logout'
                    } catch (Exception e) {
                        echo "Erreur lors de la déconnexion Docker : ${e.getMessage()}"
                    }
                }
            }
        }
        success {
            echo 'Pipeline réussi !'
        }
        failure {
            echo 'Pipeline échoué !'
        }
    }
}


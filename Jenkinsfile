pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo '=== BUILD ==='
                sh 'cat app.txt'
            }
        }

        stage('Test') {
            steps {
                echo '=== TEST ==='
                sh 'test -f app.txt'
                echo 'Tests passed!'
            }
        }

        stage('Docker Build') {
            steps {
                echo '=== DOCKER BUILD ==='

                sh '''
                    docker build -t jenkins-demo:${BUILD_NUMBER} .
                    docker images jenkins-demo
                '''
            }
        }
    }

    post {
        success {
            echo 'CI + Docker build completed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}

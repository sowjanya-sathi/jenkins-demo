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
	stage('Docker Run & Test') {
   	 steps {
       		 echo '=== DOCKER RUN & TEST ==='

      		  sh '''
            	docker rm -f jenkins-demo-test || true
		
           	 docker run -d \
                	--name jenkins-demo-test \
			--network jenkins-network \
              	 	 -p 8081:80 \
               		 jenkins-demo:${BUILD_NUMBER}

        	    sleep 3

         	   curl -f http://jenkins-demo-test
		   curl -f http://localhost:8081

            	docker logs jenkins-demo-test

            	docker rm -f jenkins-demo-test
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

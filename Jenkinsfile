pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'pwd'
            }
        }

        stage('Build Image') {
             steps {
                 sh '''
                    cd Cat && docker build -t geekcu/cat:JR10 .
                    cd ../helloworld && docker build -t geekcu/helloworld:JR10 .
                    cd ../web-nginx && docker build -t geekcu/mynginx:JR10 .
                    docker tag geekcu/cat:JR10 geekcu/cat:latest
                    docker tag geekcu/helloworld:JR10 geekcu/helloworld:latest
                    docker tag geekcu/mynginx:JR10 geekcu/mynginx:latest
                    docker images
	 	        '''
             }
         }

         stage('Push Image') {
             steps {
                 withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PSWD', usernameVariable: 'USER')]) {
                     sh """                        
                         docker login -u $USER -p $PSWD
                         for img in geekcu/cat:JR10 geekcu/cat:latest geekcu/helloworld:JR10 geekcu/helloworld:latest \
                                geekcu/mynginx:JR10 geekcu/mynginx:latest; \
                         do docker push \$img; done;
                     """
                 }

             }
         }

       //  stage('Deploy Service') {
       //      steps {
       //          sh '''
       //              docker-compose down
       //              docker-compose up -d
       //              docker-compose ps
       //          '''
       //      }
       //  }

    }
}

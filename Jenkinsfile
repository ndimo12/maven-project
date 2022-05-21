pipeline {
    agent any

    stages {

           stage('init') {
      checkout scm
   }
   stage('build') {
      sh '''
         mvn clean package
         cd target
         cp ../src/main/resources/web.config web.config
         cp todo-app-java-on-azure-1.0-SNAPSHOT.jar app.jar 
         zip todo.zip app.jar web.config
      '''
   }
        stage('clean') {

            agent {
        docker { image 'node:16.13.1-alpine' }
    }
            steps {
               sh '''
                mvn clean

                '''
            }
        }
    

    
        stage('compile') {

            agent {
        docker { image 'node:16.13.1-alpine' }
    }
            steps {
               sh '''
                mvn compile
                '''
            }
        }
    


    
        stage('test') {

            agent {
        docker { image 'node:16.13.1-alpine' }
    }
            steps {
                sh'''
                mvn test
                '''
            }
        }

         stage('packege') {

            agent {
        docker { image 'node:16.13.1-alpine' }
    }
            steps {
                sh'''
                mvn packege
                '''
            }
        }


        stage('verify') {

            agent {
        docker { image 'node:16.13.1-alpine' }
    }
            steps {
                sh'''
                mvn verify
                '''
            }
        }

       stage('install') {

            agent {
        docker { image 'node:16.13.1-alpine' }
    }
            steps {
                sh'''
                mvn install
                '''
            }
        }
        
}


}

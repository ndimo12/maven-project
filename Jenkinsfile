pipeline {
    agent any

    stages {

       stage('Setup parameters') {
            steps {
                script { 
                    properties([
                        parameters([
                          

                            string(
                                defaultValue: '001', 
                                name: 'Image-TAG', 
                                trim: true
                            )
                        ])
                    ])
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

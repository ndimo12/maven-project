pipeline {
    agent any

    stages {

        stage('clean') {
            agent {
                 docker { image 'maven:3.8.5-openjdk-8-slim' }
             }

            steps {
               sh '''
               rm -rf webapp/target/webapp.war || true
              mvn clean
               '''
            }
        }

        stage('compile') {
            agent {
                 docker { image 'maven:3.8.5-openjdk-8-slim' }
             }

            steps {
               sh '''
             mvn compile
               '''
            }
        }

stage('validate') {
    agent {
                 docker { image 'maven:3.8.5-openjdk-8-slim' }
             }

            steps {
               sh '''
               mvn validate
               '''
            }
        }



stage('test') {
    agent {
                 docker { image 'maven:3.8.5-openjdk-8-slim' }
             }

            steps {
               sh '''
             mvn test 
               '''
            }
        }

stage('package') {
    agent {
                 docker { image 'maven:3.8.5-openjdk-8-slim' }
             }

            steps {
               sh '''
            
              mvn package
              ls -l webapp/target
              pwd
               '''
            }
        }

stage('verify') {
    agent {
                 docker { image 'maven:3.8.5-openjdk-8-slim' }
             }

            steps {
               sh '''
              mvn verify
               '''
            }
        }


stage('install') {
    agent {
                 docker { image 'maven:3.8.5-openjdk-8-slim' }
             }

            steps {
               sh '''
           mvn install
               '''
            }
        }
       
stage('build ') {

            steps {
               sh '''
           docker build -t devopseasylearning2021:001 .
               '''
            }
        }
       


    }
}

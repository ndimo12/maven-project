pipeline {
    agent any
options { buildDiscarder(logRotator(artifactDaysToKeepStr: '',
     artifactNumToKeepStr: '',
      daysToKeepStr: '3',
      numToKeepStr: '5')),
      disableConcurrentBuilds() }
      
    stages {
        stage('Hello') {
            steps { 
                sh   '''
                   docker package 
               
                ls
                pwd
                top
                free
                
                '''
            }
        }
        
        stage('Hello-serge') {
            steps {
                sh   '''
                docker package
                ls
                pwd
                top
                mkdir guy
                free
               '''
            }
        }
        
        
        stage('Hello1') {
            steps {
                   sh  '''
        
                ls
                pwd
                top
                mkdir guy
                touch test
              '''  
            }
        }
    }
}

pipeline {
    agent { 
        label 'jenkins-node'
         }
   
   environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}

    options { buildDiscarder(logRotator(artifactDaysToKeepStr: '',
     artifactNumToKeepStr: '', daysToKeepStr: '3', numToKeepStr: '5'))
      disableConcurrentBuilds() }
      

    stages {
        
       stage('Setup parameters') {
            steps {
                script { 
                    properties([
                        parameters([
                          

                            string(
                                defaultValue: '001', 
                                name: 'ImageTAG', 
                                trim: true
                            )
                        ])
                    ])
                }
            }
        }




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


            steps {
               sh '''
         mvn package
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
       
       stage('SonarQube analysis') {
            agent {
                docker {

                  image 'sonarsource/sonar-scanner-cli:4.7.0'
                }
               }
               environment {
        CI = 'true'
        scannerHome='/opt/sonar-scanner'
    }
            steps{
                withSonarQubeEnv('Sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }



stage('build ') {

            steps {
               sh '''
           docker build -t devopseasylearning2021/eric:$ImageTAG .
               '''
            }
        }
       





      stage('Docker Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

           
            

     stage('Docker push ') {
            steps {
               sh '''
              docker push devopseasylearning2021/eric:$ImageTAG 
                '''
            }
        }

    


    stage('Deploy with docker ') {
            steps {
               sh '''
               docker run -d -p 8089:8080 devopseasylearning2021/eric:$ImageTAG 
                '''
            }
        }



    }
}

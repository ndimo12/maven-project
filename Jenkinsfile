
   
pipeline {
    agent any

 options { buildDiscarder(logRotator(artifactDaysToKeepStr: '',
     artifactNumToKeepStr: '', daysToKeepStr: '3', numToKeepStr: '5'))
      disableConcurrentBuilds() }



    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
    stages {
        
        stage('Setup parameters') {
            steps {
                script { 
                    properties([
                        parameters([
                            choice(
                                choices: ['No', 'Yes'], 
                                name: 'Clean'
                            ),

                            choice(
                                choices: ['Docker', 'Docker-compose', 'K8S'], 
                                name: 'environment'
                            ),

                            choice(
                                choices: ['8081', '8082', '8083', '8051', '8053', '7082', '6083', '5082', '4083'], 
                                name: 'Port'
                            ),
                       
                            string(
                                defaultValue: '1.0.0', 
                                name: 'ImageTag', 
                                trim: true
                            )
                        ])
                    ])
                }
            }
        }


        stage('clean') {
            when {
                 expression { env.Clean== "Yes" }
            }
            agent {
                docker {
                    image 'maven:3.8.5-openjdk-18'
                }
            }
            steps {
                sh '''
                 mvn clean 
                '''
            }
        }


        stage('validate') {
            agent {
                docker {
                    image 'maven:3.8.5-openjdk-18'
                }
            }
            steps {
                sh '''
                mvn validate
                '''
            }
        }



         stage('compile') {
             agent {
                docker {
                    image 'maven:3.8.5-openjdk-18'
                }
            }
            steps {
                sh '''
                mvn  compile 
                '''
            }
        }
 

        stage('test') {
            agent {
                docker {
                    image 'maven:3.8.5-openjdk-18'
                }
            }
            steps {
                sh '''
                mvn test
                '''
            }
        }



        stage('package') {
            agent {
                docker {
                    image 'maven:3.8.5-openjdk-11'
                }
            }
            steps {
                sh '''
                mvn package
                '''
            }
        }

       
        stage('verify') {
            agent {
                docker {
                    image 'maven:3.6.0-jdk-11-slim'
                }
            }
            steps {
                sh '''
                 mvn verify
                '''
            }
        }

 
        stage('install') {
            agent {
                docker {
                    image 'maven:3.6.0-jdk-11-slim'
                }
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



        stage('build images') {
 
            steps {
                sh '''
                 docker build -t devopseasylearning2021/eric:$ImageTag -f apache.Dockerfile .
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
              docker push devopseasylearning2021/eric:$ImageTag
                '''
            }
        }



        stage('deploy with docker') {
            when {
                 expression { env.environment== "Docker" }
            }
 
            steps {
                sh '''
                docker rm -f odilia || true
                 docker run -d --name odilia -p $Port:80 devopseasylearning2021/eric:$ImageTag 
                 docker ps -a 
                '''
            }
        }

 
        stage('deploy with compose') {
            when {
                 expression { env.environment== "Docker-compose" }
            }
 
            steps {
                sh '''
docker-compose down 
docker rm -f odilia || true
rm -rf docker-compose.yml || true
cat <<EOF > docker-compose.yml
version: "3"
services:
    tomcat:
       image: devopseasylearning2021/eric:$ImageTag 
       container_name: odilia
       expose:
        - 80
       ports:
         - $Port:80
EOF

cat docker-compose.yml
docker compose up -d
docker-compose ps 
'''
            }
        }

        
         stage('deploy with kubernetes') {
             when {
                 expression { env.environment== "K8S" }
            }
 
            steps {
                sh '''
                ls
                '''
            }
        }



    }

  post {
   
   success {
      slackSend (channel: '#development-alerts', color: 'good', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }

 
    unstable {
      slackSend (channel: '#development-alerts', color: 'warning', message: "UNSTABLE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }

    failure {
      slackSend (channel: '#development-alerts', color: '#FF0000', message: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }
   
    cleanup {
      deleteDir()
    }
}




}

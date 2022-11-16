pipeline {
    agent any
    environment{
        PATH = "$PATH:/usr/share/maven/bin"
    }

    stages {
         stage('Cloning from GitHub') {
                steps {
                    git branch: 'main', url: 'https://github.com/wajih942/tpAchatProject'
                }  
            }
              
          stage('MVN COMPILE') {
            steps {
               sh 'mvn compile'
           }
        }
          stage('mvn Test') {
            steps {
               sh 'mvn test'
            }
        }
          stage('mvn Verify') {
             steps {
               sh 'mvn verify'
          }
       }
         stage ('Scan Sonar'){
            steps {
    sh "mvn sonar:sonar \
  -Dsonar.projectKey=achat123 \
  -Dsonar.host.url=http://192.168.33.10:9000 \
  -Dsonar.login=d335b64650a44234a22bbf17558f7969937347a2"
    }
        }
        stage('Nexus') {
      steps {
        sh 'mvn deploy -DskipTests'
      }
    }
       
     stage("Building Docker Image") {
                steps{
                    sh 'docker build -t wajih1201/achat .'
                }
        }
        
        
           stage("Login to DockerHub") {
                steps{
                   // sh 'sudo chmod 666 /var/run/docker.sock'
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u wajih1201 -p wajihjimi2'
                }
        }
        stage("Push to DockerHub") {
                steps{
                    sh 'docker push wajih1201/achat'
                }
        }
    
               stage("Docker-compose") {
                steps{
                    sh 'docker-compose up -d'
                }
        }
    }
    
    post {
                success {
                   echo 'succes'
                }
failure {
                  echo 'failed'   
                }
             
       
    }

}

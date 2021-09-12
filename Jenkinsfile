pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                git 'https://github.com/austinobioma/Another-pipeline.git'
            }
        }
      stage('Build') {
            steps {
               sh ''' cd MywebApp && mvn clean package'''
            }
        }
      stage('Test') {
            steps {
                sh ' cd MywebApp && mvn test'
            }
        }
        stage ('Code Qualty Scan') {
            
           steps {
                  withSonarQubeEnv('sonar') {
             sh "mvn -f MywebApp/pom.xml sonar:sonar"
                      
               }
            }
       }
        stage('Quality gate') {
            
          steps {
                 waitForQualityGate abortPipeline: true
              }
          }
        stage('Artifactory configuration') {

           steps {

             rtServer (
         
              id: "my-jfrog",
     
               url: "http://3.83.236.155:8082/artifactory",

               credentialsId: "baba7fa5-a142-40b1-b368-ead653ab7bc6",
                bypassProxy: true,
                timeout: 300

            )
          }
        }
        stage('Artifact Upload') (

             steps {

                 rtUpload (

                     serverId: 'my-jfrog',
                       spec: ''' {
                               "files":[
                                    {
                                    "pattern": "MywebApp/target/MywebApp.war",
                                    "target": "jenkins-repo/MywebApp-files/"
                                    }
                                 ]
                        }''',
                 )
             }
        )
     }
 }

pipeline {
    agent any
    tools
    {
       maven "M3"
    }

    stages {
      stage('checkout') {
           {

                git branch: 'main', url: 'https://github.com/prakash189/hello-java.git'

          }
        }

         stage('build code') {
            {

                sh 'mvn clean install'
          }
        }


       stage ('Scan File') {
             {
               withSonarQubeEnv(installationName: 'sonarqubeserver', credentialsId: 'sonarqube') 
               {
                sh 'mvn clean package sonar:sonar -Dsonar.projectName="java-pipeline" -Dsonar.projectKey="java"'
                }
            }
        }


        stage("Check for Quality Gate")
               {
               
                       timeout(time: 1, unit: 'MINUTES') 
                            {
                                   def qg = waitForQualityGate()
                                    if (qg.status != 'OK') 
                                    { 
                                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                                    }
                            }
               
                  
               }

          //  stage('SQuality Gate') {
          //       steps {
          //           timeout(time: 5, unit: 'MINUTES') {
          //            waitForQualityGate abortPipeline: true
          //          }
          //       }
          //  }

        //      stage('upload the artifacts on nexus') {
        //           steps {

        //             nexusArtifactUploader artifacts: [
        //           [
        //             artifactId: 'LoginWebApp', 
        //             classifier: '', 
        //             file: "target/LoginWebApp-1.war", 
        //             type: 'war'
        //           ]

        //           ], 
        //             credentialsId: 'nexus-cred', 
        //             groupId: 'com.devops4solutions', nexusUrl: '18.139.170.236:8081', 
        //             nexusVersion: 'nexus3', 
        //             protocol: 'http', 
        //             repository: 'java-apps-artifacts/', 
        //             version: "1"
        //   }
        // }


        // stage('Ansible Deploy') {

        //     steps {
                
        //         sh "ansible-playbook main.yml -i inventories/prod/hosts --user ubuntu "
        
            
        //     }
        // }
    }
}


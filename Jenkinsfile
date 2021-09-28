pipeline {
    agent any

    tools
    {
       maven "M3"
    }

    stages {
      stage('checkout') {
           steps {

                git branch: 'main', url: 'https://github.com/prakash189/hello-java.git'

          }
        }

         stage('build code') {
           steps {

                sh 'mvn clean install'
          }
        }

         stage('upload the artifacts on nexus') {
           steps {

                nexusArtifactUploader artifacts: [
                  [
                    artifactId: 'LoginWebApp', 
                    classifier: '', 
                    file: "target/LoginWebApp-1.war", 
                    type: 'war'
                  ]

                  ], 
                    credentialsId: 'nexus-cred', 
                    groupId: 'com.devops4solutions', nexusUrl: '18.140.248.102:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'java-apps-artifacts/', 
                    version: "1"
          }
        }


        stage('Ansible Deploy') {

            steps {
                
                sh "ansible-playbook main.yml -i inventories/prod/hosts --user ubuntu "
        
            
            }
        }
    }
}


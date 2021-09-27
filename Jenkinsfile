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
             def mavenPom = readMavenPom 'pom.xml'

                nexusArtifactUploader artifacts: [
                  [
                    artifactId: 'LoginWebApp', 
                    classifier: '', 
                    file: "target/LoginWebApp-${mavenPom.version}.war", 
                    type: 'war'
                  ]

                  ], 
                    credentialsId: 'nexus-cred', 
                    groupId: 'com.devops4solutions', nexusUrl: '18.142.139.8', 
                    nexusVersion: 'nexus2', 
                    protocol: 'http', 
                    repository: 'http://18.142.139.8:8081/repository/java-apps-artifacts/', 
                    version: "${mavenPom.version}"
          }
        }


        stage('Ansible Deploy') {

            steps {
                
                sh "ansible-playbook main.yml -i inventories/prod/hosts --user ubuntu "
        
            
            }
        }
    }
}


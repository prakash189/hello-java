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
        
         stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        
        
         
        
        
        
        stage('Ansible Deploy') {
             
            steps {
                 
             
               
               sh "ansible-playbook main.yml -i inventories/prod/hosts --user ubuntu --key-file /home/ubuntu/.ssh/id_rsa"

               
            
            }
        }
    }
}

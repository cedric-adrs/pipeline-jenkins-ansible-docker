pipeline {
    agent any
    
    tools  {
        maven "my-maven"
    }

    stages {

        // Clone from Git
        stage("Clone App from Git"){
            steps{
                echo "====++++  Clone App from Git ++++===="
                git branch:"master", url: "https://github.com/mromdhani/pipeline-jenkins-ansible-docker.git"
            }          
        }
        // Build and Unit Test (Maven/JUnit)
        stage("Build and Package"){
            steps{
                echo "====++++  Build and Unit Test (Maven/JUnit) ++++===="
                sh "mvn -f greetings-app/pom.xml clean package"
            }           
        }  

        // Static Code Analysis (SonarQube)
        stage("Static Code Analysis (SonarQube)"){
            steps{
                echo "====++++  Static Code Analysis (SonarQube) ++++===="                
          //      withSonarQubeEnv('my_sonarqube_in_docker') {  
                sh "mvn -f greetings-app/pom.xml clean package clean package -Dsurefire.skip=true sonar:sonar -Dsonar.host.url=http://localhost:9000   -Dsonar.projectName=Pipeline-jenkins-ansible-docker -Dsonar.projectVersion=$BUILD_NUMBER";
             
           //     }  
            }           
        }


         // Deploiement du WAR sur le server-staging avec Ansible
        stage("Deploy WAR on staging using Ansible"){
            steps{
                
                echo "====++++  Deploy WAR on staging using Ansible ++++===="
      
                ansiblePlaybook(credentialsId: 'ssh-key-for-server-staging', 
                                  inventory:  "$WORKSPACE/config-as-code/ansible/hosts", 
                                  playbook: '$WORKSPACE/config-as-code/ansible/playbook-deploy-staging.yaml')          
            } 
        }
    }
}
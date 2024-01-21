pipeline{
    agent any
    tools{
        maven 'my maven home'
    }
    environment{
        NAME='pradeep'
    }
    stages{
        
        stage('Clean Up'){
            steps{
                deleteDir()
            }
        }
        stage('clone the repo'){
           
            steps{
                git 'https://github.com/Pradeep-0877/SpringContactApp.git'
            }
        }
        stage('Build our application'){
            steps{
                sh 'mvn clean install'
                echo "My name is ${NAME}"
            }
        }
        stage('Test'){
            steps{
                sh 'mvn test'
            }
        }
        post{
           success{
              archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
           }
        }
    }
}

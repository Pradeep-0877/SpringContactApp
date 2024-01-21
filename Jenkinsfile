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
                echo "Branch name is $BRANCH_NAME"
            }
        }
        stage('Test'){
            steps{
                sh 'mvn test'
            }
            post{
              success{
                archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
              }
            }
        }

        stage('Deploy to Tomcat'){
            when{
                equals expected: "master", actual: env.BRANCH_NAME
            }
            environment{
                TOMCAT_CREDS=credentials('my-tomcat-creds')
            }
            steps{
                
                sh "curl -v -u ${TOMCAT_CREDS_USR}:${TOMCAT_CREDS_PSW} -T /var/lib/jenkins/.m2/repository/in/ezeon/SpringContactApp/1.0-SNAPSHOT/SpringContactApp-1.0-SNAPSHOT.war http://34.125.51.114:8080/manager/text/deploy?path=/spring-contact-app"
            }
        }
        
    }
}

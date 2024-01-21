pipeline{
    agent any
    tools{
        maven 'my maven home'
    }
    environment{
        NAME='pradeep'
        
    }
    parameters{
        booleanParam(defaultValue: true, description: "Do you want sonar code analysys", name: "sonar") 
        choice(choices: ["TEST","PROD","QA"], description: "In which environment you want to Deploy to",name: "DEPLOY_TO")


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
        stage("perforn sonar"){
            when{
                equals expected: "true", actual: "${params.sonar}"
            }
            steps{
                sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=pradeep-0877_spring-contact-app'
            }
        }
        stage('Build our application'){
            
            steps{
                sh 'mvn clean install'
                echo "My name is ${NAME}"
                echo "Branch name is $env.BRANCH_NAME"
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
                equals expected: "PROD", actual: "${params.DEPLOY_TO}"
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

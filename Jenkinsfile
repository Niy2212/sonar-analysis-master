pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'jdk17'
        maven 'maven3'
        }
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/Niy2212/sonar-analysis-master'
                }
        }

      
        stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }
       
        stage("SonarQube Analysis"){
            steps {
                script {
                    withSonarQubeEnv(credentialsId: "Sonarqube_Token_Key"){
                    sh "mvn sonar:sonar"
                    }
                }
            }
        } 
         stage("Quality Gate"){
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: "Sonarqube_Token_Key"
                    }
                }
            }
        
        }
    }    

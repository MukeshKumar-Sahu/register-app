pipeline {
    
    agent any

    tools {

        maven 'maven3.9.7'
        jdk 'Java17'
    }

    stages{

        stage('Cleanup workspace'){
            steps {
                cleanWs()
            }
        }

        stage('check out'){
            steps {
              git branch: 'main' , credentialsId: 'Github' , url: 'https://github.com/MukeshKumar-Sahu/register-app'
            }
        }

        stage('Biuld'){

            steps{

                sh "mvn clean package"
            }
        }

        stage('test'){

            steps{

                sh "mvn test"
            }
        
 
        }

        stage('Static Code Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn clean verify sonar:sonar'
                    }
                }
            }
        }
    

        stage('static code analysis'){

            steps{
                script {

                    withSonarQubeEnv(credentialsId: 'Jenkins-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }

        

    }
}

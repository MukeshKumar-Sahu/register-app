pipeline {
    
    agent any

    tools {
        maven 'maven3.9.7'
        jdk 'Java17'
    }

    environment {
        DOCKER_USER = 'mukeshkumarsahu'
        DOCKER_IMAGE = 'MyFirstAPP'
       // DOCKER_CREDENTIALS_ID = 'Docker-Jenkins'
        DOCKER_PASSWORD = 'Khemchand@123'
    }

    stages {
        stage('Cleanup workspace') {
            steps {
               cleanWs()
          }
        }

        stage('Check out') {
            steps {
                git branch: 'main', credentialsId: 'Github', url: 'https://github.com/MukeshKumar-Sahu/register-app'
            }
        }

       
        stage('Build') {
            steps {
                bat "mvn install"
            }
        }

        stage('Test') {
           steps {
                bat "mvn test"
            }
        }

        stage('Static Code Analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'Sonartoken') {
                        bat "mvn sonar:sonar"
                    }
                }
            }
        }


        stage('Build and Push') {
            agent {
                docker {
                    image 'docker:20.10'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                bat 'docker build -t my-image .'
                bat 'docker push my-image'
            }
        }
    }
}

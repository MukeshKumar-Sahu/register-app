pipeline {
    
    agent any

    tools {

        maven 'maven3.9.7'
        jdk 'Java17'
    }

    environment {
        DOCKER_USER = 'mukeshkumarsahu'
        DOCKER_IMAGE = 'My First APP'
        DOCKER_CREDENTIALS_ID = 'DockerID'
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

        stage('Build'){

            steps{

                sh "mvn clean package"
            }
        }

        stage('test'){

            steps{

                sh "mvn test"
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

        stage('Build Docker Image') {
            steps {
                script {
                    DOCKER_IMAGE = docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        DOCKER_IMAGE.push()
                        DOCKER_IMAGE.push('latest')
                    }
                }
            }
        }

        

    }
}

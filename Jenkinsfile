pipeline {
    
    agent any

    tools {
        maven 'maven3.9.7'
        jdk 'Java17'
    }

    environment {
        DOCKER_USER = 'mukeshkumarsahu'
        DOCKER_IMAGE = 'MyFirstAPP'
        DOCKER_CREDENTIALS_ID = 'Docker-Jenkins'
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
                sh "mvn clean package"
            }
        }

        stage('Test') {
            steps {
                sh "mvn test"
            }
        }

        stage('Static Code Analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'Jenkins-token2') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }

        stage('Build Docker Image and Push Docker Image') {
            steps {
                script {
                    docker.withDockerRegistry([credentialsId:'Docker-Jenkins', url: 'https://registry.hub.docker.com']) {
                        def dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}

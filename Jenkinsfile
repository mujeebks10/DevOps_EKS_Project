pipeline {
    agent {label 'Jenkins-Agent'}
    tools {
        jdk 'java17'
        maven 'Maven3'
    }
     environment {
	        APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "mujeebks10"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
            cleanWs()
            }
        }

        stage ("Checkout from SCM"){
            steps {
                git branch: 'master', credentialsId: 'github', url: 'https://github.com/mujeebks10/DevOps_EKS_Project'
            }
        }

        stage ("Build Application"){
            steps {
                sh "mvn clean package"
            }
        }

        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }

        stage("Sonarqube-Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-tocken') {
                    sh "mvn sonar:sonar"
                    }
                }
            }
        }

        stage ("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-tocken'
                }
            }
        }

        stage("Docker Build & Push Docker image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push(latest)
                    }                
                }
            }
        }
    }
    
}

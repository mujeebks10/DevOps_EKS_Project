pipeline {
    agent {label 'Jenkins-Agent'}
    tools {
        jdk 'java17'
        maven 'Maven3'
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                CleanWs()
            }
        }
        stage{
            steps("Checkout from SCM") {
                git credentialsId: 'github', url: 'https://github.com/mujeebks10/DevOps_EKS_Project.git'
            }
        }
    }
}

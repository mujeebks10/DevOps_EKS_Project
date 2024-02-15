pipeline {
    agent {label 'Jenkins-Agent'}
    tools {
        jdk 'java17'
        maven 'Maven3'
    }

    stages {
        stage ("Cleanup Workspace") {
            steps {
                CleanWs()
            }
        }

        stage ("Checkout from SCM"){
            steps {
                git branch: 'Feat-03-dev3', credentialsId: 'github', url: 'https://github.com/mujeebks10/DevOps_EKS_Project'
            }

        }
        stage ("Maven Build") {
            steps {
                sh ' mvn clean package'
            }
        }

        stage ("Test Application") {
            steps {
                sh 'mvn test'
            }
        }

    }

}
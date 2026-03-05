pipeline {
    agent any

    stages {

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'trivy fs --severity HIGH,CRITICAL --exit-code 1 .'
            }
        }
        
        stage('Package') {
            steps {
                sh 'mvn package'
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }

        stage('Deploy') {
            when { branch 'main' }
            steps {
                sh 'scp target/*.jar ubuntu@10.2.2.141:/home/ubuntu/app.jar'
            }
        }
    }
}

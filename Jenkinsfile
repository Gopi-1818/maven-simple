pipeline {
    agent any

    environment {
        SERVER = "10.2.2.141"
        USER = "ubuntu"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'git@github.com:Gopi-1818/maven-simple.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'trivy fs --exit-code 1 --severity HIGH,CRITICAL .'
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

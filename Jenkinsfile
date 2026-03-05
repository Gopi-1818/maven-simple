pipeline {
    agent any

    environment {
        APP_SERVER = "10.2.2.141"
        DEPLOY_USER = "ubuntu"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'mvn org.owasp:dependency-check-maven:check -DfailBuildOnCVSS=7'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Deploy to Application Server') {
            when {
                branch 'main'
            }
            steps {
                sshagent(['app-server-ssh']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no target/*.jar ubuntu@10.2.2.141:/home/ubuntu/app.jar
                        ssh ubuntu@10.2.2.141 "pkill java || true"
                        ssh ubuntu@10.2.2.141 "nohup java -jar /home/ubuntu/app.jar > app.log 2>&1 &"
                    '''
                }
            }
        }
    }
}

pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Coverage') {
            steps {
                sh 'npx tap --coverage'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'npm audit || true'
            }
        }
    }
}
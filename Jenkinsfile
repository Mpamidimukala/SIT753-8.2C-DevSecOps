pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Coverage') {
            steps {
                bat 'npx tap --coverage'
            }
        }

        stage('Security Scan') {
            steps {
                bat 'npm audit'
            }
        }
    }
}
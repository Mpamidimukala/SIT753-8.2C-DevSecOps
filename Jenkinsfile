pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }

    tools {
        nodejs 'NodeJS'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Mpamidimukala/SIT753-8.2C-DevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    def testStatus = sh(
                        script: 'npm test',
                        returnStatus: true
                    )

                    env.TEST_STATUS = testStatus == 0 ? 'SUCCESS' : 'FAILURE'
                }
            }

            post {
                always {
                    emailext(
                        subject: "Jenkins - Run Tests - ${env.TEST_STATUS}",
                        body: """Run Tests stage completed.

Status: ${env.TEST_STATUS}
Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}
""",
                        to: 'monishpamidimukala@gmail.com',
                        attachLog: true,
                        compressLog: fasle
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                script {
                    def auditStatus = sh(
                        script: 'npm audit',
                        returnStatus: true
                    )

                    env.AUDIT_STATUS = auditStatus == 0 ? 'SUCCESS' : 'FAILURE'
                }
            }

            post {
                always {
                    emailext(
                        subject: "Jenkins - Security Scan - ${env.AUDIT_STATUS}",
                        body: """NPM Audit Security Scan completed.

Status: ${env.AUDIT_STATUS}
Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}
""",
                        to: 'monishpamidimukala@gmail.com',
                        attachLog: true,
                        compressLog: false
                    )
                }
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                sh '''
                    curl -L -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-7.2.0.5079-linux-x64.zip
                    rm -rf sonar-scanner-7.2.0.5079-linux-x64
                    unzip -oq sonar-scanner.zip
                    ./sonar-scanner-7.2.0.5079-linux-x64/bin/sonar-scanner
                '''
            }
        }
    }
}

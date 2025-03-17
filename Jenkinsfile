pipeline {
    agent any

    environment {
        NODEJS_VERSION = '22.13.0'  // Change to your required Node.js version
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    echo 'Cloning repository...'
                }
                checkout scm
            }
        }

        stage('Set Up Node.js') {
            steps {
                script {
                    def nodeHome = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                    env.PATH = "${nodeHome}/bin:${env.PATH}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                }
                sh 'npm ci'  // Clean install
            }
        }

        stage('Run Mocha Tests') {
            steps {
                script {
                    echo 'Running Mocha tests...'
                }
                sh 'npx mocha --timeout 10000 "GroceryApi/groceryStore/groceryApi.js" --reporter mocha-junit-reporter'
            }
        }

        stage('Archive Test Reports') {
            steps {
                script {
                    echo 'Archiving test reports...'
                }
                junit '**/test-results.xml'  // Adjust this path if needed
            }
        }
    }

    post {
        always {
            script {
                echo 'Cleaning up workspace...'
            }
            cleanWs()
        }
        success {
            script {
                echo 'Build and tests passed successfully!'
            }
        }
        failure {
            script {
                echo 'Build or tests failed '
            }
        }
    }
}

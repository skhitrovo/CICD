pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        environment {
            NODEJS_HOME = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        }

        stage('Build') {
            steps {
                script {
                    def nodeHome = tool 'NodeJS'
                    env.PATH = "${nodeHome}/bin:${env.PATH}"
                    sh 'node --version'
                }
            }
        }

        stage('Build and Test') {
            steps {
                sh 'chmod +x scripts/build.sh'
                sh './scripts/build.sh'
                
                sh 'chmod +x scripts/test.sh'
                sh './scripts/test.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:${BUILD_ID} .'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker run -d -p 3000:3000 myapp:${BUILD_ID}'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'docker run -d -p 3001:3000 myapp:${BUILD_ID}'
                    }
                }
            }
        }
    }
}

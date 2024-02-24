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

    tools {
        // Указываем инструмент установки Node.js
        nodejs 'NodeJs'
    }

    stages {
        stage('Declarative Tool Install') {
            steps {
                // Убедитесь, что инструмент Node.js используется как nodejs, а не NodeJs
                sh 'node --version'
            }
        }
    }
}

        stage('Build') {
            steps {
                sh './scripts/build.sh'
            }
        }

        stage('Test') {
            steps {
                sh './scripts/test.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:${BUILD_ID} .'
            }
        }

        stage('Scan Docker Image for Vulnerabilities') {
            steps {
                script {
                    def vulnerabilities = sh(script: "trivy image --exit-code 0 --severity HIGH,MEDIUM,LOW --no-progress myapp:${BUILD_ID}", returnStdout: true).trim()
                    echo "Vulnerability Report:\n${vulnerabilities}"
                }
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

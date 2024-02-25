pipeline {
    agent any
    tools {
        nodejs 'nodejs'
    }
    stages {
        stage("build") {
            when {
                branch 'main'
            }
            steps {
                echo 'app build'
                sh 'npm install'
                echo 'build end'
            }
        }
        stage("test") {
            when {
                branch 'dev'
            }
            steps {
                echo 'app test'
                sh 'npm test'
                echo 'test end'
            }
        }
        
        stage("docker build") {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh "echo docker build main"
                        sh "docker build -t nodemain:${BUILD_NUMBER} ."
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh "echo docker build dev"
                        sh "docker build -t nodedev:${BUILD_NUMBER} ."
                    }
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh "docker ps"
                        sh "echo docker run main nodemain_cicd"
                        sh "docker stop nodemain_cicd || true"
                        sh "docker rm nodemain_cicd || true"
                        sh "docker run --name nodemain_cicd -d --expose 3000 -p 3000:3000 nodemain:${BUILD_NUMBER}"
                        sh "docker ps"
                        sh "docker rmi -f nodemain:${BUILD_NUMBER} || true"
                        sh "echo docker rmi main"
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh "echo docker run dev nodemain_cicd"
                        sh "docker ps"
                        sh "docker stop nodedev_cicd || true"
                        sh "docker rm nodedev_cicd || true"
                        sh "docker run --name nodedev_cicd -d --expose 3001 -p 3001:3000 nodedev:${BUILD_NUMBER}"
                        sh "docker ps"
                        sh "docker rmi -f nodemain:${BUILD_NUMBER} || true"
                        sh "echo docker rmi dev"
                    }
                }
            }
        }
    }
}

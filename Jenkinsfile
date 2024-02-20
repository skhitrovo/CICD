pipeline {
    agent any
    
    stages {
        stage('Deploy') {
            steps {
                input 'Deploy to production?' // Manual trigger for deployment
                sh 'docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)'
                sh 'docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0'
            }
        }
    }
}

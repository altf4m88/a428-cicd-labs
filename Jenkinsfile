pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') { 
            steps {
                sh 'set -x'
                sh 'npm test' 
            }
        }
        stage('Deploy') {
            steps {
                sh 'set -x'
                sh 'npm run build'
                sh 'set +x'

                sh 'set -x'
                sh 'npm start &'
                sh 'sleep 1'
                sh 'echo $! > .pidfile'
                sh 'set +x'
                
                sh 'sleep 60'
                
                sh 'set -x'
                sh 'cat .pidfile'
                sh 'kill'
            }
        }
    }
}

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
        stage('Manual Approval') { 
            steps {
                input message: 'Lanjutkan ke tahap Deploy?' 
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    set -x
                    npm run build
                    set +x

                    set -x
                    npm start &
                    sleep 1
                    echo $! > .pidfile
                    set +x
                    
                    sleep 60
                    
                    set -x
                    PID=$(cat .pidfile | tr -d '[:space:]')
                    kill $PID
                '''
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
              sh '''
                docker build -t abdulwahabshukri/ttd-backend:jenkins-${GITHUB_RUN_ID} .
              '''
            }
        }
        stage('Release') {
            steps {
              withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh '''
                docker login -u $USERNAME -p $PASSWORD
                docker push abdulwahabshukri/ttd-backend:jenkins-${GITHUB_RUN_ID}
                '''
              }
            }
        }
        stage('deploy') {
            steps {
                sh '''
                docker stop ttd-backend || true
                docker rm -f ttd-backend || true
                docker run -p5000:3000 -v /home/deploy/data.csv:/app/data.csv -d --name ttd-backend abdulwahabshukri/ttd-backend:jenkins-${GITHUB_RUN_ID}
                '''
            }
        }
    }
}

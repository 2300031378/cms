pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'github-cred'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'master',
                    credentialsId: "${GIT_CREDENTIALS}",
                    url: 'https://github.com/2300031378/cms.git'
            }
        }

        stage('Backend Build') {
            steps {
                sh 'cd backend && mvn -q -DskipTests clean package'
            }
        }

        stage('Frontend Build') {
            steps {
                sh '''
                cd storyspark-content-hub-main
                npm install
                npm run build
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose down && docker compose up -d'
            }
        }
    }

    post {
        always {
            echo "Deployment Finished."
        }
    }
}

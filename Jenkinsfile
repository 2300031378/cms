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

        stage('Clean Old Docker Containers') {
            steps {
                bat '''
                echo Stopping and removing old containers...
                docker stop acme_mysql acme_backend acme_frontend 2>NUL || true
                docker rm acme_mysql acme_backend acme_frontend 2>NUL || true
                '''
            }
        }

        stage('Backend Build') {
            steps {
                bat '''
                cd backend
                mvn -q -DskipTests clean package
                '''
            }
        }

        stage('Frontend Build') {
            steps {
                bat '''
                cd storyspark-content-hub-main
                npm install
                npm run build
                '''
            }
        }

        stage('Docker Build') {
            steps {
                bat '''
                docker compose build --no-cache
                '''
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                echo Bringing application UP...
                docker compose down --remove-orphans
                docker compose up -d
                '''
            }
        }
    }

    post {
        always {
            echo "Deployment Finished."
        }
    }
}



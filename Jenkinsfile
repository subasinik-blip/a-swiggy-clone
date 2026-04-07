pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')
        SONAR_ORG = 'subasinik-blip'
        SONAR_PROJECT_KEY = 'subasinik-blip_a-swiggy-clone'
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build || echo "No build script found"'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                sh '''
                docker run --rm \
                  -e SONAR_HOST_URL=https://sonarcloud.io \
                  -e SONAR_LOGIN=$SONAR_TOKEN \
                  sonarsource/sonar-scanner-cli \
                  -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                  -Dsonar.organization=$SONAR_ORG \
                  -Dsonar.sources=.
                '''
            }
        }

        stage('TRIVY FS SCAN') {
            steps {
                sh 'trivy fs . > trivyfs.txt'
            }
        }

        stage('Deploy') {
            steps {
                echo "Add your deploy steps here"
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESS ✅"
        }
        failure {
            echo "Pipeline FAILED ❌"
        }
    }
}

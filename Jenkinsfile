pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

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

        stage('Checkout Code') {
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
                sh 'npm run build || echo "No build step"'
            }
        }

        stage('SonarCloud Scan') {
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

        stage('Trivy Scan') {
            steps {
                sh 'trivy fs . > trivy-report.txt || true'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deployment step placeholder"
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

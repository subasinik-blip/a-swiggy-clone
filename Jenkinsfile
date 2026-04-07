pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')             // Jenkins credential you created
        SONAR_ORG = 'subasinik-blip'                         // Your SonarCloud org
        SONAR_PROJECT_KEY = 'subasinik-blip_a-swiggy-clone'  // Your project key
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/subasinik-blip/a-swiggy-clone.git'
            }
        }

        stage('Build & Test') {
            steps {
                echo "Building and testing project..."
                // Adjust build command if needed (Gradle/Maven/etc)
                sh './gradlew build test || mvn clean install'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                echo "Running SonarCloud analysis..."
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

        stage('Deploy') {
            steps {
                echo "Deploying app..."
                sh './deploy.sh || echo "Replace deploy.sh with your deploy commands"'
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

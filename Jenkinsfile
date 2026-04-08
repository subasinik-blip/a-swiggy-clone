pipeline {
agent any

```
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
            sh 'CI=false npm run build'
        }
    }

    stage('SonarCloud Scan') {
        steps {
            echo "Skipping SonarCloud for now"
        }
    }

    stage('Trivy Scan') {
        steps {
            sh 'trivy fs . > trivy-report.txt || true'
        }
    }

    stage('Docker Build & Push') {
        steps {
            script {
                withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker') {

                    sh 'docker build -t swiggy-clone .'

                    sh 'docker tag swiggy-clone YOUR_DOCKERHUB_USERNAME/swiggy-clone:latest'

                    sh 'docker push YOUR_DOCKERHUB_USERNAME/swiggy-clone:latest'
                }
            }
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
```

}

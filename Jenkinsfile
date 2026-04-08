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
    DOCKER_IMAGE = 'subasinik-blip/swiggy-clone'
}

stages {

    stage('Clean Workspace') {
        steps {
            cleanWs()
        }
    }

    stage('Checkout Code') {
        steps {
            git branch: 'main', url: 'https://github.com/subasinik-blip/a-swiggy-clone.git'
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

    stage('Trivy FS Scan') {
        steps {
            sh 'trivy fs . > trivyfs.txt || true'
        }
    }

    stage('Docker Build & Push') {
        steps {
            script {
                withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker') {
                    sh 'docker build -t swiggy-clone .'
                    sh 'docker tag swiggy-clone subasinik-blip/swiggy-clone:latest'
                    sh 'docker push subasinik-blip/swiggy-clone:latest'
                }
            }
        }
    }

    stage('Trivy Image Scan') {
        steps {
            sh 'trivy image subasinik-blip/swiggy-clone:latest > trivyimage.txt || true'
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

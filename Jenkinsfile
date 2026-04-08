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

    stage('Trivy Scan') {
        steps {
            sh 'trivy fs . > trivy.txt || true'
        }
    }

    stage('Docker Build') {
        steps {
            sh 'docker build -t swiggy-clone .'
        }
    }

    stage('Deploy') {
        steps {
            sh '''
            docker stop swiggy-container || true
            docker rm swiggy-container || true
            docker run -d -p 3000:3000 --name swiggy-container swiggy-clone
            '''
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

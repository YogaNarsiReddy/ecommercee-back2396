pipeline {
    agent any

    tools {
        maven "Maven_3.8"   // Configure in Jenkins -> Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/YogaNarsiReddy/ecommerce-backend.git'
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean install -DskipTests'
            }
        }

        stage('Archive Jar') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Backend build completed successfully!'
        }
        failure {
            echo '❌ Backend build failed!'
        }
    }
}

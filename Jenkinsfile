pipeline {
    agent any

    tools {
        maven "Maven"     // Configure in Jenkins -> Global Tool Configuration
        jdk "jdk"          // Configure in Jenkins -> Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YogaNarsiReddy/ecommerce-backend.git',
                    credentialsId: 'github-creds'   // Use GitHub PAT credential you added in Jenkins
            }
        }

        stage('Build') {
            steps {
                bat "mvn clean package -DskipTests"   // Windows-friendly
            }
        }

        stage('Test') {
            steps {
                bat "mvn test"
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}

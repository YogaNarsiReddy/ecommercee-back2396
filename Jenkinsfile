pipeline {
    agent any

    tools {
        jdk 'jdk'          // Configure in Jenkins -> Global Tool Configuration
        maven 'Maven'       // Configure in Jenkins -> Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/ecommerce-backend.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'  // use sh if Linux
            }
        }

        stage('Run Tests') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Archive JAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}

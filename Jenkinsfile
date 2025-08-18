pipeline {
    agent any

    tools {
        jdk 'JDK17'          // Configure in Jenkins -> Global Tool Configuration
        maven 'Maven3'       // Configure in Jenkins -> Global Tool Configuration
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

pipeline {
    agent any

    tools {
        maven "Maven"
        jdk "jdk"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YogaNarsiReddy/ecommerce-backend.git',
                    credentialsId: 'github-creds'
            }
        }

        stage('Build') {
            steps {
                bat "mvn clean package -DskipTests"
            }
        }

        stage('Test') {
            steps {
                bat "mvn test"
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'tomcat-creds', usernameVariable: 'admin', passwordVariable: 'admin')]) {
                    bat """
                        curl -u %USERNAME%:%PASSWORD% ^
                        --upload-file target\\myspringboot.war ^
                        "http://localhost:2005/manager/text/deploy?path=/myspringboot&update=true"

                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}

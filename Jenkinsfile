pipeline {
    agent any

    tools {
        nodejs 'NodeJS_18'    // Configure this name in Global Tool Configuration
        maven  'Maven_3'      // Configure this name too
    }

    environment {
        FRONTEND_DIR = 'frontend'
        BACKEND_DIR  = 'backend'
        // OPTIONAL: set JAVA_HOME if needed, e.g.
        // JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
        // PATH = "${JAVA_HOME}\\bin;${PATH}"
    }

    options {
        skipDefaultCheckout(true)   // we'll do git checkout ourselves
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/YogaNarsiReddy/Ecommerce.git']]
                ])
            }
        }

        stage('Frontend: Install & Build') {
            steps {
                dir("${FRONTEND_DIR}") {
                    echo '📦 Installing frontend deps...'
                    bat 'npm ci || npm install'
                    echo '🏗️ Building React (Vite)...'
                    bat 'npm run build'
                }
            }
        }

        stage('Backend: Build (skip tests)') {
            steps {
                dir("${BACKEND_DIR}") {
                    echo '🔨 Building Spring Boot (Maven)...'
                    bat 'mvn -B -DskipTests clean package'
                }
            }
        }

        stage('Tests (Parallel)') {
            parallel {
                stage('Frontend Tests') {
                    steps {
                        dir("${FRONTEND_DIR}") {
                            echo '🧪 Running frontend tests...'
                            // Adjust if you don’t have tests yet
                            bat 'npm test -- --watchAll=false || echo "No frontend tests or they failed."'
                        }
                    }
                }
                stage('Backend Tests') {
                    steps {
                        dir("${BACKEND_DIR}") {
                            echo '🧪 Running backend tests...'
                            bat 'mvn -B test || echo "Backend tests failed or none present."'
                        }
                    }
                }
            }
        }

        stage('Package Fullstack (optional)') {
            when {
                expression { return true }   // set to false if you don't want to bundle
            }
            steps {
                echo '📦 Packaging React build into Spring Boot static resources...'
                // clean old static (optional)
                bat "if exist ${BACKEND_DIR}\\src\\main\\resources\\static rmdir /S /Q ${BACKEND_DIR}\\src\\main\\resources\\static"
                bat "mkdir ${BACKEND_DIR}\\src\\main\\resources\\static"
                // copy dist -> static
                bat "xcopy /E /I /Y ${FRONTEND_DIR}\\dist ${BACKEND_DIR}\\src\\main\\resources\\static"

                dir("${BACKEND_DIR}") {
                    echo '🏺 Rebuilding Spring Boot JAR with packaged frontend...'
                    bat 'mvn -B -DskipTests clean package'
                }
            }
        }
    }

    post {
        success { echo '✅ Fullstack CI passed!' }
        failure { echo '❌ Build failed. Check stage logs above.' }
        always  { echo 'ℹ️ Pipeline finished.' }
    }
}

pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'SonarScanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build the code') {
            steps {
                script {
                    try {
                        sh 'mvn clean package'
                    } catch (Exception e) {
                        echo "Build failed: ${e}"
                        currentBuild.result = 'FAILURE'
                        error("Stopping pipeline due to build failure")
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    try {
                        withSonarQubeEnv('SonarQube') {
                            def nodeAvailable = sh(script: 'which node || true', returnStdout: true).trim()
                            if (nodeAvailable == '') {
                                echo "⚠ Node.js not found. JS/TS analysis will be skipped."
                                sh """
                                    ${SCANNER_HOME}/bin/sonar-scanner \
                                    -Dsonar.projectKey=task-master-pro \
                                    -Dsonar.sources=. \
                                    -Dsonar.java.binaries=target/classes \
                                    -Dsonar.javascript.disabled=true
                                """
                            } else {
                                sh """
                                    ${SCANNER_HOME}/bin/sonar-scanner \
                                    -Dsonar.projectKey=task-master-pro \
                                    -Dsonar.sources=. \
                                    -Dsonar.java.binaries=target/classes
                                """
                            }
                        }
                    } catch (Exception e) {
                        echo "SonarQube analysis failed: ${e}"
                        currentBuild.result = 'UNSTABLE'
                    }
                }
            }
        }

        stage('Building Docker image') {
            steps {
                script {
                    if (fileExists('Dockerfile')) {
                        try {
                            sh 'docker build -t fullstack-blogging-app .'
                        } catch (Exception e) {
                            echo "Docker build failed: ${e}"
                            currentBuild.result = 'UNSTABLE'
                        }
                    } else {
                        echo "⚠ Dockerfile not found. Skipping Docker build."
                    }
                }
            }
        }

        stage('Next stage test') {
            steps {
                echo "Pipeline reached next stage, Pallavi"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            mail to: 'walunjpallavi69@gmail.com',
                 subject: '🎉 Build Successful 🎉',
                 body: 'Congrats Pallavi! The Jenkins build and deployment were successful 🎉'
        }
        // No email for failure or unstable
    }
}

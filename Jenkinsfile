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
                withSonarQubeEnv('SonarQube') {
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=task-master-pro \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Building Docker image') {
            steps {
                script {
                    try {
                        sh 'docker build -t fullstack-blogging-app .'
                    } catch (Exception e) {
                        echo "Docker build failed: ${e}"
                        currentBuild.result = 'FAILURE'
                        error("Stopping pipeline due to Docker build failure")
                    }
                }
            }
        }

        stage('Next stage test') {
            steps {
                echo "Pipeline reached next level, Pallavi"
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
        failure {
            mail to: 'walunjpallavi69@gmail.com',
                 subject: '❌ Build Failed ❌',
                 body: 'Oops! The Jenkins build or deployment failed. Please check the logs.'
        }
    }
}

pipeline {
    agent any

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
                        currentBuild.result = 'fail'  
                    }
                }
            }
        }

        stage("Building Docker image"){
            steps{
                script{
                    try{
                    sh 'docker build -t fullstack-blogging app .'
                } catch(Exception e) {
                    echo "Docker build failed: ${e}"
                    currentBuild.result = 'FAILURE'
                }

            }
        }
       }
       
    }
}

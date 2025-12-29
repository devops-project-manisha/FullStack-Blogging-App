pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

          stage('Build the code') {
            steps {
                sh "mvn clean package"
            }
        }
    }
}

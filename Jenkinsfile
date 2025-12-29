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
                    }
                }
            }
        }
		
		stage ('Sonarqube Analysis') {
		    steps {
			   withSonarQubeEnv('SonarQube'){
			     sh "${SCANNER_HOME}/bin/sonar-scanner"
			   }
			}
		
		}

        stage("Building Docker image") {
            steps {
                script {
                    try {
                        sh 'docker build -t fullstack-blogging-app .'
                    } catch(Exception e) {
                        echo "I am inside Docker catch block"
                        echo "Docker build failed: ${e}"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }

        stage ("Next stage test") {
            steps {
                echo "pipeline reached next level palvi"
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
}

}

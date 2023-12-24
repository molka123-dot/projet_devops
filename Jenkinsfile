pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/rchidana/calcwebapp.git'    
		            echo "Code Checked-out Successfully!!";
            }
        }
        
        stage('Package') {
            steps {
                bat 'mvn package'    
		            echo "Maven Package Goal Executed Successfully!";
            }
        }
        stage('SonarQube analysis') {
            steps {
		// Change this as per your Jenkins Configuration
                withSonarQubeEnv('SonarQube_server') {
                    bat 'mvn package sonar:sonar'
                }
            }
        }
}
}
 

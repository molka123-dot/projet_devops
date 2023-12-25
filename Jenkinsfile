pipeline {
    agent any
    
    environment {
        // Définissez ici les variables d'environnement nécessaires pour votre pipeline
        JAVA_HOME = tool 'java17'
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupération du code source depuis le référentiel GitHub
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Utilisation de Maven pour construire le projet Spring Boot
                sh 'mvn clean install'
            }
        }

        stage('Run SonarQube Analysis') {
            steps {
                // Exécution de l'analyse SonarQube
                withSonarQubeEnv('SonarQube_server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Deploy') {
            steps {
                // Déployer votre application (peut être adapté en fonction de votre cas)
                // Exemple : sh 'mvn deploy'
                sh 'echo "Déploiement de l\'application"'
            }
        }
    }

    post {
        success {
            // Actions à effectuer en cas de succès du pipeline
            echo 'Le pipeline a réussi!'
            // Ajoutez d'autres actions en cas de succès
        }
        failure {
            // Actions à effectuer en cas d'échec du pipeline
            echo 'Le pipeline a échoué!'
            // Ajoutez d'autres actions en cas d'échec
        }
    }
}

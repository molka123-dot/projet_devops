pipeline {
    agent any

    tools {
        maven "M3"
        sonarqube 'sonar_scanner'
    }

    environment {
        DB_URL = 'jdbc:mysql://localhost:3306/formation3'
        DB_USER = 'root'
        DB_PASSWORD = 'root'
        SONARQUBE_URL = 'http://localhost:9000'
        SONARQUBE_TOKEN = 'sqa_92b2b916ac4ff86d5884f774fd7887e0bd89d212'
        DOCKERHUB_CREDENTIALS = credentials('chernimolka-dockerhub')

    }

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/molka123-dot/projet_devops.git'
                
                sh "echo 'Configurer la base de données ici'"
                
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                success {
                    echo "Build réussi. Exécuter des actions spécifiques."
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube_Server') {
                        sh "mvn sonar:sonar \
                            
                    }
                }
            }
        }

stage('Deploy to Nexus') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: env.NEXUS_CREDENTIALS_ID, usernameVariable: 'jenkins', passwordVariable: 'jenkins')]) {
                        // Déployer l'artefact dans Nexus
                        sh "mvn deploy:deploy-file \
                            -Durl=https://example.com/nexus/repository/maven-releases/ \
                            -DrepositoryId=nexus \
                            -DgroupId=com.example \
                            -DartifactId=my-artifact \
                            -Dversion=1.0.0 \
                            -Dpackaging=jar \
                            -Dfile=target/my-artifact.jar \
                            -DgeneratePom=false \
                            -DpomFile=pom.xml \
                            -DgeneratePom.description='Nexus Repository for my project'
                    }
                }
            }
        }
           stage('Build docker image') {
            steps {  
                sh 'docker build -t chernimolka/clientserv:1.1 .'
            }
        }

        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('push image') {
            steps{
                sh 'docker push chernimolka/clientserv:1.1'
            }
        }
    }
    }
}


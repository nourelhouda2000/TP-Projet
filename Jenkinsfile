pipeline {
    agent any

    tools {
        maven 'M2_HOME'
        jdk 'JAVA_HOME'
    }

    environment {
        SONAR_AUTH_TOKEN = credentials('SONAR_AUTH_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/nourelhouda2000/TP-Projet.git'
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    if (isUnix()) {
                        // Forcer les permissions pour mvnw avant de lancer Maven
                        sh 'chmod +x mvnw && ./mvnw clean install -DskipTests=false -Ptest'
                    } else {
                        bat 'mvnw.cmd clean install -DskipTests=false -Ptest'
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_AUTH_TOKEN', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('nour') {
                        script {
                            if (isUnix()) {
                                sh "./mvnw sonar:sonar -Dsonar.projectKey=TP-Projet -Dsonar.projectName=tp_projet_db -Dsonar.login=$SONAR_TOKEN"
                            } else {
                                bat "mvnw.cmd sonar:sonar -Dsonar.projectKey=TP-Projet -Dsonar.projectName=tp_projet_db -Dsonar.login=%SONAR_TOKEN%"
                            }
                        }
                    }
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build TP-Projet succeeded!'
        }
        failure {
            echo 'Build TP-Projet failed!'
        }
    }
}

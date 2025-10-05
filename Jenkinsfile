pipeline {
    agent any

    tools {
        maven 'M2_HOME'   // Nom de ton Maven installé dans Jenkins
        jdk 'JAVA_HOME'    // Nom de ton JDK installé dans Jenkins
    }

    environment {
        SONAR_AUTH_TOKEN = credentials('SONAR_AUTH_TOKEN') // Ton token SonarQube
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/nourelhouda2000/TP-Projet.git'
            }
        }

        stage('Prepare') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'chmod +x mvnw'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh './mvnw clean install'
                    } else {
                        bat 'mvnw.cmd clean install'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh './mvnw test'
                    } else {
                        bat 'mvnw.cmd test'
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Nom de ton serveur SonarQube
                    script {
                        if (isUnix()) {
                            sh "./mvnw sonar:sonar -Dsonar.projectKey=TP-Projet -Dsonar.projectName=\"tp_projet_db\" -Dsonar.login=$SONAR_AUTH_TOKEN"
                        } else {
                            bat "mvnw.cmd sonar:sonar -Dsonar.projectKey=TP-Projet -Dsonar.projectName=\"tp_projet_db\" -Dsonar.login=%SONAR_AUTH_TOKEN%"
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

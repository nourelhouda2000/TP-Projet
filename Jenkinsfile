pipeline {
    agent any

    tools {
        maven 'M2_HOME'   // Nom de ton Maven installé dans Jenkins
        jdk 'JAVA_HOME'    // Nom de ton JDK installé dans Jenkins
    }

    environment {
        // Utiliser l'ID exact du credential dans Jenkins
        SONAR_AUTH_TOKEN = credentials('SONAR_AUTH_TOKEN') 
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
                withCredentials([string(credentialsId: 'SONAR_AUTH_TOKEN', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('nour') { // Nom exact de ton installation SonarQube dans Jenkins
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

pipeline {
    agent any

    tools {
        maven 'M2_HOME'    // Nom du Maven configuré dans Jenkins
        jdk 'JAVA_HOME'    // Nom du JDK configuré dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/nourelhouda2000/TP-Projet.git'
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean install'
            }
        }

        stage('Test') {
            steps {
                sh './mvnw test'
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
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}

# Tomcat
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git branch: 'main', credentialsId: '1234', url: 'https://github.com/Lakshma12/Tomcat.git'
            }
        }

        stage('Build') {
            steps {
                // Build the project and package it as a WAR file
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the WAR file to Tomcat
                sh '''
                 echo "your_password" | sudo -S cp -r /var/lib/jenkins/workspace/tomcat/target/hello-world-1.0-SNAPSHOT.jar /opt/tomcat/webapps/hello.jar
                '''
            }
        }
    }

    post {
        success {
            echo 'Build and deployment were successful!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}

pipeline {

    agent any

    tools {
        jdk 'JDK-21'
        maven 'Maven-3.9.16'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Package WAR') {
            steps {
                bat 'mvn package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                bat '''
                copy /Y target\\jenkins-tomcat-demo.war R:\\DevTools\\apache-tomcat-11.0.24\\webapps\\jenkins-tomcat-demo.war
                '''
            }
        }
    }

    post {

        always {
            junit 'target/surefire-reports/*.xml'
        }

        success {
            echo 'BUILD AND DEPLOYMENT SUCCESSFUL!'
        }

        failure {
            echo 'BUILD FAILED!'
        }
    }
}
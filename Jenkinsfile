pipeline {
    agent any
    tools {
        jdk 'Jdk17'
        maven 'maven-3.8.6'
    }
    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-secret', url: 'https://github.com/Angad-Raut/service-registry.git']])
                bat 'mvn clean install'
                echo 'Git Checkout Completed'
            }
        }
        stage('Code Compile') {
            steps {
                bat 'mvn clean compile'
            }
        }
        stage('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }
        stage('Build Project') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('Archive Artifacts'){
            steps {
                archiveArtifacts artifacts: 'target/*.war'
            }
        }
        stage('Deploy on Tomcat') {
             steps {
                  deploy adapters: [tomcat9(url: 'http://localhost:8085/',
                                 credentialsId: 'tomcat-creds')],
                         war: 'target/*.war',
                         contextPath: 'service-registry'
             }
        }
        stage('Notification'){
             steps {
                  emailext(
                     subject: 'Service Registry Microservice Deployed',
                     body: 'Service registry microservice successfully deployed on tomcat server',
                     to: 'angadraut89@gmail.com'
                  )
                  echo 'SUCCESS'
             }
        }
    }
}

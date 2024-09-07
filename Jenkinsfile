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
        stage('Build Artifact') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('Deploy on Tomcat') {
             steps {
                  bat 'cp http://localhost:8080/job/Docker-Container/job/service-registry/$BUILD_NUMBER/execution/node/3/ws/target/eureka-server.war  C:/Program Files/Apache Software Foundation/Tomcat 9.0/webapps/'
                  echo 'SUCCESS'
             }
        }
    }
}

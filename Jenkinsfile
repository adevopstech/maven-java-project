pipeline {
    
    agent any
    
    tools {
        jdk 'myjava'
        git 'Default'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: '']])
            }
        }
        
        stage('Code Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        
        stage('Code Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Code Deploy') {
            steps {
                sh 'mvn package'
            }
        }
    }
    
     post {
                success {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
}

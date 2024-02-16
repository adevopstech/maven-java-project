pipeline {

    parameters {
        choice(
            name: 'Choice',
            choices: ['Test', 'Dev', 'Prod'],
            description: 'Pick env to deploy app'
            )
        string(
            name: 'Approver',
            defaultValue: 'jadmin',
            description: 'Who is the Approver?'
        )
    }

    agent any

    //List of tools
    tools {
        jdk 'myjava'
        git 'Default'
        maven 'mymaven'
    }

    // Keeps the last 1 build
    options {
        buildDiscarder(logRotator(numToKeepStr: '1'))
    }

    //maven-java project stages block
    stages {
        stage('Env Selection') {
            steps {
                echo "Choice: ${params.Choice}"
            }
        }
        stage('Git Checkout') {
            steps {
                echo 'Git Repository check'
                checkout scm: scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url:'https://github.com/adevopstech/addressbook.git']])
            }
        }
        stage('Code Compile') {
            steps {
                echo 'Code compile through Maven'
                sh 'mvn compile'
            }
        }
        stage('Code Test') {
            steps {
                echo 'Code test through Maven'
                sh 'mvn test'
            }
        }
        stage('Code Deploy') {
            input {
                message "Are we continue to deploy?"
                submitter "Jenkins Server"
            }
            steps {
                echo "Hello, ${Approver}, Thanks for Approval."
                echo 'Create package through Maven.'
                sh 'mvn package'
            }
        }
    }

    //test report block
    post {
        success {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}

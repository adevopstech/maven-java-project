pipeline {

    parameters {
        choice(
            name: 'CHOICE',
            choices: ['Test', 'Dev', 'Prod'],
            description: 'Pick env to deploy app'
            )
        string(
            name: 'Git-Repo-Link',
            defaultValue: 'https://github.com/adevopstech/addressbook.git',
            description: 'Requierement for Git Repo Lnik'
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
                echo "Choice: ${params.CHOICE}"
            }
        }
        stage('Git Checkout') {
            steps {
                echo 'Git Repository check'
                checkout scm: scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: ${params.Git-Repo-Link}]])
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
            steps {
                echo 'Craete package through Maven'
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

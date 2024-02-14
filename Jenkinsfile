/* groovylint-disable CompileStatic */
pipeline {
    /* groovylint-disable-next-line CompileStatic */
    agent any
    tools {
        jdk 'myjava'
        git 'Default'
    }
    stages {
        stage('Git Checkout') {
            steps {
                echo 'Git Repository check'
                /* groovylint-disable-next-line LineLength */
                checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/adevopstech/maven-java-project.git']])
            }
        }
        stage('Code Compile') {
            triggers {
               cron('* * * * *')
            }
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
    post {
        success {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}

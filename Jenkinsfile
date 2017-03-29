pipeline {

    agent {
        docker {
            image 'maven:3.3.9-jdk-7'
            label 'docker'
        }
    }

    options {
        // General Jenkins job properties
        buildDiscarder(logRotator(numToKeepStr: '40'))
        // Timestamps
        timestamps()
    }

    stages {
        stage('Build') {
            steps {
                sh '''\
#!/bin/bash
mvn clean verify
'''
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Release approval') {
            steps {
                input "Publishing this version?"
            }
        }
        stage('Release') {
            steps {
                echo "Published!"
            }
        }
    }
}
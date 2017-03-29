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
                input(
                        id: 'version',
                        message: "Publishing this version?",
                        parameters: [
                                string(name: 'VERSION', description: 'Version to create')
                        ]
                )
            }
        }
        stage('Release') {
            steps {
                echo "Publishing ${version}"
            }
        }
    }
}
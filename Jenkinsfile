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
mvn clean verify --batch-mode
'''
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        def version = null
        stage('Release approval') {
            agent none
            steps {
                version = input(
                        id: 'versionInput',
                        message: "Publishing this version?",
                        parameters: [
                                string(name: 'VERSION', description: 'Version to create')
                        ]
                )
            }
        }
        stage('Release') {
            steps {
                sh """\
#!/bin/bash
echo "Publishing ${version}"
"""

            }
        }
    }
}
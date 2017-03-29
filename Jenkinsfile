pipeline {
  agent {
    docker {
      image 'maven:3.3.9-jdk-7'
      label 'docker'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        sh '''#!/bin/bash
mvn clean verify --batch-mode
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
        script {
          env.VERSION = input(
            id: 'versionInput',
            message: "Publishing this version?",
            parameters: [
              string(name: 'VERSION', description: 'Version to create')
            ]
          )
        }
        
      }
    }
    stage('Release') {
      steps {
        sh '''#!/bin/bash
echo "Setting ${VERSION} version"
mvn versions:set -DgenerateBackupPoms=false -DnewVersion=${VERSION}'''
      }
    }
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '40'))
    timestamps()
  }
}
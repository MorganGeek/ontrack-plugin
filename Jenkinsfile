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
        environment {
            GITHUB = credentials('GITHUB')
        }
      steps {
        sh '''\
#!/bin/bash
echo "Setting ${VERSION} version"
git clean -xfd
mvn versions:set -DgenerateBackupPoms=false -DnewVersion=${VERSION}

echo "Tagging locally"
git config --local user.email "jenkins@nemerosa.net"
git config --local user.name "Jenkins"
git commit -am "Release ${VERSION}"
git tag "${VERSION}"
'''
      }
    }
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '40'))
    timestamps()
  }
}
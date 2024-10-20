pipeline {
  agent any
  stages {
    stage('CI : Prepare the region') {
      steps {
        sh '''cd /opt/kickstart-docker/
'''
      }
    }

    stage('CI : Confirmation') {
      steps {
        input 'Region set up complete'
      }
    }

    stage('CI : Image building') {
      steps {
        sh 'sudo docker image build -t sloopstash/mongo-db:v4.4.4 -f image/mongo-db/4.4.4/mongo-db.dockerfile image/mongo-db/4.4.4/context'
      }
    }

    stage('Ci : Confirmation') {
      steps {
        input 'Build successful, wanna proceed ?'
      }
    }

  }
}
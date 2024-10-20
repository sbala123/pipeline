pipeline {
  agent any
  stages {
    stage('Prepare the working Dir') {
      steps {
        sh '''cd /opt/kickstart-docker/
'''
      }
    }

    stage('CI : Confirmation') {
      steps {
        input 'The working Directory is ready'
      }
    }

    stage('Building the image') {
      steps {
        sh '''sudo docker image build -t sloopstash/mongo-db:v4.4.4 -f image/mongo-db/4.4.4/mongo-db.dockerfile image/mongo-db/4.4.4/context
'''
      }
    }

    stage('Image Built') {
      parallel {
        stage('Image Built') {
          steps {
            input 'Build successful, wanna proceed ?'
          }
        }

        stage('Confirmation - 2') {
          steps {
            input 'second level confirmation'
          }
        }

      }
    }

    stage('CI : Container run') {
      steps {
        sh '''sudo docker container run -d -i -t --rm \\
--name sloopstash-dev-prometheus \\
-h prometheus \\
-p 7070:9090 \\
--network sloopstash-dev-crm_common \\
--ip 15.1.1.50 \\
--mount \'type=volume,src=sloopstash-dev-prometheus-data,dst=/opt/prometheus/data\' \\
--mount \'type=volume,src=sloopstash-dev-prometheus-log,dst=/opt/prometheus/log\' \\
-v /opt/kickstart-docker/workload/supervisor/conf/server.conf:/etc/supervisord.conf \\
-v /opt/kickstart-docker/workload/prometheus/2.53.1/conf/supervisor.ini:/opt/prometheus/system/supervisor.ini \\
-v /opt/kickstart-docker/workload/prometheus/2.53.1/conf/prometheus.yml:/opt/prometheus/conf/prometheus.yml \\
--entrypoint /usr/bin/supervisord \\
sloopstash/prometheus:v2.53.1 \\
-c /etc/supervisord.conf'''
      }
    }

    stage('The container is up and running.! did you verify ?') {
      steps {
        input 'Container up brought up, happy ?!'
      }
    }

    stage('Terminate the container') {
      steps {
        sh 'sudo docker container stop sloopstash-dev-prometheus'
      }
    }

  }
}
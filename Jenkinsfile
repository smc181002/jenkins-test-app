pipeline {
  agent any

  stages {
    stage ('create podman volume') {
      steps{
        sh 'sudo podman volume create synapse-data'
      }
    }

    stage('create the homeserver.yml file') {
      steps {
        sh 'sudo podman run -dit  -v synapse-data:/data -e SYNAPSE_SERVER_NAME=my.matrix.host -e SYNAPSE_REPORT_STATS=yes matrixdotorg/synapse:latest generate' }
        sh 'sudo cp ./homeserver.yml /var/lib/containers/storage/synapse-data/_data/'
    }

    stage('start synapse server') {
      sh 'sudo podman run -dit -p 8008:8008 -p 4143:443 -v synapse-data:/data -e SYNAPSE_SERVER_NAME=my.matrix.host -e SYNAPSE_REPORT_STATS=yes matrixdotorg/synapse:latest'
    }
  }
}

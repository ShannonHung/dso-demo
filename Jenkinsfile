pipeline {
  agent {
    kubernetes {
      yamlFile 'build-agent.yaml'
      defaultContainer 'maven'
      idleMinutes 1
    }
  }
  stages {
    stage('Build') {
      parallel {
        stage('Compile') {
          steps {
            container('maven') {
              sh 'mvn compile'
            }
          }
        }
      }
    }

    stage('Test') {
      parallel {
        stage('Unit Tests') {
          steps {
            container('maven') {
              sh 'mvn test'
            }
          }
        }
      }
    }


    stage('Package') {
      parallel {
        stage('Create Jarfile') {
          steps {
            container('maven') {
              sh 'mvn package -DskipTests'
            }
          }
        }
      }
    }

    stage('Push to Docker Registry') {
      parallel {
        stage('Build Image') {
          steps {
            container('kaniko') {
              echo $DOCKER_HUB_CREDENTIAL
              // echo "{\"auths\":{\"https://index.docker.io/v1/\":{\"auth\":\"$DOCKER_HUB_CREDENTIAL\"}}}" > docker.json
              // sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --force --insecure --skip-tls-verify --cache=true --destination=docker.io/shannonhung/dso-demo'
            }
          }
        }
      }
    }


    stage('Deploy to Dev') {
      steps {
        // TODO
        sh "echo done"
      }
    }
  }
}

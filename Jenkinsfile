pipeline {
  agent {
    kubernetes {
      yamlFile 'build-agent.yaml'
      defaultContainer 'maven'
      idleMinutes 1
    }
  }


  stages {
//     stage('Build') {
//       parallel {
//         stage('Compile') {
//           steps {
//             container('maven') {
//               sh 'mvn compile'
//             }
//           }
//         }
//       }
//     }

    stage('Package') {
      parallel {
        stage('Create Jarfile') {
          steps {
            container('maven') {
              sh 'mvn package -DskipTests'
            }
          }
        }

        stage('Docker BnP') {
           steps {
               container(name: 'kaniko') {
                   sh 'echo "{\"auths\":{\"https://index.docker.io/v1/\":{\"auth\":\"${DOCKER_HUB_CREDENTIAL}\"}}}" > docker.json'
                   sh 'cat docker.json'
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

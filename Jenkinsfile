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
                   sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --force --insecure --skip-tls-verify --customPlatform=linux/arm64  --cache=true --destination=docker.io/shannonhung/dso-demo'
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

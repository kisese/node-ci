pipeline{  
  environment {
    registry = "kisese/node-ci"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
    stages {
        stage('Build'){
           steps{
                nodejs(nodeJSInstallationName: 'node') {
                    sh 'npm install'
                }
           }   
        }
        stage('Building image') {
            steps{
                script {
                  dockerImage = docker.build registry + ":latest"
                 }
             }
          }
          stage('Push Image') {
              steps{
                  script {
                       docker.withRegistry( '', registryCredential){                            
                       dockerImage.push()
                      }
                   }
                } 
           }
           stage('Deploying into k8s'){
            steps{
                sh 'kubectl apply -f deployment.yaml' 
            }
        }
    }
}
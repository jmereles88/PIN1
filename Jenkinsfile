pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    ARTIFACT_ID = "elbuo8/webapp:${env.BUILD_NUMBER}"
    // Define las credenciales de Docker Hub
    registryCredential = '131d3016-9c7c-41ef-a3b0-407a9cd5f400'

  }
   stages {
   stage('Building image') {
      withCredentials([usernamePassword(credentialsId: registryCredential, passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
      steps{
          sh '''
          def dockerImage = docker.build("jmereles88/mundose:latest")
          cd webapp
          docker build -t testapp .

          docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials-id') {
          dockerImage.push()

             '''  
        }
    }
  
  
    stage('Run tests') {
      steps {
        // cambio directorio
        dir('webapp') 
        sh "docker run testapp npm test"
      }
    }
   stage('Deploy Image') {
      steps{
        sh '''
        docker tag testapp 127.0.0.1:5000/mguazzardo/testapp
        docker push 127.0.0.1:5000/jmereles88/testapp   
        '''
        }
      }
      
    }
}


    
  


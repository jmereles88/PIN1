pipeline {
    agent any

    options {
        timeout(time: 2, unit: 'MINUTES')
    }

    environment {
        ARTIFACT_ID = "elbuo8/webapp:${env.BUILD_NUMBER}"
        DOCKER_HUB_CREDENTIALS = '131d3016-9c7c-41ef-a3b0-407a9cd5f400' // Reemplaza con el ID de tus credenciales de Docker Hub en Jenkins
        DOCKER_HUB_REGISTRY = 'docker.io' // Docker Hub Registry
        DOCKER_HUB_USERNAME = 'jmereles88' // Reemplaza con tu nombre de usuario de Docker Hub
        DOCKER_IMAGE_NAME = 'jmereles/testapp' // Reemplaza con tu nombre de imagen en Docker Hub
    }

    stages {
        stage('Building image') {
            steps {
                script {
                    dir('webapp') {
                        // Construir la imagen Docker
                        sh 'docker build -t testapp .'
                    }
                }
            }
        }

        stage('Run tests') {
            steps {
                dir('webapp') {
                    // Ejecutar pruebas en la imagen Docker
                    sh 'docker run testapp npm test'
                }
            }
        }

        stage('Deploy Image') {
            steps {
                script {
                    // Autenticar en Docker Hub
                    withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                        // Etiquetar y empujar la imagen al registro de Docker Hub
                        sh "docker tag testapp $DOCKER_HUB_REGISTRY/$DOCKER_IMAGE_NAME"
                        sh "docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD $DOCKER_HUB_REGISTRY"
                        sh "docker push $DOCKER_HUB_REGISTRY/$DOCKER_IMAGE_NAME"
                    }
                }
            }
        }
    }
}

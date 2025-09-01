pipeline {
    agent { label 'devOps' }

    environment {
        DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
        FRONT_IMAGE_TAG = "mariemendoye/ml-frontend:${BUILD_NUMBER}"
        FRONT_IMAGE_LATEST = "mariemndoye/ml-frontend:latest"
    }

    stages {

        stage('Connexion docker') {
            steps {
                script {
                    sh 'echo $DOCKER_PASSWORD | docker login -u mariemendoye --password-stdin'
                }
            }
        }

        stage('Build et push de l\'image Frontend') {
            steps {
                script {
                        sh "docker build -t ${FRONT_IMAGE_TAG} ."
                        sh "docker tag ${FRONT_IMAGE_TAG} ${FRONT_IMAGE_LATEST}"
                        sh "docker push ${FRONT_IMAGE_TAG}"
                        sh "docker push ${FRONT_IMAGE_LATEST}"
                }
            }
        }

        stage('nettoyage') {
            steps {
                script {
                    // Supprission des images locales 
                    sh "docker rmi ${FRONT_IMAGE_TAG} || true"
                    sh "docker rmi ${FRONT_IMAGE_LATEST} || true"

                    // Suppression des images inutiles
                    sh "docker image prune -f"
                }
            }
        }
    }
}

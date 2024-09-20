pipeline{
    agent any
    environment{
        DOCKER_USERNAME = 'nkcharan'
        IMAGE_VERSION = 'v1'
        DOCKER_IMAGE_NAME = 'solar'
        DOCKERHUB_CREDENTIALS = credentials('docker-creds') 
    }

    stages {
        stage('Clone the GitHub repository') {
            steps {
                git branch: 'solar', url: 'https://github.com/charannk007/Staragile-Finance-New.git'
            }
        }

    stage('Docker Login') {
        steps {
            script {
                    withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    }
                }
            }
        }

    stage('Removing existing Images and Containers') {
        steps {
            script {
                    // Remove existing containers if any
                    sh """
                    CONTAINERS=\$(docker ps -aq)
                    if [ "\$CONTAINERS" ]; then
                        docker rm -f \$CONTAINERS
                    else
                        echo "No containers to remove"
                    fi
                    """

                    // Remove existing images if any
                    sh """
                    IMAGES=\$(docker images -q)
                    if [ "\$IMAGES" ]; then
                        docker rmi -f \$IMAGES
                    else
                        echo "No images to remove"
                    fi
                    """
                }
            }
        }

    stage('Docker Build') {
        steps {
                sh 'docker build -t ${DOCKER_IMAGE_NAME}:${IMAGE_VERSION} .'
                sh 'docker images'
            }
        }

    stage('Creating the Image') {
        steps {
                sh 'docker tag ${DOCKER_IMAGE_NAME}:${IMAGE_VERSION} ${DOCKER_USERNAME}/${DOCKER_IMAGE_NAME}:${IMAGE_VERSION}'
            }
        }

    stage('Docker Push Image') {
        steps {
                sh 'docker push ${DOCKER_USERNAME}/${DOCKER_IMAGE_NAME}:${IMAGE_VERSION}'
            }
        }
    }
   
}


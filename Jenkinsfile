pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *') // Vérifie les changements toutes les 5 minutes
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Assurez-vous que cet ID correspond aux credentials dans Jenkins
        IMAGE_NAME_SERVER = 'minabf/mern-server'
        IMAGE_NAME_CLIENT = 'minabf/mern-client'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    git branch: 'main',
                        url: 'git@github.com:minabl/TP3-DevOps.git',
                        credentialsId: 'Github_ssh' // Assurez-vous que ce credential existe et fonctionne
                }
            }
        }
        stage('Build Server Image') {
            when {
                changeset "server/**" // Exécute cette étape seulement si des fichiers dans le dossier 'server' ont changé
            }
            steps {
                dir('server') {
                    script {
                        env.DOCKER_IMAGE_SERVER = docker.build("${IMAGE_NAME_SERVER}")
                    }
                }
            }
        }
        stage('Build Client Image') {
            when {
                changeset "client/**" // Exécute cette étape seulement si des fichiers dans le dossier 'client' ont changé
            }
            steps {
                dir('client') {
                    script {
                        env.DOCKER_IMAGE_CLIENT = docker.build("${IMAGE_NAME_CLIENT}")
                    }
                }
            }
        }
        stage('Scan Server Image') {
            when {
                changeset "server/**" // Exécute cette étape seulement si des fichiers dans le dossier 'server' ont changé
            }
            steps {
                script {
                    echo "Scanning Server Image..."
                    sh """
                    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
                    aquasec/trivy:latest image --exit-code 0 --severity LOW,MEDIUM,HIGH,CRITICAL \
                    --timeout 10m \
                    ${IMAGE_NAME_SERVER}
                    """
                }
            }
        }
        stage('Scan Client Image') {
            when {
                changeset "client/**" // Exécute cette étape seulement si des fichiers dans le dossier 'client' ont changé
            }
            steps {
                script {
                    echo "Scanning Client Image..."
                    sh """
                    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
                    aquasec/trivy:latest image --exit-code 0 --severity LOW,MEDIUM,HIGH,CRITICAL \
                    --timeout 10m \
                    ${IMAGE_NAME_CLIENT}
                    """
                }
            }
        }
        stage('Push Images to Docker Hub') {
            steps {
                script {
                    echo "Logging in to Docker Hub..."
                    sh """
                    docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}
                    """
                    echo "Pushing images to Docker Hub..."
                    sh "docker push ${IMAGE_NAME_SERVER}:latest"
                    sh "docker push ${IMAGE_NAME_CLIENT}:latest"
                }
            }
        }
    }
    post {
        always {
            echo "Cleaning up Docker images..."
            sh 'docker system prune -f' // Nettoyage des images Docker non utilisées
        }
    }
}

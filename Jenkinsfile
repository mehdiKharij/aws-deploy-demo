pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'alibouali/aws-demo:1.0' // Image Docker mise à jour
        EC2_IP = '13.60.196.67' // IP publique de votre instance EC2
        KEY_PATH = 'C:/users/user/key-spring.pem' // Chemin exact vers votre clé privée
        USER = 'ec2-user' // Utilisateur par défaut pour Amazon Linux
    }

    stages {
        stage('Deploy to EC2') {
            steps {
                script {
                    echo 'Deploying the Docker container to the EC2 instance...'

                    // Connectez-vous à l'instance EC2 et déployez le conteneur Docker
                    sh """
                        ssh -i "${KEY_PATH}" -o StrictHostKeyChecking=no ${USER}@${EC2_IP} << EOF
                        # Mettre à jour le système et installer Docker si nécessaire
                        sudo yum update -y
                        sudo amazon-linux-extras enable docker
                        sudo yum install -y docker
                        sudo service docker start
                        sudo usermod -aG docker ${USER}

                        # Arrêter et supprimer tout conteneur existant nommé deployspring
                        docker stop deployspring || true
                        docker rm deployspring || true

                        # Pull de la dernière image depuis Docker Hub
                        docker pull ${DOCKER_IMAGE}

                        # Lancer un nouveau conteneur
                        docker run -d --name deployspring -p 80:8080 ${DOCKER_IMAGE}
                        EOF
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Application successfully deployed to EC2!'
        }
        failure {
            echo 'Deployment failed. Please check the logs.'
        }
    }
}

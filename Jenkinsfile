pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'alibouali/aws-demo:1.0'  // Image Docker mise à jour
        EC2_IP = '13.60.167.33'  // IP publique de votre instance EC2
        KEY_PATH = 'C:/users/user/key-spring.pem'  // Chemin exact vers votre clé privée
        USER = 'ec2-user'  // Utilisateur par défaut pour Amazon Linux
    }

    stages {
        stage('Deploy to EC2') {
            steps {
                script {
                    echo 'Deploying the Docker container to the EC2 instance...'

                    // Windows-specific SSH commands (using 'bat' instead of 'sh')
                    bat """
                        ssh -i "${KEY_PATH}" -T ${USER}@${EC2_IP} 
                        # Stop and remove any existing container named deployspring
                        docker stop deployspring || true
                        docker rm deployspring || true

                        # Pull the latest image from Docker Hub
                        docker pull ${DOCKER_IMAGE}

                        # Run a new container
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

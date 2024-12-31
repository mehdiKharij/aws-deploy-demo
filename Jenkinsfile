pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'alibouali/aws-demo:1.0'  // Image Docker mise à jour
        EC2_IP = '13.60.167.33'  // IP publique de votre instance EC2
        KEY_PATH = 'C:/keys/key-spring.pem'  // Chemin exact vers votre clé privée
        USER = 'ec2-user'  // Utilisateur par défaut pour Amazon Linux
    }

    stages {
        stage('Deploy to EC2') {
            steps {
                script {
                    echo 'Deploying the Docker container to the EC2 instance...'

                    // Windows-specific SSH commands (using 'bat' instead of 'sh')
                    bat """
                        ssh -i "${KEY_PATH}" -T -o StrictHostKeyChecking=no ${USER}@${EC2_IP} "docker stop deployspring || true"
                        ssh -i "${KEY_PATH}" -T -o StrictHostKeyChecking=no ${USER}@${EC2_IP} "docker rm deployspring || true"
                        ssh -i "${KEY_PATH}" -T -o StrictHostKeyChecking=no ${USER}@${EC2_IP} "docker pull ${DOCKER_IMAGE}"
                        ssh -i "${KEY_PATH}" -T -o StrictHostKeyChecking=no ${USER}@${EC2_IP} "docker run -d --name deployspring -p 80:8080 ${DOCKER_IMAGE}"
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

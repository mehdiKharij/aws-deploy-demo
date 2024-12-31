 pipeline {
    agent any

  stages {
        stage('Checkout') {
            steps {
                echo 'Cloning the Git repository...'
                git url: 'https://github.com/mehdiKharij/aws-deploy-demo.git', branch: 'main'
                echo 'Git checkout successful! Repository cloned.'
            }
        }

        stage('Build') {
            steps {
                echo 'Starting Maven build...'
                bat '"C:\\Program Files\\apache-maven-3.9.9\\bin\\mvn" clean install'
                echo 'Maven build successful!'
            }
        }

  
}

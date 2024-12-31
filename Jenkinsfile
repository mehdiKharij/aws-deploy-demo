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
                bat 'mvn clean install'
                echo 'Maven build successful!'
            }
        }
    } // Fin des stages
} // Fin du pipeline

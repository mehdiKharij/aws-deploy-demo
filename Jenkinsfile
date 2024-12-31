 pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone le dépôt Git
                git url: 'https://github.com/mehdiKharij/aws-deploy-demo.git', branch: 'main'
            }
        }

        s

        stage('Build Backend') {
            steps {
                dir('spring-boot-server') {
                    script {
                        // Utiliser Maven ou Gradle pour le build backend
                        sh './mvnw clean install' // Si Maven Wrapper est utilisé
                    }
                }
            }
        }
    }

  
}

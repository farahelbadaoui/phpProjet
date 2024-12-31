pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo "Cloner le projet..."
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/farahelbadaoui/phpProjet']]
                )
            }
        }
        stage('Install Composer') {
            steps {
                bat 'php -r "copy(\'https://getcomposer.org/installer\', \'composer-setup.php\');"'
                bat 'php composer-setup.php'
                bat 'php -r "unlink(\'composer-setup.php\');"'
                bat 'php composer.phar install'
            }
        }

        stage('Build') {
            steps {
                echo "Building de l'application..."
                bat 'composer install'
            }
        }

        stage('Dockerization') {
            steps {
                echo "Building l'image docker..."
                script {
                    bat 'docker build -t farahfrh/projetdevops .'
                }
            }
        }

        stage('Push') {
            steps {
                echo "Push de l'image docker..."
                script {
                    bat "docker login -u farahfrh -p farah12345"
                    bat 'docker push farahfrh/projetdevops'
                    }
            }
        }
    }

    post {
        always {
            echo "Traitement du workspace..."
            cleanWs()
        }
        success {
            echo "Pipeline réussit!"
        }
        failure {
            echo "Pipeline echoué."
        }
    }
}
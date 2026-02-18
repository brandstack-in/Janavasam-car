pipeline {
    agent docker 
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git Branch to Deploy')
        choice(name: 'ENVIRONMENT', choices: ['DEV', 'TEST', 'PROD'], description: 'Select Environment')
        string(name: 'DEPLOY_PATH', defaultValue: '/var/www/html', description: 'Deployment Path')
        booleanParam(name: 'RESTART_APACHE', defaultValue: true, description: 'Restart Apache After Deploy?')
    environment {
        DEPLOY_PATH = "/var/www/html"
        GIT_REPO = "https://github.com/brandstack-in/Janavasam-car.git"
    }

stages {

        stage('Checkout Code') {
            steps {
                git branch: "${params.BRANCH_NAME}", url: "${GIT_REPO}"
            }
        }

        stage('Build') {
            steps {
                echo "No build required for static site"
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Deploying files..."
                    cp -r public_html/* $DEPLOY_PATH/
                '''
            }
        }

        stage('Restart Apache') {
            steps {
                sh '''
                    echo "Restarting Apache..."
                    systemctl restart apache2
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
}

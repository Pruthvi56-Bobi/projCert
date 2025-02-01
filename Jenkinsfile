pipeline {
    agent none

    stages {
        stage('Checkout Code') {
            agent { label 'test-server' }  // Make sure 'test-server' is the label of your Jenkins Slave
            steps {
                checkout scm  // Checkout the latest code from GitHub repository
            }
        }

        stage('Install Dependencies') {
            agent { label 'test-server' }
            steps {
                sh 'ansible-playbook -i /etc/ansible/hosts install_dependencies.yml'  // Install dependencies via Ansible
            }
        }

        stage('Build Docker Image') {
            agent { label 'test-server' }
            steps {
                sh 'docker build -t php-app .'  // Build the Docker image for your PHP app
            }
        }

        stage('Deploy to Slave Node') {
            agent { label 'test-server' }
            steps {
                sh 'docker run -d -p 8080:80 php-app'  // Deploy the Docker container on the Slave Node
            }
        }
    }
}

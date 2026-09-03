pipeline {
    agent any

    tools {
        maven 'Maven-3.9'
        jdk 'JDK-21'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['5db0520d-6728-4730-879e-8169e80b5b7f']) {
                    sh '''
                        echo "Deploying artifact to remote server..."
                        scp -o StrictHostKeyChecking=no target/jenkinsproject1-java-1.0-SNAPSHOT.jar ubuntu@54.169.113.164:/opt/app/
                        ssh -o StrictHostKeyChecking=no ubuntu@54.169.113.164 "systemctl restart jenkinsproject1"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful!'
        }
        failure {
            echo 'Build or Deployment Failed!'
        }
    }
}

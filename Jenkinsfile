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
                echo "Starting Deploy stage..."
                sshagent(credentials: ['your-ssh-credentials-id']) {
                    sh '''
                        echo "Connected to ssh agent successfully."
                        echo "Checking target folder on remote server..."
                        ssh -o StrictHostKeyChecking=no ubuntu@54.169.113.164 "mkdir -p /opt/app"
                        
                        echo "Copying jar file..."
                        scp -o StrictHostKeyChecking=no target/jenkinsproject1-java-1.0-SNAPSHOT.jar ubuntu@54.169.113.164:/opt/app/
                        
                        echo "Restarting service..."
                        ssh -o StrictHostKeyChecking=no ubuntu@54.169.113.164 "systemctl restart jenkinsproject1"
                        
                        echo "Deployment commands finished."
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
            echo 'Build or Deployment Failed! Check the console output above.'
        }
    }
}

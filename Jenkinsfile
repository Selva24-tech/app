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
        sshagent(credentials: ['sel2']) {
            sh '''
                echo "Deploying artifact to remote server..."
                scp -o StrictHostKeyChecking=no target/jenkinsproject1-java-1.0-SNAPSHOT.jar ubuntu@54.253.129.59:/tmp/
                ssh -o StrictHostKeyChecking=no ubuntu@54.253.129.59 "sudo mv /tmp/jenkinsproject1-java-1.0-SNAPSHOT.jar /opt/my app/ && sudo systemctl restart jenkinsproject1"
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

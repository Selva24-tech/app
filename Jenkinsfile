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
                sh 'mvn package'
            }
        stage('Deploy') {
             steps {
              {
             sh '''
                echo "Deploying artifact to remote server..."
                scp target/jenkinsproject1-java-1.0-SNAPSHOT.jar ubuntu@54.169.113.164:/opt/app/
                ssh ubuntu@54.169.113.164 "systemctl restart jenkinsproject1"
            '''
        }
    }
}
        }
    }

    post {
        success {
            echo 'Build Successful!'
        }
        failure {
            echo 'Build Failed!'
        }
    }
}

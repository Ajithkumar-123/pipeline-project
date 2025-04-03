pipeline {
    agent any 

    environment {
        MAVEN_HOME = "/usr/share/maven"
        TOMCAT_USER = "admin"
        TOMCAT_PASS = "admin123"
        TOMCAT_URL = "http://your-server-ip:8080/manager/text"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/sample-java-app.git'
            }
        }

        stage('Build') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn clean package'
            }
        }

        stage('Run Tests') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = "target/sample-java-app.war"
                    sh "curl -v -u ${TOMCAT_USER}:${TOMCAT_PASS} -T ${warFile} ${TOMCAT_URL}/deploy?path=/sample-java-app"
                }
            }
        }

        stage('Notification') {
            steps {
                echo "Deployment completed successfully!"
                // Example: Send Slack notification
                // slackSend channel: '#deployments', message: 'Deployment Successful!', color: 'good'
            }
        }
    }

    post {
        failure {
            echo "Build or Deployment failed!"
        }
    }
}

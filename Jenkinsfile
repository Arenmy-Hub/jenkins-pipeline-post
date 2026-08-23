pipeline {
    agent any
   
    stages {
        stage('Create directory for the WEB Application') {
            steps {
                sh 'rm -rf ./tomcat-web'
                sh 'mkdir -p ./tomcat-web/shopping'
            }
        }
        stage('Copy the web application to the workspace directory') {
            steps {
                echo 'Copying web application files...'             
                sh 'cp -r shopping/* ./tomcat-web/shopping/'
                sh 'cp -r shopping/pages/* ./tomcat-web/shopping/ || true'
            }
        }
        stage('Drop the Apache Tomcat Docker container') {
            steps {
                echo 'Dropping the existing container...'
                sh 'docker rm -f tomcat1 || true'
            }
        }
        stage('Create and Start the Tomcat container') {
            steps {
                echo 'Creating container with mounted webapp...'
                sh 'docker run -dit --name tomcat1 -p 9090:8080 -v "${WORKSPACE}/tomcat-web":/usr/local/tomcat/webapps tomcat:9.0'
            }
        }
    }

    post {
        success {
            echo 'The deployment has worked successfully.'
        }
        failure {
            echo 'An error has occurred during deployment.'
        }
    }
}

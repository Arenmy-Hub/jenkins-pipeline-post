pipeline {
    agent any
   
    stages {
        stage('Create directory for the WEB Application') {
            steps {
                // Usamos el directorio de trabajo de Jenkins
                sh 'rm -rf ./tomcat-web'
                sh 'mkdir -p ./tomcat-web'
            }
        }
        stage('Drop the Apache Tomcat Docker container') {
            steps {
                echo 'Droping the container...'
                sh 'docker rm -f tomcat1 || true'
            }
        }
        stage('Create the Tomcat container') {
            steps {
                echo 'Creating the container...'
                // Usamos ${WORKSPACE} para que Docker encuentre la ruta correcta
                sh 'docker run -dit --name tomcat1 -p 9090:8080 -v "${WORKSPACE}/tomcat-web":/usr/local/tomcat/webapps tomcat:9.0'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Creating the shopping folder in the container'
                sh 'mkdir -p ./tomcat-web/shopping'
                echo 'Copying web application...'             
                sh 'cp -r shopping/* ./tomcat-web/shopping/'
            }
        }
    }

    post {
        success {
            echo 'the deployment has worked'
        }
        failure {
            echo 'An error has ocurred'
        }
    }
}

pipeline {
    agent any
   
    stages {
        stage('Create directory for the WEB Application') {
            steps {
                // Borra el directorio si existe
                sh 'rm -rf /home/jenkins/tomcat-web'
                // Usa -p para crear directorios padres si no existen
                sh 'mkdir -p /home/jenkins/tomcat-web'
            }
        }
        stage('Drop the Apache Tomcat Docker container') {
            steps {
                echo 'Droping the container...'
                // Agrega || true para que no falle si el contenedor no existe
                sh 'docker rm -f tomcat1 || true'
            }
        }
        stage('Create the Tomcat container') {
            steps {
                echo 'Creating the container...'
                sh 'docker run -dit --name tomcat1 -p 9090:8080 -v /home/jenkins/tomcat-web:/usr/local/tomcat/webapps tomcat:9.0'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Creating the shopping folder in the container'
                sh 'mkdir -p /home/jenkins/tomcat-web/shopping'
                echo 'Copying web application...'             
                sh 'cp -r shopping/* /home/jenkins/tomcat-web/shopping/'
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

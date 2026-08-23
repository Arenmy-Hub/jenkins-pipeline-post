pipeline {
    agent any
   
    stages {
        stage('Create directory for the WEB Application') {
            steps {
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
                sh 'docker run -dit --name tomcat1 -p 9090:8080 -v "${WORKSPACE}/tomcat-web":/usr/local/tomcat/webapps tomcat:9.0'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Creating the shopping folder in the container'
                sh 'mkdir -p ./tomcat-web/shopping'
                echo 'Copying web application...'             
                // Copia la estructura Java (WEB-INF, META-INF) y los recursos
                sh 'cp -r shopping/* ./tomcat-web/shopping/'
                // Si la vista principal está dentro de pages/, mueve su contenido a la raíz de shopping
                sh 'cp -r shopping/pages/* ./tomcat-web/shopping/ || true'
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

pipeline {
    agent any
   
    stages {
        stage('Create directory for ROOT Application') {
            steps {
                sh 'rm -rf ./tomcat-web'
                sh 'mkdir -p ./tomcat-web/ROOT'
            }
        }
        stage('Prepare ROOT application files') {
            steps {
                echo 'Preparing web application files...'             
                sh 'cp -r shopping/* ./tomcat-web/ROOT/'
                sh 'cp -r shopping/pages/* ./tomcat-web/ROOT/ || true'
                sh "echo '<% response.sendRedirect(\"welcome.jsp\"); %>' > ./tomcat-web/ROOT/index.jsp"
            }
        }
        stage('Recreate Tomcat Container') {
            steps {
                echo 'Recreating container...'
                sh 'docker rm -f tomcat1 || true'
                sh 'docker run -dit --name tomcat1 -p 9090:8080 tomcat:9.0'
            }
        }
        stage('Deploy files to Tomcat Container') {
            steps {
                echo 'Deploying ROOT application into running container...'
                // Copia la carpeta ROOT directamente dentro del contenedor
                sh 'docker cp ./tomcat-web/ROOT tomcat1:/usr/local/tomcat/webapps/'
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

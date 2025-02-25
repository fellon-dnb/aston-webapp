pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/fellon-dnb/aston-webapp.git'
        SERVER_IP = '46.229.213.155'
        TOMCAT_USER = 'root'
        TOMCAT_PASS = 'h+X1-gM+i-KiQv'
        TOMCAT_WEBAPPS = '/opt/tomcat/webapps'
        WAR_NAME = 'app.war'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${GITHUB_REPO}"
            }
        }

        stage('Build') {
            steps {
                bat 'mvnw clean package'  // для Windows, если на Linux — заменяем на sh './mvnw clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = findFiles(glob: 'target/*.war')[0].path
                    sh """
                        curl -v --user ${TOMCAT_USER}:${TOMCAT_PASS} \
                        --upload-file ${warFile} \
                        "http://${SERVER_IP}:8085/manager/text/deploy?path=/myapp&update=true"
                    """
                }
            }
        }
    }
}

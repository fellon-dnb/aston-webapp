pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/fellon-dnb/aston-webapp.git'
        SERVER_IP = '46.229.213.155'
        TOMCAT_USER = 'jenkins'
        TOMCAT_PASS = '12345678'
        TOMCAT_WEBAPPS = '/opt/tomcat/webapps'
        WAR_NAME = 'app.war'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: "${GITHUB_REPO}"
            }
        }

        stage('Build') {
            steps {
                script {

                    if (isUnix()) {
                        sh 'mvn clean package -Dfile.encoding=UTF-8'
                    } else {
                        bat 'mvn clean package' // Для Windows
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = findFiles(glob: 'target/*.war')[0].path
                    echo "Found WAR file: ${warFile}"

                    // Для Windows или Linux
                    if (isUnix()) {
                        sh """
                            curl -v --user ${TOMCAT_USER}:${TOMCAT_PASS} \
                            --upload-file ${warFile} \
                            "http://${SERVER_IP}:8085/manager/text/deploy?path=/myapp&update=true"
                        """
                    } else {
                        bat """
                            curl -v --user ${TOMCAT_USER}:${TOMCAT_PASS} ^
                            --upload-file ${warFile} ^
                            "http://${SERVER_IP}:8085/manager/text/deploy?path=/myapp&update=true"
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
        success {
            echo 'Build and Deploy succeeded!'
        }
        failure {
            echo 'Build or Deploy failed!'
        }
    }
}

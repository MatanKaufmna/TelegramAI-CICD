pipeline {
    agent any


    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    def nexusUrl = 'http://54.184.99.126:8081/repository/pypi-hosted/'
                    def nexusCredentialsId = 'matan_nexus'

                    withCredentials([usernamePassword(credentialsId: nexusCredentialsId, usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh ''' cd nexus '''
                        sh "pip install --index-url=${nexusUrl} --trusted-host http://34.221.130.126 --user --upgrade pip"

                    }
                }
            }
        }

        stage('Build') {
            steps {

                sh '''
                echo "Nexus Integration Build"
                cd nexus
                python3 -m build .
                '''
            }
        }
        stage('Publish') {
           steps {
                withCredentials([usernamePassword(credentialsId: 'matan_nexus', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')
              ]) {

                sh '''
                cd nexus
                echo $USERNAME
                echo $PASSWORD
                sed -i -e "s/<username>/$USERNAME/g" .pypirc
                sed -i -e "s/<password>/$PASSWORD/g" .pypirc
                python3 -m twine upload --config-file .pypirc --repository pypi-hosted dist/*
                '''
              }
            }
        }
    }
    post {
        cleanup {
            cleanWs()
        }
    }
}
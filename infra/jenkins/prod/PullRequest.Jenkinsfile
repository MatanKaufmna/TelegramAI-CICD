pipeline {
    agent {
        docker {

            image '700935310038.dkr.ecr.us-west-2.amazonaws.com/matan-jenkinsagent-cicd:1'
            args  '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    stages {
        stage('Linting test') {
            agent {
                docker {
                    image 'python:3.9.16-slim-buster'
                    reuseNode true
                }
            }
            steps {
              sh '''
                  pip3 install pylint
                  python3 -m pylint -f parseable --reports=no **/*.py > pylint.log
                '''
            }

            // Make sure the "Warnings Next Generation" plugin is installed!!
            post {
              always {
                sh 'cat pylint.log'
                recordIssues (
                  enabledForFailure: true,
                  aggregatingResults: true,
                  tools: [pyLint(name: 'Pylint', pattern: '**/pylint.log')]
                )
              }
            }
        }
    }
}

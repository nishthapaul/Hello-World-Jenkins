pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
        DOCKERHUB_USERNAME='nishthapaul'
        EMAIL_TO = 'paulnishtha19@gmail.com'
    }
    stages {
          stage ("One") {
                steps {
                    echo 'Hi it is the first step !!!'
                } 
          }

        stage ("Email Notif") {
            steps {
                script {
                    echo "Environment variables:"
                    echo env.PROJECT_NAME
                    echo env.BUILD_URL
                }
            }
        }

          stage ("Clone Git") {
                steps {
                    git branch: 'main', url: 'https://github.com/nishthapaul/Hello-World-Jenkinss.git'
                }
          }
        
          stage ("Build & Run Code") {
                steps {
                    sh 'docker build -t nishthapaul/hello-world-jenkin-project .'
                }
          }

          stage ("Push docker image") {
                steps {
                    script {
                    sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_CREDENTIALS'
                    sh 'docker push nishthapaul/hello-world-jenkin-project'
                    }
                }
          }
        
          stage ("Run Ansible Playbook") {
                steps {
                    sh 'ansible-playbook -i inventory deploy.yml'
                }
          }
    }
    post {
        failure {
            mail bcc: '', body: 'Check console output at ' + env.BUILD_URL + 'to view the results. \n\n ${CHANGES} \n\n ------------------', cc: '', from: '', replyTo: '', subject: 'Unstable build in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER', to: 'paulnishtha19@gmail.com'
            // mail(
            //     to: 'paulnishtha19@gmail.com',
            //     subject: 'Unstable build in Jenkins: ' + env.PROJECT_NAME + ' - #' + env.BUILD_NUMBER,
            //     body: 'Check console output at ' + env.BUILD_URL + ' to view the results.\n\n ' + CHANGES + '\n\n ------------------'
            // )
        }
        // unstable {
        //     emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
        //             to: "${EMAIL_TO}", 
        //             subject: 'Unstable build in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
        // }
        // changed {
        //     emailext body: 'Check console output at $BUILD_URL to view the results.', 
        //             to: "${EMAIL_TO}", 
        //             subject: 'Jenkins build is back to normal: $PROJECT_NAME - #$BUILD_NUMBER'
        // }
    }
}

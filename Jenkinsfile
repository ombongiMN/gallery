pipeline {
    agent any
    tools{
        nodejs "node"
    }

    stages {
        stage('Clone repository') {
            steps {
                git 'https://github.com/ombongiMN/gallery'
            }
        }
        stage('Installing dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage ('Test') {
            post {
                failure{
                    mail bcc: '', body: 'Jenkins failure test', cc: '', from: '', replyTo: '', subject: 'Failed test', to: 'ombongim391@gmail.com'
                }
            }
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy Heroku') {
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'treetop', variable: 'PASS')])  {
                    sh 'git push https://${PASS}@git.heroku.com/treetop.git master'
                  }
            }
        }
        stage('Slack notification') {
            steps {
                slackSend channel: '#maryip1', message: "BUILD ID  ${BUILD_ID}Link https://treetop.herokuapp.com/"
            }
        }
    }
    }            

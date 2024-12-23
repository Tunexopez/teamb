pipeline {
    agent any
    environment {
        SSH_CRED = credentials('daad931a-8809-47f4-b0bc-ef4cb84487d8')
        def CONNECT = 'ssh -o StrictHostKeyChecking=no ubuntu@3.99.174.54'
    }
    stages {
        
        stage('Build') {
            steps {
                echo 'building app'
                sh "pwd"
                sh "ls"
                sh "zip -r webapp.zip ."
                sh "ls"
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying app'
                sshagent(['daad931a-8809-47f4-b0bc-ef4cb84487d8']) {
                    sh 'scp -o StrictHostKeyChecking=no -i $SSH_CRED webapp.zip ubuntu@3.99.174.54:/home/ubuntu'
                    sh '$CONNECT "sudo apt install zip -y"'
                    sh '$CONNECT "sudo rm -rf /var/www/html/"'
                    sh '$CONNECT "sudo mkdir /var/www/html/"'
                    sh '$CONNECT "sudo unzip webapp.zip -d /var/www/html/"'
                }
            }
        }

        stage('Clean-Up') {
            steps {
                echo 'Remove existing files'
                deleteDir()
            }
        }
    }
}
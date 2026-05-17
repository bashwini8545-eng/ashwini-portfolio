pipeline {
    agent any

    environment {
        EC2_HOST = '3.16.163.235'
        EC2_USER = 'ec2-user'
        KEY_PATH = 'C:\\ProgramData\\Jenkins\\.jenkins\\agility.pem'
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                echo 'Checking out from GitHub...'
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/bashwini8545-eng/ashwini-portfolio'
                echo 'Checkout successful!'
            }
        }

        stage('Test EC2 Connection') {
            steps {
                echo 'Testing EC2 connection...'
                bat """
                    ssh -o StrictHostKeyChecking=no -i "%KEY_PATH%" %EC2_USER%@%EC2_HOST% "echo EC2 Connection Successful!"
                """
            }
        }

    }

    post {
        success {
            echo '✅ All connections working!'
        }
        failure {
            echo '❌ Something failed - check console output'
        }
    }
}

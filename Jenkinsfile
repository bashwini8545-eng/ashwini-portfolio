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
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'ec2-ssh-key',
                    keyFileVariable: 'C:\\ProgramData\\Jenkins\\.jenkins\\agility.pem'
                )]) {
                    bat """
                        icacls "%KEY_FILE%" /inheritance:r
                        icacls "%KEY_FILE%" /remove "Everyone"
                        icacls "%KEY_FILE%" /grant:r "NT AUTHORITY\\SYSTEM:(R)"
                        ssh -o StrictHostKeyChecking=no -i "%KEY_FILE%" ec2-user@%EC2_HOST% "echo Connected!"
                    """
                }
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

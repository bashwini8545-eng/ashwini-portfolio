pipeline {
    agent any

    stages {

        stage('Checkout from GitHub') {
            steps {
                echo 'Checking out from GitHub...'
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/your-username/your-repo.git'
                echo 'Checkout successful!'
            }
        }

        stage('Test EC2 Connection') {
            steps {
                echo 'Testing EC2 connection...'
                sshagent(['ec2-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ec2-user@your-ec2-ip 'echo EC2 Connection Successful!'
                    """
                }
                echo 'EC2 connection successful!'
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

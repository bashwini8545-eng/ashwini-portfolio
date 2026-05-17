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
                        withCredentials([file(
                            credentialsId: 'ec2-ssh-key',
                            variable: 'KEY_FILE'
                        )]) {
                            bat """
                                copy "%KEY_FILE%" "%WORKSPACE%\\temp-key.pem"
                                icacls "%WORKSPACE%\\temp-key.pem" /reset
                                icacls "%WORKSPACE%\\temp-key.pem" /inheritance:r
                                icacls "%WORKSPACE%\\temp-key.pem" /remove "Everyone"
                                icacls "%WORKSPACE%\\temp-key.pem" /remove "BUILTIN\\Users"
                                icacls "%WORKSPACE%\\temp-key.pem" /grant:r "NT AUTHORITY\\SYSTEM:(R)"
                                ssh -o StrictHostKeyChecking=no -i "%WORKSPACE%\\temp-key.pem" %EC2_USER%@%EC2_HOST% "echo EC2 Connection Successful!"
                                del "%WORKSPACE%\\temp-key.pem"
                            """
                        }
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

pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'jdk17'
    }

    stages {

        stage('Checkout') {
            steps {
                git(
                    branch: 'main',
                    url: 'https://github.com/pmanju1616-bit/MavenApp-deployment-to-Ec2-with-JenkinsPipeline.git'
                )
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['deploy-creds']) {
                    bat '''
                        scp -o StrictHostKeyChecking=no target\\demo-1.0.0.jar ubuntu@54.91.75.247:/opt/app/

                        ssh -o StrictHostKeyChecking=no ubuntu@54.91.75.247 "pkill -f demo-1.0.0.jar || true"

                        ssh -o StrictHostKeyChecking=no ubuntu@54.91.75.247 "nohup java -jar /opt/app/demo-1.0.0.jar > /opt/app/app.log 2>&1 &"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully.'
        }

        failure {
            echo 'Deployment failed. Check Jenkins or EC2 logs.'
        }
    }
}

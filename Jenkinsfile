pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("643767682290.dkr.ecr.us-east-1.amazonaws.com/didi-netflix:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 643767682290.dkr.ecr.us-east-1.amazonaws.com/didi-netflix:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://643767682290.dkr.ecr.us-east-1.amazonaws.com/didi-netflix:latest') {
                    // build image
                    def myImage = docker.build("643767682290.dkr.ecr.us-east-1.amazonaws.com/didi-netflix:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}

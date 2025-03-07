pipeline{
    agent any
    stages{
        stage('CodeScan'){
            steps{
                sh 'trivy fs . -o file.txt'
                sh 'cat file.txt'

            }
        }
       stage('dockerBuild'){
            steps{
                sh 'docker build -t webapp .'
                sh 'docker build -t newversion .'
                sh 'docker images'

            }
        }
        stage('dockerlogin'){
            steps{
                sh 'aws ecr get-login-password --region us-east-1 | \
                docker login --username AWS \
                --password-stdin 296945066713.dkr.ecr.us-east-1.amazonaws.com'

            }
        }
        stage('dockerbuild'){
            steps{
                sh 'docker build -t pipelinerepo .'
                sh 'docker images'
            }
        }
        stage('dockerTag'){
            steps{
                sh 'docker tag pipelinerepo:latest 296945066713.dkr.ecr.us-east-1.amazonaws.com/pipelinerepo:latest'
                sh 'docker tag pipelinerepo:latest 296945066713.dkr.ecr.us-east-1.amazonaws.com/pipelinerepo:v1.$BUILD_NUMBER'
            }
        }
        stage('dockerPush'){
            steps{
                sh 'docker push 296945066713.dkr.ecr.us-east-1.amazonaws.com/pipelinerepo:latest'
                sh 'docker push 296945066713.dkr.ecr.us-east-1.amazonaws.com/pipelinerepo:v1.$BUILD_NUMBER'
            }
        }
        stage('Testing'){
            steps{
                sh 'docker run -itd --name ola -p 8000:80 pipelinerepo'
                
            }
        }
    }
}
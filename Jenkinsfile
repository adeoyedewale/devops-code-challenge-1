pipeline {
    agent any
  
    environment {
	    AWS_DEFAULT_REGION="us-east-1"
	    THE_BUTLER_SAYS_SO=credentials('aws-cred')
      }

    stages {
        stage('Checkout...') {
              steps {
                  git url: 'https://github.com/adeoyedewale/devops-code-challenge-1.git'
              }
         }
      

        stage("Build Backend Docker Image...") {
            steps {
              sh "docker build -t beta-backend:latest -f backend/Dockerfile ./backend"
            }
        }

        stage("Build Frontend Docker Image...") {
            steps {
                sh "docker build -t beta-frontend:latest -f frontend/Dockerfile ./frontend"
            }
        }
	
	    
       stage ('Publish to ECR...') {
            steps {
              sh '''
              aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/c6p1p1z3
        
              docker tag beta-frontend:latest public.ecr.aws/c6p1p1z3/beta-frontend:latest
              docker push public.ecr.aws/c6p1p1z3/beta-frontend:latest
              
              docker tag beta-backend:latest public.ecr.aws/c6p1p1z3/beta-backend:latest
              docker push public.ecr.aws/c6p1p1z3/beta-backend:latest
              '''
            }
          } 
    }
  
  post {
        always {
	          cleanWs()
        }
   }
}

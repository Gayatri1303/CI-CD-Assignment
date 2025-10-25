pipeline {
    agent {label 'agent-jenkins'}

    environment {
        AWS_ACCESS_KEY_ID = credentials('my-aws-creds_USR') 
        AWS_SECRET_ACCESS_KEY = credentials('my-aws-creds_PSW') 
        AWS_DEFAULT_REGION    = 'ap-south-1'
        
    }
    stages {


        stage('Terraform Init') {
            steps {
                sh 'terraform init -input=false'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan -input=false'
                sh 'terraform show -no-color tfplan > plan.txt'
            }
            
        }

        stage('Approval') {
            steps {
                input message: "Approve Terraform Apply?", ok: "Apply"
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -input=false tfplan'
            }
        }
    }

    post {
        success {
            echo 'Terraform apply succeeded!'
           
        }
        failure {
            echo 'Terraform failed!'
            
        }
    }



    
}

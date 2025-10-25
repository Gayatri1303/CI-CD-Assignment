pipeline {
    agent {label 'agent-jenkins'}

    environment {
        AWS_CREDENTIALS   = credentials('aws-creds-id')
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

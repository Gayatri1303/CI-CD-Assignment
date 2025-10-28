pipeline {
    agent {label 'agent-jenkins'}

    environment { 
        AWS_DEFAULT_REGION    = 'ap-south-1'
        FILE= credentials('credsaws')
        
    }
    stages {

        stage('AWS Login') {
            steps {
                
                sh 'aws configure import --csv file://${FILE}'
                sh 'aws configure list'
            
        }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init -input=false'
                sh 'aws configure list'
                }
            }
        

        stage('Terraform Plan') {
            steps {
                sh 'aws configure list'
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

    // post {
    //     success {
    //         echo 'Terraform apply succeeded!'
           
    //     }
    //     failure {
    //         echo 'Terraform failed!'
            
    //     }
    // }

    post {
    success {
        emailext(
            subject: '$DEFAULT_SUBJECT',
            body: '$DEFAULT_CONTENT',
            to: '$DEFAULT_RECIPIENTS'
        )
    }
    failure {
        emailext(
            subject: '$DEFAULT_SUBJECT',
            body: '$DEFAULT_CONTENT',
            to: '$DEFAULT_RECIPIENTS'
        )
    }
}


    

}
pipeline {
    agent {label 'agent-jenkins'}

    environment { 
        AWS_DEFAULT_REGION    = 'ap-south-1'
        FILE= credentials('credsaws')
        AWS_ACCESS_KEY= credentials('AWS_ACCESS_KEY')
        AWS_SECRET_KEY= credentials('AWS_SECRET_KEY')
    }
    stages {

        stage('AWS Login') {
            steps {
                
                // sh 'aws configure import --csv file://${FILE}'
                sh '''
                cat $FILE
                aws configure set aws_access_key_id  $AWS_ACCESS_KEY 
                aws configure set aws_secret_access_key $AWS_SECRET_KEY
                aws configure list

                '''
            
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
pipeline {
    agent {label 'agent-jenkins'}

    environment { 
        AWS_DEFAULT_REGION    = 'ap-south-1'
        
    }
    stages {


        stage('Terraform Init') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {
                sh 'terraform init -input=false'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {
                sh 'terraform plan -out=tfplan -input=false'
                sh 'terraform show -no-color tfplan > plan.txt'}
            }
            
        }

        stage('Approval') {
            steps {
                input message: "Approve Terraform Apply?", ok: "Apply"
            }
        }

        stage('Terraform Apply') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {
                sh 'terraform apply -input=false tfplan'
            }
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
            to: '$DEFAULT_RECEPIENTS'
        )
    }
    failure {
        emailext(
            subject: '$DEFAULT_SUBJECT',
            body: '$DEFAULT_CONTENT',
            to: '$DEFAULT_RECEPIENTS'
        )
    }
}


    

}
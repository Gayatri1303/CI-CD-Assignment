pipeline {
    agent {label 'agent-jenkins'}


    stages {

        stage('Set AWS Credentials') {
            
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'   
                ]]) {
                    sh 'aws sts get-caller-identity'
                }
            }
        }

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



    post {
        always {
            script {
                
                emailext(
                    subject: "Build ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: "The build has completed with status: ${currentBuild.currentResult}.",
                    recipientProviders: [[$class: 'DevelopersRecipientProvider']]
                )
            }
        }
    }
}

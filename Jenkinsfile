@Library('my-shared-lib') _
pipeline{

    agent any

    parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
        string(name: 'aws_account_id', description: " AWS Account ID", defaultValue: '295134731113')
        string(name: 'Region', description: "Region of ECR", defaultValue: 'eu-west-1')
        // string(name: 'ECR_REPO_NAME', description: "name of the ECR", defaultValue: 'ar7u4')
        // string(name: 'cluster', description: "name of the EKS Cluster", defaultValue: 'demo-cluster1')
    }
    environment{

        ACCESS_KEY = credentials('AWS_ACCESS_KEY_ID')
        SECRET_KEY = credentials('AWS_SECRET_KEY_ID')
        // ECR_REPO_URI = '295134731113.dkr.ecr.us-east-1.amazonaws.com/ar7u4'
    }
    stages{
         
        stage('Git Checkout'){
                    when { expression {  params.action == 'create' } }
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/ar7u4/terraform-tested-vpc.git"
            )
            }
        }
        stage('Create EKS Cluster : Terraform'){
            when { expression {  params.action == 'create' } }
            steps{
                script{

                      sh """
                          
                          terraform init 
                          terraform plan -var 'access_key=$ACCESS_KEY' -var 'secret_key=$SECRET_KEY'                       
                          terraform apply -var 'access_key=$ACCESS_KEY' -var 'secret_key=$SECRET_KEY' --auto-approve 
                          
                      """
                }
            }
        }
//         stage('Connect to EKS '){
//             when { expression {  params.action == 'create' } }
//         steps{

//             script{

//                 sh """
//                 aws configure set aws_access_key_id "$ACCESS_KEY"
//                 aws configure set aws_secret_access_key "$SECRET_KEY"
//                 aws configure set region "${params.Region}"
//                 aws eks --region ${params.Region} update-kubeconfig --name ${params.cluster}
//                 """
//             }
//         }
//         } 
//         stage('Deployment on EKS Cluster'){
//             when { expression {  params.action == 'create' } }
//             steps{
//                 script{
                  
//                   def apply = false

//                   try{
//                     input message: 'please confirm to deploy on eks', ok: 'Ready to apply the config ?'
//                     apply = true
//                   }catch(err){
//                     apply= false
//                     currentBuild.result  = 'UNSTABLE'
//                   }
//                   if(apply){

//                     sh """
//                       kubectl apply -f .
//                     """
//                   }
//                 }
//             }
//         } 

    }
}     


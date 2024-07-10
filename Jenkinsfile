pipeline{
    
    agent any 
    
    stages{
        
        stage('git checkout'){
            steps{
               git branch: 'main', url: 'https://github.com/rohan7739/terraform_module.git'
            }
        }
        stage('terraform init'){
            steps{
                sh 'terraform init'
            }
        }
        stage('terraform plan'){
            steps{
                sh 'terraform plan'
            }
        }
        stage('terraform apply'){
            steps{
                 sh 'terraform apply --auto-approve'
            }
        }
        stage('terraform destroy'){
            steps{
              sh 'terraform destroy --auto-approve'
            }
        }
        
    }
}

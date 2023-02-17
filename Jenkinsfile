pipeline { 
    agent any 
    environment { 
        AWS_ACCESS_KEY_ID     = credentials('aws_access_key_id') 
        AWS_SECRET_ACCESS_KEY = credentials('aws_security_key_id')
    }
    stages { 
        stage('Terraform Initialization') { 
            steps { 
                sh 'terraform init' 
                sh 'pwd' 
                sh 'ls -al' 
                sh 'printenv' 
            } 
        } 
        stage('Terraform Format') { 
            steps { 
                sh 'terraform fmt -check || exit 0' 
            } 
        } 
        stage('Terraform Validate') { 
            steps { 
                sh 'terraform validate'
            }       
        }
        stage('Terraform Planning') { 
            steps { 
                sh 'terraform plan -no-color -out=terraform_plan'
                sh 'terraform show -json ./terraform_plan > terraform_plan.json'
            } 
        }
        stage('archive terrafrom plan output') {
            steps {
                archiveArtifacts artifacts: 'terraform_plan.json', excludes: 'output/*.md', onlyIfSuccessful: true
            }
        }
        stage('Terraform Apply') { 
            steps { 
                sh 'terraform apply -auto-approve'
            } 
        }
        stage('Terraform Destroy') { 
            steps { 
                sh 'sleep 80'
                sh 'terraform destroy -auto-approve'
            }
        }
    }
}

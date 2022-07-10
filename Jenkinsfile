pipeline {
    agent any

    stages {
        stage('scm checkouto') {
            steps {
                //git branch: 'main', url: 'https://github.com/Yash-Yaadav/aks-terraform-latest.git'
                git branch: 'main', url: 'git@github.com:Yash-Yaadav/aks-terraform-latest.git'
            }
        }
    stage ('Terraform init') {
        steps {
            sh "terraform init"
        }
    }
    stage('Deploy aks with terraform') {
            steps {
                script {
                    withCredentials([azureServicePrincipal(credentialsId: 'azure', subscriptionIdVariable: 'SUBS_ID', clientIdVariable: 'CLIENT_ID', clientSecretVariable: 'CLIENT_SECRET', tenantIdVariable: 'TENANT_ID')]) {
                         sh 'az login --service-principal -u $CLIENT_ID -p $CLIENT_SECRET -t $TENANT_ID'
                         sh 'terraform init'
                         sh 'terraform plan -var client_id=$CLIENT_ID -var client_secret=$CLIENT_SECRET -var subs_id=$SUBS_ID -var tenant_id=$TENANT_ID'
                         //sh 'terraform apply --auto-approve -var client_id=$CLIENT_ID -var client_secret=$CLIENT_SECRET -var subs_id=$SUBS_ID -var tenant_id=$TENANT_ID'
                    }
                }
            }
        }
    }
}

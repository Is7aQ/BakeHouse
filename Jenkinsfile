pipeline { 
    agent any 
    stages {
          stage('Get Start with eks master'){
          steps{
            sh "aws eks --region eu-west-1 update-kubeconfig --name jeno"
            }
          }
          stage('Create Namespace dev'){
           steps{
            sh "~/kubectl create namespace dev"
           }
         }
          stage('Deploy deployment eks'){
           steps{
            sh "~/kubectl create -f deployment.yaml -n=dev"
           }
         }
        stage('launch loadbalancer service'){
           steps{
            sh "~/kubectl create -f service.yaml -n=dev"
           }
         }
        }  
      }

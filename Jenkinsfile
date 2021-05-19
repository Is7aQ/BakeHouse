List<String> CHOICES = ["test","dev","release"]
pipeline { 
   // properties([
   //    prameters([string name
   //  ])
   // ])
    environment { 
        registry = "is7aq/lap" 
        registryCredential = '1772ac8c-5089-4ab8-ba45-980aae019219' 
        dockerImage = '' 
    }
    agent any 
     parameters{
      choice(name: 'CHOICE' , choices: CHOICES, description: 'which branch to configure')
     }
    stages {
        
          stage('Set Credentials') {
             steps{
             withAWS(credentials: 'nti_Isaac') {
                 sh "aws eks --region eu-west-1 update-kubeconfig --name jeno"
               }
              }
             }
          stage('Set Kube Credentials') {
             steps{
              sh "~/kubectl get deployments"
              }
             }        
           }  
          stage('Build according to choosed branch'){
            steps{
              script{
                if(params.CHOICE == "release"){
                    git 'https://github.com/Is7aQ/BakeHouse.git'
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push()
                  } 
                }
                else if (params.CHOICE == "dev"){
                 sh "~/kubectl create ns dev"
                 sh "git checkout dev"
                 sh "~/kubectl apply -f deployment.yaml -n dev"
                 sh "~/kubectl apply -f service.yaml -n dev"
                }
                 else if(params.CHOICE == "test"){
                 sh "kubectl create ns test"
                 sh "git checkout test"
                 sh "kubectl apply -f deployment.yaml -n test"
                 sh "kubectl apply -f service.yaml -n test"  
                }
              }  
            }
          }     
        }
      }

pipeline {
    agent any
    
    tools{
        maven "M3"
    }    

    stages {
        stage('Git Clone') {
            steps {
               git branch: 'master', url: 'https://github.com/javapranay/01_products_api.git'
            }
        }
        stage('Maven Build'){
            steps{
             sh 'mvn clean package'
            }
        }
        stage('Docker Image Build'){
            steps{
             sh 'docker build -t javapranay/products_api .'
            }
        }
        stage('Docker Image Push'){
            steps{
                withCredentials([string(credentialsId: 'Docker-token', variable: 'secretPass')]) {
                    sh 'docker login -u javapranay -p ${secretPass}'
                    sh 'docker push javapranay/products_api'
                }
            }
        }
        stage('k8s deployment'){
            steps{
             sh 'kubectl apply -f Deployment.yml'
            }
        }        
    }
}

pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                git 'https://github.com/Darsht24/devops-cicd-ygminds.git'
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker buildx build -t darsht24/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u darsht24 -p ${dockerhubpwd}'

}
                   sh 'docker push darsht24/devops-integration'
                }
            }
        }
        stage('EKS and Kubectl configuration'){
            steps{
                script{
                    sh 'aws eks update-kubeconfig --region us-east-1 --name darsh-cluster'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    sh 'kubectl apply -f deploymentservice.yaml'
                }
            }
        }
    }
}

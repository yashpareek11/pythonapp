pipeline {
    
    agent any
    
    stages {
        stage('Git checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/yashpareek11/pythonapp.git']])
            }
        }
        stage('Build docker image') {
            steps {
                sh 'docker build -t yashpareek99/pythonapp-image .'
            }
        }
        stage('Push docker image on dockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u yashpareek99 -p ${dockerhubpwd}'
                    sh 'docker push yashpareek99/pythonapp-image'
                }
            }
        }
        stage('Deployment on K8s') {
            steps {
                kubernetesDeploy (configs: 'deployment.yaml,service.yaml', kubeconfigId: 'k8spwd')
            }
        }
    }
}
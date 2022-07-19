pipeline {
    agent any 
    environment {
        registryCredential = 'dockerhub_id'
        imageName = 'external'
        dockerImage = ''
        
        
        }
    stages {
        stage('Run the tests') {
             agent {
                docker { 
                    image 'node:18-alpine'
                    args '-e HOME=/tmp -e NPM_CONFIG_PREFIX=/tmp/.npm'
                    reuseNode true
                    
                }
            }
            steps {
                echo 'Retrieve source from github' 
                git branch: 'test',
                    url: 'https://github.com/jayram98/extrenel02.git'

                echo 'showing files from repo?' 
                sh 'ls -a'
                echo 'install dependencies' 
                sh 'npm install'
                echo 'Run tests'
                sh 'npm test'
                echo 'Testing completed'
            }
        }
        stage('Building image') {
            steps{
                script {
                    echo 'building image' 
                    dockerImage = docker.build("${env.imageName}:${env.BUILD_ID}")
                    echo 'image built'
                }
            }
        }
 		stage('Login') {

			steps {
				script{
                    docker.withRegistry('',registryCredential){
                        dockerImage.push("${env.BUILD_ID}")
                    }
                }
			}
		}           
         stage ('K8S Deploy') {
        steps {
            script {
		//git clone 'https://github.com/jayram98/extrenel02.git'   
                kubernetesDeploy(
                    configs: 'k8s-deployment.yaml',
                    kubeconfigId: 'aks',
                    enableConfigSubstitution: true
                    )      
        stage('push image') {
            steps{
                script {
                    echo 'image push'
                    sh'echo ' 
                    
                }
            }
            }    
  
     
   

    }
}
	   

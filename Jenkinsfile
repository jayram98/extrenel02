pipeline {
    agent any 
    environment {
        //registryCredential = 'jay998'
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
		    //sudo su	
                    echo 'building image' 
                    //dockerImage = docker.build("${env.imageName}:${env.BUILD_ID}")
		    //docker image tag '${dockerImage}' '${env.BUILD_ID}'	
			
                    echo 'image built'
                }
            }
        }

          steps{
                      script{
                      withSonarQubeEnv('sonarserver') { 
                      sh "mvn sonar:sonar"
                       }
                      timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
		    sh "mvn clean install"
                  }
                }  
              }


        


        
 		stage('Login') {

			steps {
				script{
               //docker.withRegistry('',registryCredential)
		withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerpassword')])
					
					{
	        //sh 'docker build -t  .
		sh "docker login -u 'jay899' -p ${dockerpassword} "       
                sh "docker build -t jay899/external:${env.BUILD_ID} ."		    
		sh "docker images"
	        sh "docker login docker.io"
  		sh "docker push jay899/external:${env.BUILD_ID}"
		    //sh 'docker push external:${env.BUILD_ID}' 
	            //sh 'docker push external:${env.BUILD_ID}'
	            
                    //dockerImage.push(jay899/'${env.BUILD_ID}')
		    
		    //docker push ('${env.BUILD_ID}')}		
                    
                    //docker login --username='${dockerHubUser}' --password='${dockerHubPassword}'
                    
                }
			}
		}           
		}
	stage ('K8S Deploy') {
        steps {
            script {
		    sh "ls -ltha"
		    //sh kubectl apply -f "k8s-deployment.yaml"
                kubernetesDeploy(
		    	
                    configs: 'k8s-deployment.yaml,external-load-balancer.yaml,external-deployment.yaml',
                    kubeconfigId: 'aks',
                    enableConfigSubstitution: true
		)}
	}
	}
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
    

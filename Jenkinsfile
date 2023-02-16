pipeline{
    agent any
    environment{
        DOCKERHUB_USERNAME = "prad72"
        APP_NAME = "nodejs_app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
        }
    stages{
        stage('git'){
            steps{
                git url: 'https://github.com/pradyumna93/node-todo-cicd.git', branch: 'master' 
            }

        }
        stage('Building image') {
            steps{
                script {
                    docker_Image = docker.build "${IMAGE_NAME}"
                }
            }
        }    
        stage('Deploy Image') {
            steps{
                script {
                    docker.withRegistry( '', REGISTRY_CREDS ){
                        docker_Image.push("$BUILD_NUMBER")
                        docker_Image.push('latest')
                        
                    }
            
                      
                }   
            }
            
        
        }
        stage('K8S Deploy'){
            steps{
                script{
                    withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                    sh "kubectl apply -f deployment.yaml"
                    }
                }
            }    
        }
    
    }
    
}

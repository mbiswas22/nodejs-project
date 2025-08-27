pipeline { 
agent any 
environment { 
        IMAGE_NAME = 'hello-world-node' 
        IMAGE_TAG = 'v1.0' 
    } 
 
    stages { 
        stage('Checkout Code') { 
            steps { 
                git branch: 'main', 
                    url: 'https://github.com/mbiswas22/nodejs-project.git' 
            } 
        } 
 
        stage('Build Docker Image') { 
            steps { 
                script { 
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}") 
                } 
            } 
        } 

        stage('Pushing Docker Image') { 
            environment {
                registryCredential = 'dockerLogin' // Need to add credential
            }
            steps { 
                script { 
                    docker.withRegistry('http://registry.hub.docker.com/', registryCredential){
                       dockerImage.push("latest") 
                     } 
                } 
            } 
        } 
 
        stage('Deploy to Kubernetes') { 
            steps { 
                script { 
                    bat 'kubectl apply -f k8s-deployment.yaml' 
                    bat 'kubectl apply -f k8s-service.yaml' 
                } 
            } 
        } 
    } 
 
    post { 
        success { 
            echo 'Pipeline completed successfully!' 
        } 
    } 
} 
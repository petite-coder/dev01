pipeline { 
    agent any 
 
    stages { 
        stage('Git Checkout') { 
            steps { 
                git branch: 'main', url: 'https://github.com/petite-coder/dev01.git' 
            } 
        } 
 
        stage('Build Docker Image') { 
            steps { 
                sh 'docker build -t yennies/dev01:1.0 .' 
            } 
        } 
 
        stage('Push to DockerHub') { 
            steps { 
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 
'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) { 
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin' 
                    sh 'docker push yennies/dev01:1.0' 
                } 
            } 
        } 
 
        stage('Deploy to Kubernetes') { 
            steps { 
		sh 'kubectl apply -f dev01-deploy.yaml' 
		sh 'kubectl apply -f dev01-svc.yaml' 
	    } 
	} 
    } 
} 

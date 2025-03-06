pipeline{
	agent any
	environment{
		DOCKER_IMAGE = "kpkm25/my-jenkins-app"

	}
	stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."
                git url: 'https://github.com/KPkm25/jenkins-docker.git', branch: 'main'
                echo "Repository cloned successfully."
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Starting Docker build..."
                sh "docker build -t ${DOCKER_IMAGE} ."
                echo "Docker image built successfully."
            }
        }

	stage('Run Container Locally'){
		steps {
			script {
				sh "docker run -d -p 8081:80 --name jenkins-docker ${DOCKER_IMAGE}:latest"
			}
		}
}

        stage('Push Docker Image') {
            steps {
                script {
                    echo "Logging into Docker Hub..."
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-creds') {
                        echo "Pushing Docker image..."
                        sh "docker push ${DOCKER_IMAGE}"
                        echo "Docker image pushed successfully."
                    }
                }
            }

        }
	stage('Deploy to server'){
		steps{
			script{
				sh "sshe master@master-vm'docker pull ${DOCKER_IMAGE}:latest && docker run -d -p 8081:80 ${DOCKER_IMAGE}:latest'"
			}
		}
}
    }
}

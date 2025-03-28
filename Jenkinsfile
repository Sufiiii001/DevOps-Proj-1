pipeline {
    agent any

    environment {
        NEXUS_URL = "http://10.227.141.103:8081"
        NEXUS_REPO = "maven-releases"
        // DOCKER_HUB_USER = "your-docker-hub-username"
        IMAGE_NAME = "spring-boot-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sufiiii001/DevOps-Proj-1'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        // stage('SonarQube Analysis') {
        //     steps {
        //         withSonarQubeEnv('SonarQube') {
        //             sh 'mvn sonar:sonar'
        //         }
        //     }
        // }

        // stage('Publish Artifact to Nexus') {
        //     steps {
        //         sh '''
        //         mvn deploy \
        //             -DaltDeploymentRepository=nexus::default::${NEXUS_URL}/repository/${NEXUS_REPO}/ \
        //             -DrepositoryId=nexus
        //         '''
        //     }
        // }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t $IMAGE_NAME .'
            }
        }

        // stage('Push to Docker Hub') {
        //     steps {
        //         withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
        //             sh 'docker push $IMAGE_NAME'
        //         }
        //     }
        // }
    }
}

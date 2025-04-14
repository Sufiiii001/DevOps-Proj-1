pipeline {
    agent any

    environment 
    {
        NEXUS_URL = "http://10.227.141.96:8081"
        NEXUS_REPO = "maven-releases"
        // DOCKER_HUB_USER = "your-docker-hub-username"
        IMAGE_NAME = "spring-boot-app:latest"
    }

    stages 
    {

        stage('Login to Nexus') {
            steps {
withCredentials([usernamePassword(credentialsId: 'nexus-docker-creds', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
            sh '''
                echo "$NEXUS_PASS" | docker login http://10.227.141.120:12001 --username "$NEXUS_USER" --password-stdin
            '''
        }
            }
        }
        stage('Checkout') 
        {
            steps 
            {
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


        

        stage('Build Docker Image') 
        {
            steps 
            {
                sh 'sudo docker build -t $IMAGE_NAME .'
            }
        }

                stage('Publish Artifact to Nexus') 
        {
    steps 
    {
        sh '''
docker build -t demo-app:v1 .
docker tag demo-app:v1 10.227.141.96:12001/mydocker/demo-app:v1
docker push 10.227.141.96:12001/mydocker/demo-app:v1
        '''
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

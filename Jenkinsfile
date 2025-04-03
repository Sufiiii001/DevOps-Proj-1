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
                script {
                    def loginResponse = sh(
                        script: """
                        curl -u admin:cityreal -s -o /dev/null -w "%{http_code}" $NEXUS_URL/service/rest/v1/status
                        """,
                        returnStdout: true
                    ).trim()

                    if (loginResponse == "200") {
                        echo "✅ Login Successful: Done"
                    } else {
                        error "❌ Login Failed! Status Code: $loginResponse"
                    }
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

        stage('Publish Artifact to Nexus') 
        {
    steps 
    {
        sh '''
        mvn deploy \
            -DaltDeploymentRepository=nexus::default::http://10.227.141.96:8081/repository/maven-releases/ \
            -DrepositoryId=admin \
            -Dnexus.user=admin \
            -Dnexus.pass=cityreal
        '''
    }
}

        

        stage('Build Docker Image') 
        {
            steps 
            {
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

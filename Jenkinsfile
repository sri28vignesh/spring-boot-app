pipeline {
    agent any

    stages {
        stage('Maven Build') {
            agent {
                docker {
                    image 'maven:3.8.1-adoptopenjdk-11' 
                    args '-v $HOME/.m2:/root/.m2'
                    // Run the container on the node specified at the
                    // top-level of the Pipeline, in the same workspace,
                    // rather than on a new node entirely:
                    reuseNode true 
                    }
                } 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }

        stage('Docker Build'){
            steps{
                sh 'docker build -t spring-boot-app .'
                sh 'docker tag spring-boot-app spring-boot-app:$BUILD_NUMBER'
                sh 'docker tag spring-boot-app spring-boot-app:latest'
            }
        }

        stage('Docker push to ECR'){
            steps{
                
            }
        }


    }
}
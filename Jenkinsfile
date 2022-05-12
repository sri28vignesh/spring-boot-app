pipeline {
    agent any

    stages {
        stage('Git Tag'{
            steps{

                sh 'git tag -a build-$BUILD_NUMBER'

                sshagent(['git-ssh']) {
                    sh("git push --tags")
                }
            }
        })
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
                sh 'docker build -t sri-training-spring-app .'
                sh 'docker tag sri-training-spring-app:latest sri-training-spring-app:build-$BUILD_NUMBER'
                sh 'docker tag sri-training-spring-app:latest 590852515231.dkr.ecr.us-east-1.amazonaws.com/sri-training-spring-app:latest'
            }
        }

        stage('Docker push to ECR'){
            steps{
                    withDockerRegistry( [ credentialsId: "ecr:us-east-1:aws-user", url: "https://590852515231.dkr.ecr.us-east-1.amazonaws.com" ] ){
                        sh 'docker push 590852515231.dkr.ecr.us-east-1.amazonaws.com/sri-training-spring-app:build-$BUILD_NUMBER'
                        sh 'docker push 590852515231.dkr.ecr.us-east-1.amazonaws.com/sri-training-spring-app:latest'
                    }
            }
        }


    }
}
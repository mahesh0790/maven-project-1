pipeline{
    agent any
    stages{
        stage("scm checkout"){
            steps{
                git "https://github.com/mahesh0790/maven-project.git"
            }
        }
        stage("verify & unit tests"){
            steps{
                sh "mvn clean verify -DskipITs=true"
            }
        }
        stage("verify& ITs"){
            steps{
                sh "mvn clean verify -Dsurefire.skip=true"
            }
        }
        stage("build the docker image"){
            steps{
                sh "sudo cp /var/lib/jenkins/workspace/ci_cd_push_pull_docker/webapp/target/webapp.war /home/mahesh/Videos/Practices/maven-project/docker"
                sh "docker build -t mahesh0790/${JOB_NAME}:${BUILD_NUMBER} ./docker"
            }
        }
        stage("push the image into dockerhub"){
            steps{
                withDockerRegistry(credentialsId: 'docker-registry', url: 'https://index.docker.io/v1/') {
            sh "docker push mahesh0790/${JOB_NAME}:${BUILD_NUMBER}"
            }
        }
        }
        stage("ssh connection to ec2"){
            steps{
                sshagent(['ssh-docker']) {
            sh "ssh -o StrictHostKeyChecking=no ubuntu@52.66.81.4 docker pull mahesh0790/${JOB_NAME}:${BUILD_NUMBER} "
            }
            }
        }
            stage("ssh connection to ec2-1"){
            steps{
                sshagent(['ssh-docker']) {
            sh "ssh -o StrictHostKeyChecking=no ubuntu@52.66.81.4 docker run -d -p 8080:8080 mahesh0790/${JOB_NAME}:${BUILD_NUMBER} "

            }
        }
    }
        }
    }




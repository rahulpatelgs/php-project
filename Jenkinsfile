pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/rahulpatelgs/php-project', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t $JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rahulpatel123/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rahulpatel123/$JOB_NAME:latest'
                    sh 'docker images'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push rahulpatel123/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker push rahulpatel123/$JOB_NAME:latest'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe-1 -p 8085:80 rahulpatel123/$JOB_NAME:v1.$BUILD_ID'
                    sshagent(['ssh-keypair-php']) {
                        //sh "sudo docker rm -f My-first-containe33"
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe211 -p 8082:80 $JOB_NAME:v1.BUILD_ID"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@54.167.179.238 ${dockerCmd}"
                    }
                }
            }
        }
    }
}

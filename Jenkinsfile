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
                    sh 'docker build -t rahulpatel123/3febimg:v2 .'
                    sh 'docker images'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push rahulpatel123/3febimg:v2'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe221 -p 8083:80 rahulpatel123/3febimg:v2'
                    sshagent(['ssh-keypair']) {
                        sh "docker rm -f My-first-containe221"
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe211 -p 8082:80 rahulpatel123/3febimg:v2"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@34.238.116.168 ${dockerCmd}"
                    }
                }
            }
        }
    }
}

pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/DileepMSGIT/php-project/', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t dileepmsdocker/phpprojectimg:v1 .'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push dileepmsdocker/phpprojectimg:v1'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe21 -p 8081:80 dileepmsdocker/phpprojectimg:v1'
                    sshagent(['sshkeypair']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe211 -p 8082:80 akshu20791/phpprojectimg:v1"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.42.101 ${dockerCmd}"
                    }
                }
            }
        }
    }
}

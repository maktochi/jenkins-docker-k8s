pipeline {
   agent  any
   environment {
       DOCKER_TAG = getDockerTag()
    }
    stages{

            stage('Build Docker Image'){
                    steps{
                           sh "sudo  docker build  .  -t  maktochi/nodeapp:${DOCKER_TAG}"
                    }
             }
             stage('DockerHub Push'){
                     steps{
                           withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'dockerHubPwd')]){
                                 sh "docker login -u maktochi -p ${dockerHubPwd}"
                                 sh "docker push docker.io/maktochi/nodeapp:${DOCKER_TAG}"
                                 }
                           }
             }
             stage('Deploy to k8s'){
                    steps{
                            sh  "chmod  +x  changeTag.sh"
                            sh  "./changeTag.sh  ${DOCKER_TAG}"
                            sshagent(['k8s-machine']){
                                sh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml  devops@10.100.9.51:/home/devops/"
                         //     script{
                         //             try{
                         //                   sh "ssh  devops@10.100.9.51 kubectl apply -f ."
                         //                 }catch(error){
                         //                   sh "ssh  devops@10.100.9.51  kubectl create -f ."
                         //                 }
                         //     }
                            }
                    }
             }
      }
}

def  getDockerTag() {
  def tag = sh  script: 'git rev-parse HEAD', returnStdout: true
  return  tag
}


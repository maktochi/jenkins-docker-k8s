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
      }

}

def  getDockerTag() {
  def tag = sh  script: 'git rev-parse HEAD', returnStdout: true
  return  tag
}


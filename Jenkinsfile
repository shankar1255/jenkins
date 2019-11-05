pipeline {
  environment {
    registry = "durgaprasad/pipelineproject"
    registryCredential = 'dockerhub'
    dockerImage = ''
	
  }
  agent any
  stages {
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
	 stage('Deploy App') {
      steps {
        script {
		sh "chmod +x changeTag.sh"
        sh "./changeTag.sh $BUILD_NUMBER"
        kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
  }
}
}

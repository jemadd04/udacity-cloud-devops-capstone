pipeline{
  agent any
  stages{
    stage('Lint blue HTML & green HTML'){
        steps{
            sh 'tidy -q -e ./blue/*.html'
            sh 'tidy -q -e ./green/*.html'
      }
    }
    stage('Build blue & green images'){
        steps {
          withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
            sh "docker build -f blue/Dockerfile -t jamesmaddox/bluedeploy ."
            sh "docker build -f green/Dockerfile -t jamesmaddox/greendeploy ."
          }
      }
    }
    stage('Push blue & green images'){
      steps{
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
          sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
          sh "docker push jamesmaddox/bluedeploy"
          sh "docker push jamesmaddox/greendeploy"
        }
      }
    }
    stage('Set current kubectl context'){
      steps {
				withAWS(region:'us-east-2', credentials:'aws-credentials') {
					sh '''
            aws eks --region us-east-2 update-kubeconfig --name jmcapstone
						kubectl config use-context arn:aws:eks:us-east-2:218943385380:cluster/jmcapstone
					'''
				}
			}
    }
    stage('Deploy blue & green container'){
      steps{
        withAWS(region:'us-east-2', credentials:'aws-credentials') {
					sh '''
            kubectl apply -f ./blue/blue-controller.json
            kubectl apply -f ./green/green-controller.json
					'''
				}
      }
    }
    stage('Confirm traffic redirect'){
      steps{
        input "Please provide confirmation to redirect traffic to other deployment"
      }
    }
    stage('Create cluster service and redirect'){
      steps{
        withAWS(region:'us-east-2', credentials:'aws-credentials') {
					sh '''
            kubectl apply -f ./blue-green-service.json
					'''
				}
      }
    }
  }    
}
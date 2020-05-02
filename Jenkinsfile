pipeline{
  agent any
  stages{
    stage('Linting1'){
        steps{
            sh 'tidy -q -e ./blue/*.html'
            sh 'tidy -q -e ./green/*.html'
      }
    }
    stage('Build image'){
        steps {
          withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
            sh "docker build -f blue/Dockerfile -t jamesmaddox/bluedeploy ."
            sh "docker build -f green/Dockerfile -t jamesmaddox/greendeploy ."
          }
      }
    }
    stage('Push image'){
      steps{
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
          sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
          sh "docker push jamesmaddox/bluedeploy"
          sh "docker push jamesmaddox/greendeploy"
        }
      }
    }
    // stage('Set current kubectl context'){
    //   steps{
    //         //
    //   }
    // }
    // stage('Deploy container'){
    //   steps{
    //         //
    //   }
    // }
  }    
}
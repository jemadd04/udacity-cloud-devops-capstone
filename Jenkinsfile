pipeline{
  agent any
  stages{
    stage('Linting1'){
        steps{
            sh 'tidy -q -e ./blue/*.html'
            sh 'tidy -q -e ./green/*.html'
      }
    }
    stage('Build images'){
        steps {
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
                sh '''
                    docker build -t jamesmaddox/greendeploy .
                '''
        }
    }
    }
    // stage('Push image'){
    //   steps{
    //         //
    //   }
    // }
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
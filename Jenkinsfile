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
          script {
            dockerImage = docker.build("jamesmaddox/bluedeploy")
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
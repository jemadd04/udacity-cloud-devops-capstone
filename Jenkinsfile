pipeline{
  agent any
  stages{
    stage('Linting'){
      steps{
            sh 'tidy -q -e ./blue/*.html'
            sh 'tidy -q -e ./green/*.html'
      }
    }
    // stage('Build image'){
    //   steps{
    //         //
    //   }
    // }
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
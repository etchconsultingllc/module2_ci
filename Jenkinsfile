pipeline{
  agent any
  stages{
    stage{
      steps{
      checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-check', url: 'https://github.com/etchconsultingllc/module2_ci']]])  
      }
    }
    stage('etech-hello'){
      steps{
        sh 'git version'
      }
    }
  }
  
 }

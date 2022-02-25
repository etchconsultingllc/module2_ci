pipeline{
  agent any 
  tools { maven 'maven'}
  stages{
    stage('git-clone'){
      steps{
       checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-check', url: 'https://github.com/etchconsultingllc/module2_ci']]]) 
      }
    }
    stage('etech-hello'){
      steps{
        sh 'git version'
        sh 'mvn -v'
      }
    }
   stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
   }
   stage('Unit Tests - JUnit and JaCoCo') {
      steps {
        sh 'mvn test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }
   stage('Mutation Tests - PIT') {
      steps {
        sh "mvn org.pitest:pitest-maven:mutationCoverage"
      }
      post {
        always {
          pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
        }
      }
    }
    stage('CodeQuality-SAST'){
      steps{
        sh 'mvn clean verify sonar:sonar \
  -Dsonar.projectKey=mypipeline \
  -Dsonar.host.url=http://devopsdemo.eastus.cloudapp.azure.com:9000 \
  -Dsonar.login=3fe499802026f9cc8213d0fb98f9bc6c7dc4c17b'
      }
    }
  }    
}

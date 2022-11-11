pipeline {
  agent any
  stages {
    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        //change from archive to Artifacts
        archiveArtifacts 'target/*.jar'
      }
    }
    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
     }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
   }
    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: 'dockerhubcre', url: '']) {
        sh 'printenv'
        sh 'docker build -t mide2020/numeric-app:""$GIT_COMMIT"" .'
        sh 'docker push mide2020/numeric-app:""$GIT_COMMIT""'
       }
      }
    }
//     stage('Kubernetes Deployment - DEV') {
//       steps{
//         withKubeConfig([credentialsId: 'kubeconfig']) {
//           sh "sed -i 's#replace#mide2020/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
//           sh 'kubectl apply -f k8s_deployment_service.yaml'
//         }
//       }
//     }
  }
 }
// script test
pipeline {
  agent { label 'linux_deb' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('lorconksu-dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh "docker build -t lorconksu/cicdpipelinewithjenkinsproject1:${env.BUILD_ID} ."
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
        sh 'echo $JOB_NAME'
      }
    }
    stage('Push') {
      steps {
        sh "docker push lorconksu/cicdpipelinewithjenkinsproject1:${env.BUILD_ID}"
      }
    }
    stage('Deploy to OCP') {
      steps {
        sh "oc set image deployment.apps/CICD-Pipeline-training-project1 CICDPipelineWithJenkinsProject1=lorconksu/cicdpipelinewithjenkinsproject1:${env.BUILD_ID} --kubeconfig /var/lib/jenkins/kubeconfig"
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
pipeline {
  agent { 
    label 'kit'
  }
  environment {
    DH_CREDS=credentials('dh-creds')
    REGISTRY_SERVER='docker.io'
    REGISTRY_ORG='darinpope'
    MODEL_NAME='mymodelkit'
    MODEL_TAG='latest'
  }
  stages {
    stage('verify kit installation') {
      steps {
        sh 'kit version'
      }
    }
    stage('Remove all locally stored modelkits') {
      steps {
        sh 'kit remove --all --force'
      }
    }
    stage('verify the model is removed') {
      steps {
        sh 'kit list'
      }
    }
    stage('pack the ModelKit') {
      steps {
        sh 'kit pack . -t $REGISTRY_SERVER/$REGISTRY_ORG/$MODEL_NAME:$MODEL_TAG'
      }
    }
    stage('make sure the model exists') {
      steps {
        sh 'kit list'
      }
    }
    stage('login') {
      steps {
        sh 'echo $DH_CREDS_PSW | kit login docker.io --username=$DH_CREDS_USR --password-stdin'
      }
    }
    stage('push the model to DockerHub') {
      steps {
        sh 'kit push $REGISTRY_SERVER/$REGISTRY_ORG/$MODEL_NAME:$MODEL_TAG'
      }
    }
  }
  post {
    always {
      sh 'kit logout docker.io'
    }
  }
}
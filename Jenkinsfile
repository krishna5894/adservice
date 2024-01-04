pipeline {
  agent {
    kubernetes {
      label 'sample-app'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  containers:
  - name: gcloud-kubectl-docker
    image: gcr.io/disco-domain-402111/gcloud-kubectl-docker
    command:
    - cat
    tty: true
"""
}
  }
  stages {
    stage('Build and push image with Container Builder') {
      steps {
        container('gcloud-kubectl-docker') {
          sh "gcloud auth activate-service-account --key-file=disco-domain-402111-e9f624083714.json"
          sh "gcloud config set project disco-domain-402111"
          sh "PYTHONUNBUFFERED=1 gcloud builds submit -t gcr.io/disco-domain-402111/adservice ."
        }
      }
   }
    stage('deployment'){
      steps{
        container('gcloud-kubectl-docker'){
          sh"gcloud container clusters get-credentials cluster-2 --zone us-east1-b --project disco-domain-402111"
          sh"kubectl apply -f adservice.yaml"
}
}

      }
    }
  }
